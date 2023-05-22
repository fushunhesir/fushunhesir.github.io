---
layout: wiki
title: 多线程
wiki: MIT-6.S081
order: 27
date: 2023-05-019 14:01:34

---

## 用户线程切换

**背景**：用户级线程相较于内核级线程更为轻量，因为不需要从用户态到内核态的切换。

**任务**

* 设计线程的创建和切换方案。完善`user/uthread.c`中的`thread_create()`和`thread_schedule()`，以及 `user/uthread_switch.S`中的`thread_switch`。 确保当`thread_schedule()`第一次运行给定线程时，该线程在其自己的堆栈上执行传递给`thread_create()`的函数。 另一个目标是确保`thread_switch`保存被切换的线程的寄存器，恢复被切换到的线程的寄存器，并返回到后一个线程指令中它最后停止的位置。修改`struct thread`以保存寄存器是不错的选择。 需要在`thread_schedule`中添加对`thread_switch`的调用；可以将任何需要的参数传递给`thread_switch`，但目的是从线程`t`切换到`next_thread`。
* 只需要保存`callee-save registers`。
* `user/uthread.asm`中的汇编代码也许对调试有所帮助。

**代码**

`uthread.c`

```C
void 
thread_create(void (*func)())
{
  struct thread *t;

  for (t = all_thread; t < all_thread + MAX_THREAD; t++) {
    if (t->state == FREE) break;
  }
  t->state = RUNNABLE;
  // YOUR CODE HERE
  t->context.ra = (uint64)func;
  t->context.sp = (uint64)t->stack + STACK_SIZE;
}

void 
thread_schedule(void)
{
  struct thread *t, *next_thread;

  /* Find another runnable thread. */
  next_thread = 0;
  t = current_thread + 1;
  for(int i = 0; i < MAX_THREAD; i++){
    if(t >= all_thread + MAX_THREAD)
      t = all_thread;
    if(t->state == RUNNABLE) {
      next_thread = t;
      break;
    }
    t = t + 1;
  }

  if (next_thread == 0) {
    printf("thread_schedule: no runnable threads\n");
    exit(-1);
  }

  if (current_thread != next_thread) {         /* switch threads?  */
    next_thread->state = RUNNING;
    t = current_thread;
    current_thread = next_thread;
    /* YOUR CODE HERE
     * Invoke thread_switch to switch from t to next_thread:
     * thread_switch(??, ??);
     */
    thread_switch((uint64)&t->context, (uint64)&current_thread->context);
  } else
    next_thread = 0;
}
```

`uthread_switch.S`

```asm
thread_switch:
	/* YOUR CODE HERE */
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
	ret    /* return to ra */
```

## 使用线程

**代码**

`ph.c`

```C
pthread_mutex_t lock;

static 
void put(int key, int value)
{
  int i = key % NBUCKET;

  // is the key already present?
  struct entry *e = 0;
  for (e = table[i]; e != 0; e = e->next) {
    if (e->key == key)
      break;
  }
  if(e){
    // update the existing key.
    e->value = value;
  } else {
    // the new is new.
    pthread_mutex_lock(&lock);
    insert(key, value, &table[i], table[i]);
    pthread_mutex_unlock(&lock);
  }
}
```

## 围栏

**代码**

`barrier`

```C
static void 
barrier()
{
  // YOUR CODE HERE
  //
  // Block until all threads have called barrier() and
  // then increment bstate.round.
  //
  pthread_mutex_lock(&bstate.barrier_mutex);
  bstate.nthread++;
  if(bstate.nthread < nthread)
    pthread_cond_wait(&bstate.barrier_cond, &bstate.barrier_mutex);
  if(bstate.nthread == nthread) {
    bstate.round++;
    bstate.nthread = 0;
    pthread_cond_broadcast(&bstate.barrier_cond);
  }
  pthread_mutex_unlock(&bstate.barrier_mutex);
}
```

