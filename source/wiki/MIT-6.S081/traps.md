---
layout: wiki
title: Traps
wiki: MIT-6.S081
order: 25
---

*帧指针、栈指针是函数调用的灵魂，栈指针负责为局部变量和参数分配空间，帧指针扮演寻找变量和参数的基石*

*内核堆栈和进程堆栈没有联系，只是他们都和进程一一对应*

## RISC-V汇编初识

**目的**：了解RISC-V中的汇编指令

**预备知识**：汇编语言

**任务**

* 编译`fs.img`，阅读`call.asm`中的`g`、`f`、`main`函数。回答以下问题并将答案编辑在`answers-traps.txt`文件之中

  * 哪些寄存器保存传递给函数的参数，例如：当`main`函数调用`printf`函数时，哪个寄存器保存了13？

    $a_0,a_1,a_2$，其中调用`printf`时，$a_2$寄存器保存了13

  * 在`main`函数的汇编码中，哪里调用了`f`，`g`这两个函数

  * 函数 `printf` 位于什么地址？

    `0x616`，在`RISCV`指令中，`auipc`指令一般结合其他跳转指令来执行，因为在`RISCV`指令中无法表示32位的立即数，因此需要用`auipc`指令将一个20位立即数左移12位加上`PC`，存储在目的寄存器中。然后再由接下来的跳转指令用12位立即数填充低12位，最后实现跳转到`PC`附近的32位地址。这里的计算就是：$0x30+1510_{(10)}=0x616$

  * 在 `jalr` 到 `main` 中的 `printf` 之后，寄存器 `ra` 中的值是什么？

    38，即当前地址+4

  * 运行以下代码

    ```C
    unsigned int i = 0x00646c72;
    printf("H%x Wo%s", 57616, &i);
    ```

    请问输出是什么？(这段代码的输出依赖于RISC-V的小端模式)。若这段代码运行在大端模式的机器中，要使输出相同，应该给$\;i\;$赋什么值？需要将57616改为其他值么？

    输出是`Hell0 World`，不用更改`57616`，只需要将`0x00646c72`改为`0x726c6400`

  * 在这行代码中

    ```C
    printf("x=%d y=%d", 3);
    ```

    $y=\;$后面会出现什么数字？(*提示：这个数据不是某个特定的数字*) 为什么会出现这种情况？

    y后面的数字是随机的数字，取决于从栈帧中取到a2的垃圾数据

## 堆栈回溯

**背景**：在调试过程中，如果知道在出错点处的堆栈中存在哪些没有返回的函数调用是非常有帮助的

**任务**

* 在`kernel/printf.c`中实现`backtrace()`函数，并在`sys_sleep`中调用
* 在`panic`函数中调用`backtrace`，以便在系统崩溃时了解相关信息

**代码**

* 修改`riscv.h`

  ```C
  // read the s0 register
  static inline uint64
  r_fp()
  {
    uint64 x;
    asm volatile("mv %0, s0" : "=r" (x) );
    return x;
  }
  ```

* `backtrace()`

  ```C
  void
  backtrace(void)
  {
    printf("backtrace:\n");
    uint64 fp = r_fp();
    uint64 head = PGROUNDDOWN(fp);
    uint64 tail = PGROUNDUP(fp);
    uint64 ra;
  
    while((fp >= head) && fp <= tail){
      ra = *(uint64*)(fp - 8);
      fp = *(uint64*)(fp - 16);
      printf("%p\n", ra);
    }
  }
  ```

* `panic(char* s)`

  ```C
  void
  panic(char *s)
  {
    pr.locking = 0;
    printf("panic: ");
    printf(s);
    printf("\n");
    backtrace();
    panicked = 1; // freeze uart output from other CPUs
    for(;;)
      ;
  }
  ```

* `sys_sleep`

  ```C
  uint64
  sys_sleep(void)
  {
    int n;
    uint ticks0;
  
    if(argint(0, &n) < 0)
      return -1;
    acquire(&tickslock);
    ticks0 = ticks;
    while(ticks - ticks0 < n){
      if(myproc()->killed){
        release(&tickslock);
        return -1;
      }
      sleep(&ticks, &tickslock);
    }
    release(&tickslock);
    backtrace();
    return 0;
  }
  ```

## 闹钟

**背景**：计算密集型进程往往想知道自己消耗了多少时间，同时一些进程希望定期做某件任务

**任务**

* 实现一个**用户级中断**处理程序，`sigalarm(interval, handler)`
* 调用`sigalarm(n, fn)`的函数每经过`n`个CPU时钟，`kernel`就会调用`fn`，当`fn`返回后，进程恢复继续执行。(*本质就是TRAP*)
* 当应用调用`sigalarm(0,0)`时，`kernel`应该停止周期闹铃
* 在`Makefile`中添加`alarmtest.c`，同时在`kernel`添加`sigalarm`和`sigturn`
* `user/alarmtest.asm`或许对调试有所帮助

**代码**

* 修改`Makefile`

  ```makefile
  ifeq ($(LAB),traps)
  UPROGS += \
  	$U/_call\
  	$U/_bttest\
  	$U/_alarmtest
  endif
  ```

* 修改`usys.pl`

  ```perl
  entry("sigalarm");
  entry("sigreturn");
  ```

* 修改`syscall.h`

  ```c
  #define SYS_sigalarm    22
  #define SYS_sigreturn     23
  ```

* 修改`syscall.c`

  ```C
  ...
  extern uint64 sys_sigalarm(void);
  extern uint64 sys_sigturn(void);
  
  static uint64 (*syscalls[])(void) = {
  	...
  	[SYS_sigalarm]  sys_sigalarm,
  	[SYS_sigreturn]   sys_sigreturn,
  }
  ```

* 修改`proc.h`

  ```c
  struct proc {
  	...
  	// these are private to the process, so p->lock need not be held.
  	...
  	uint64 interval;             // call handler for every interval ticks
    uint64 handler;              // run every interval ticks
    uint64 count;                // time left to call handler
    ...
  }
  ```

* 修改`proc.c`

  ```C
  static struct proc*
  allocproc(void)
  {
  	...
  	p->count = 0;
  	...
  }
  
  int
  sigalarm(uint64 interval, uint64 handler) {
    struct proc *p = myproc();
    p->interval = interval;
    p->handler = handler;
    return 0;
  }
  ```

* 修改`trap.c`

  ```c
  if(which_dev == 2){
    if(++p->count >= p->interval) {
      p->trapframe->epc = p->handler;
      p->count = 0;
    }
    yield();
  }
  ```

* 修改`sysproc.c`

  ```c
  uint64
  sys_sigalarm(void)
  {
    uint64 interval;
    uint64 handler;
    
    if(argaddr(0, &interval) < 0)
      return -1;
    if(argaddr(1, &handler) < 0)
      return -1;
    return sigalarm(interval, handler);
  }
  ```
