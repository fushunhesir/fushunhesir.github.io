---
layout: wiki
wiki: MIT-6.S081
title: 环境配置
order: 9

---

通过调度，实现了进程间的隔离。但是真实的操作系统中，往往进程之间存在通信，比如一个进程等待硬盘读写，如何解决这个问题呢？

## 睡眠唤醒机制

`xv6`中的睡眠唤醒机制是一个相对底层的接口，通过这个机制可以实现一个更高级的接口，通常被称为信号量。信号量的典型方法被称为`PV`操作，通常是给信号量做加减。

```C
struct semaphore {
	spinlock lock;
	int count;
}

void P(struct semaphore* s) {
  acquire(s->lock);
  
  while(s->count == 0)
    sleep(s, s->lock);
  
  s->count -= 1;
  release(s->lock);
}

void V(struct semaphore* s) {
  acquire(s->lock);
  s->count++;
  weakup(s);
  release(s->lock);
}
```

通过睡眠唤醒机制避免忙等待，同时为了避免唤醒丢失和死锁，所以在`sleep`中传入锁。

```C
void P(struct semaphore* s) {
  while(s->count == 0)
    sleep(s);
  acquire(s->lock);
  s->count -= 1;
  release(s->lock);
}

void V(struct semaphore* s) {
  acquire(s->lock);
  s->count++;
  weakup(s);
  release(s->lock);
}
```

上述代码就可能出现唤醒丢失的情况。因为`sleep(s)`和判断条件之间存在小段时间，可能`s->count`不为0，但是进程进入睡眠了，破坏了进程仅在计数为0时睡眠的不变性，从而错过唤醒，进程睡死。

接下来是`xv6`中实现睡眠唤醒机制的真实代码：

```C
// Atomically release lock and sleep on chan.
// Reacquires lock when awakened.
void
sleep(void *chan, struct spinlock *lk)
{
  struct proc *p = myproc();
  
  // Must acquire p->lock in order to
  // change p->state and then call sched.
  // Once we hold p->lock, we can be
  // guaranteed that we won't miss any wakeup
  // (wakeup locks p->lock),
  // so it's okay to release lk.

  acquire(&p->lock);  //DOC: sleeplock1
  release(lk);

  // Go to sleep.
  p->chan = chan;
  p->state = SLEEPING;

  sched();

  // Tidy up.
  p->chan = 0;

  // Reacquire original lock.
  release(&p->lock);
  acquire(lk);
}
```

```c
// Wake up all processes sleeping on chan.
// Must be called without any p->lock.
void
wakeup(void *chan)
{
  struct proc *p;

  for(p = proc; p < &proc[NPROC]; p++) {
    if(p != myproc()){
      acquire(&p->lock);
      if(p->state == SLEEPING && p->chan == chan) {
        p->state = RUNNABLE;
      }
      release(&p->lock);
    }
  }
}
```

