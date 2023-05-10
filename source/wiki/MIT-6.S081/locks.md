---
layout: wiki
wiki: MIT-6.S081
title: 锁机制
order: 7

---

锁存在的目的是解决并发问题。操作系统之所以面对并发问题，是因为物理硬件的共享。现代的计算机一般是多核心，可以独立执行指令，他们共享着物理内存，因此存在着对同一块内存，一颗核心进行读，另外一颗核心进行写的情况。甚至多个核心同时写的情况。除此之外，即使只有一颗核心也需要应对多个线程切换，交错运行的场景。

*并发是指由于多核并行、线程切换或中断导致的多指令流交替执行的情况*

内核设计会允许大量的并发，以此来提高系统的性能。由此催生了以并发下正确性为目的的**并发控制技术**。

*锁提供互斥，只有持有锁的CPU能够对该数据区进行操作，确保了对数据操作的正确性。但是它会限制系统性能，因为锁会序列化并发操作。*

## 竞争条件

竞争条件就是同时对内存进行访问，并且其中至少有一个写操作。这样的情况通常会导致错误，并且难以调试。因为，添加一个打印语句从而增加的时间就可能消除了竞争条件。

**解决竞争条件的方式一般就是加锁。**

```C
struct element *list = 0;
struct lock listlock;

void
push(int data) {
	struct element *l;
	l = malloc(sizeof *l);
	l->data = data;
	acquire(&listlock);
	l->next = list;
	list = l;
	release(&listlock);
}

```

这里`acquire()`和`release()`之间的区域也称为**临界区**。

通常所说的锁保护了数据，实际上是锁保护了对象的不变性质。比如在上述的`push()`中，`list`的性质是始终指向列表的表头，但是当执行完`l->next = list`后，这个不变性被暂时破坏了，如果有其他依赖于这个不变特性的指令在另外一个CPU并发执行，就会引起错误。所以锁实际上是保护了这样一种不变特性。

**锁虽然能够保证代码的正确性，但是同时也会损失性能，从而失去多CPU运行的优势**。同时，如果多个CPU同时请求锁会产生对锁的竞争，因此在真实的操作系统中设计了特殊的数据结构和算法来避免锁的竞争。

在`xv6`中存在两种锁，一种叫`spinlock`，另外一种叫`sleep-lock`。

`spinlock`是互斥锁，结构如下：

```C
// Mutual exclusion lock.
struct spinlock {
  uint locked;       // Is the lock held?

  // For debugging:
  char *name;        // Name of lock.
  struct cpu *cpu;   // The cpu holding the lock.
};
```

其中`locked`字段为0时表示该锁可获取，当该字段非零时，表示该锁已经被其他进程占有。

当进程想要申请锁的时候，我们一般会想到这样的函数：

```C
void acquire(struct spinlock* lk){
	for(;;) {
		if(lk->locked == 0){
			lk->locked = 1;
			break;
		}
	}
}
```

但是这段代码在一些情况下会发生错误。比如当两个运行在不同CPU上的进程同时申请锁的时候，可能同时进行`if`检测。这样两个进程都申请到了锁，这就违背了互斥的原则。所以我们想通过某种方式让**申请锁的过程保持原子性**。通常在多核处理器上设计了**内存和寄存器直接交换数据**的汇编指令来实现原子性。在`xv6`中实现如下：

```C
acquire(struct spinlock *lk)
{
  push_off(); // disable interrupts to avoid deadlock.
  if(holding(lk))
    panic("acquire");

  // On RISC-V, sync_lock_test_and_set turns into an atomic swap:
  //   a5 = 1
  //   s1 = &lk->locked
  //   amoswap.w.aq a5, a5, (s1)
  while(__sync_lock_test_and_set(&lk->locked, 1) != 0)
    ;

  // Tell the C compiler and the processor to not move loads or stores
  // past this point, to ensure that the critical section's memory
  // references happen strictly after the lock is acquired.
  // On RISC-V, this emits a fence instruction.
  __sync_synchronize();

  // Record info about lock acquisition for holding() and debugging.
  lk->cpu = mycpu();
}
```

我们不断通过原子操作去将`lk->locked`字段置 1 ，如果原来该字段的值为 1 那么我们置 1 并没有什么影响，但是如果原来为 0 ，我们就可以将该字段置为 1 并获得该锁。释放锁同样也是采用类似的方法实现的。

使用锁的难点在于，确定那些需要被保护的数据和不变性。这里有两个原则：

1. 如果一个数据在被一个CPU写入的时候，同时能够被其他CPU读取或者写入，那么这个数据需要加锁。
2. 如果一个不变性由多个地方的数据来构成，那么需要对构成该不变性的所有内存加锁。

除了需要知道何时加锁，避免使用多余的锁同样重要。因为过多的锁会减少系统的并发，从而降低性能。`xv6`中的`kalloc()`分配器就是典型的一个粗粒度锁，一个更好的做法是为每一个CPU维护一个空闲列表。`xv6`中的文件就是一个细粒度锁的方案，系统为每个文件都设置一个锁。如果想要实现多个进程对文件的不同位置进行读写，那么还可以设置粒度更为细致的锁。

## 死锁和锁排序

如果一个进程在内核中需要申请一系列锁，那么这需要其他所有进程都以相同的顺序获取这些锁。举个例子，进程A以lock_a，lock_b的顺序申请锁，那么其他进程想要同时申请这两个锁的时候就需要以相同的顺序申请。否则可能出现进程A已经成功申请lock_a，然而进程B成功申请了lock_b，两者相互等待的情况。这种情况被称为死锁。

为了解决这种情况，需要建立一个全局的锁申请顺序，所有函数只能按照这个顺序来申请锁。但是这个解决方案破坏了程序模块化设计的理念。

## 可重入锁

可重入锁，也叫递归锁。当一个进程已经占有了一个锁的时候，又在申请该锁，对于自旋锁来讲，这是典型的死锁。但是对于可重入锁来讲，这个操作是合法的，进程会继续运行。自旋锁更严格，有利于调试。但是如果合理设计，两种锁都是可以工作的。

## 锁与中断处理程序

中断处理程序
