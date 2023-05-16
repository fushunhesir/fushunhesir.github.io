---
layout: wiki
wiki: MIT-6.S081
title: 调度
order: 8
---

操作系统运行中同时运行的进程通常比核心的数量多，那么这些进程是怎样“同时”运行的呢？操作系统给这些进程提供了怎样的抽象呢？操作系统如何实现这个抽象的呢？

## 复用

`xv6`在两种情况下会发生进程切换，第一种是当`IO`操作完成时，存在等待它的进程，`sleep-wakeup`机制会进行进程切换；第二种是一个进程的时间片到时。

这样就给每一个进线提供了CPU的抽象。宏观上来看，似乎每个线程都有自己独占的CPU，就类似于页表使每个进程仿佛有自己的独有内存一样。

在设计这样的一个抽象的时候。我们需要考虑一些问题：

* 如何实现一个进程切换到另外一个进程？虽然其上下文切换的思想很简单，但是代码写起来却相当麻烦。
* 如何实现切换进程的同时用户无法感知？在`xv6`中使用定时器来实现的。
* 如何避免就绪进程队列的竞争条件问题？多个CPU会竞争就绪队列的进程，需要用`lock`来解决。
* 如何对已结束进程的所有资源进行回收？虽然进程本身可以释放部分资源，但是它无法释放一些自己的核心资源，比如内核堆栈。

在`Linux`中，一个进程可能拥有多个线程，这些线程共享内存。在`xv6`中有内核线程的概念，因此在`xv6`内核中，线程是共享内存的。但在用户空间中一般是一个进程中只有一个线程，因此用户空间中的线程可以视为相互隔离的。

## 上下文切换

<img src="https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/%E5%A4%8D%E7%94%A8.png" alt="image-20230515211525260" style="zoom:25%;" />

线程切换的大致流程是用户程序因为中断或者系统调用进入内核，然后该进程的内核线程切换到`scheduler`线程，再由`scheduler`线程从就绪列表选择合适的新内核线程进行切换。其中`scheduler`线程是依赖于CPU的，也就是说每个CPU都维护了一个自己的`scheduler`。

线程的切换主要包括寄存器的保存和恢复，包括`PC`和堆栈指针的保存和恢复。在`swtch`中我们保存了当前线程的寄存器，并载入新线程的寄存器。并没有保存`PC`而是保存`RA`，这是调用`swtch`的地址。

`scheduler`线程是依赖于CPU的并且一直运行`scheduler()`函数的专用线程。

需要注意的是，并非所有的线程切换都是时间片用尽导致的，线程结束调用`exit()`也能导致线程切换。

## 代码流程

```c
struct proc {
  struct spinlock lock;

  // p->lock must be held when using these:
  enum procstate state;        // Process state
  ...

  // these are private to the process, so p->lock need not be held.
  uint64 kstack;               // Virtual address of kernel stack
  ...
  struct trapframe *trapframe; // data page for trampoline.S
  struct context context;      // swtch() here to run process
  ...
};
```

上面是线程切换需要用到的一些字段。其中`state`保存了进程的状态；`kstack`保存了进程的内核堆栈指针；`trapframe`保存了用户线程的寄存器；`lock`保护了很多地方线程的一致性；`context`保存了内核线程的寄存器状态。

想要区分不同进程的内核线程可以通过一些方式：第一，利用内核堆栈的不同来判断，因为每个用户进程都有自己的内核堆栈；第二，则是通过`myproc()`函数来判断，该函数通过读取`tp`寄存器的值来确定当前的CPU编号，同时利用一个记录所有CPU上运行的进程的数组来判断。



```C
// Give up the CPU for one scheduling round.
void
yield(void)
{
  struct proc *p = myproc();
  acquire(&p->lock);
  p->state = RUNNABLE;
  sched();
  release(&p->lock);
}
```

`yield()`之所以要获取进程锁是为了避免不一致状态。如果没有申请锁，那么当`p->state`修改为`RUNABLE`后，此时进程的状态表示其不在运行，但是实际上其仍然在接着运行`sched()`函数，若此时另外一个CPU选择运行该线程，那么此时就有两个CPU在同一个个堆栈上运行了。

```asm
.globl swtch
swtch:
        sd ra, 0(a0)
        sd sp, 8(a0)
        sd s0, 16(a0)
        sd s1, 24(a0)
        sd s2, 32(a0)
        sd s3, 40(a0)
        sd s4, 48(a0)
        sd s5, 56(a0)
        sd s6, 64(a0)
        sd s7, 72(a0)
        sd s8, 80(a0)
        sd s9, 88(a0)
        sd s10, 96(a0)
        sd s11, 104(a0)

        ld ra, 0(a1)
        ld sp, 8(a1)
        ld s0, 16(a1)
        ld s1, 24(a1)
        ld s2, 32(a1)
        ld s3, 40(a1)
        ld s4, 48(a1)
        ld s5, 56(a1)
        ld s6, 64(a1)
        ld s7, 72(a1)
        ld s8, 80(a1)
        ld s9, 88(a1)
        ld s10, 96(a1)
        ld s11, 104(a1)
        
        ret
```

这里需要注意的是，`xv6`中明明有32个寄存器，而这里只保存了14个。这是因为当线程调用`sched()`函数的时候，是以函数调用的方式进行的，因此部分寄存器已经在内核堆栈中保存，并且在函数返回的时候会自动恢复，所以不用专门去做保存。

```C
void
scheduler(void)
{
  struct proc *p;
  struct cpu *c = mycpu();
  
  c->proc = 0;
  for(;;){
    // Avoid deadlock by ensuring that devices can interrupt.
    intr_on();

    for(p = proc; p < &proc[NPROC]; p++) {
      acquire(&p->lock);
      if(p->state == RUNNABLE) {
        // Switch to chosen process.  It is the process's job
        // to release its lock and then reacquire it
        // before jumping back to us.
        p->state = RUNNING;
        c->proc = p;
        swtch(&c->context, &p->context);

        // Process is done running for now.
        // It should have changed its p->state before coming back.
        c->proc = 0;
      }
      release(&p->lock);
    }
  }
}
```

