---
layout: wiki
title: Copy-On-Write
wiki: MIT-6.S081
order: 26
date: 2023-05-019 14:01:34
---

## 实现写时复制

**背景**：`fork()`将父进程的所有内存复制到子进程，如果父进程占用内存很大那么复制将会十分耗时，而且如果子进程在调用`fork()`后，直接调用`exec()`那么之前所做的复制都是无效的工作，这是对`CPU`资源的浪费。

**任务**

* 修改`uvmcopy()`将父进程的物理页面映射到子进程的页表之中。同时将父进程和子进程页表的`PTE`均设置为只读。
* 修改`usertrap()`识别COW页面的缺页异常，触发时通过`kalloc()`给子进程分配物理页面，并复制父进程内容，修改`PTE_W`
* 确保对没有被任何进程的页表所引用的页面进行回收。可以通过给每个物理页面记录一个`reference count`。当`kalloc()`时将`reference count`设置为1，当`reference count`为0时，`kfree()`才会回收页面。可以使用一个固定大小的整型数组来存储`reference count`，但是必须计算出合理的数组大小，并设计合理的索引方式。例如，你可以将物理页面地址除以4096作为索引，并通过最高的物理地址来计算出数组的大小。
* 修改`copyout()`进行复制，就像遇到缺页异常一样。

**提示**

* 使用`RSW`位来记录一个页面是否是`COW`映射
* 在`kernel/riscv.h`中定义了对你有帮助的宏和页表标识位。

* 当一个COW页面发生缺页异常，如果此时没有多余的内存，那么选择将该进程杀死。

**代码**

`trap.c`

```C
else if((PTE_FLAGS(*(walk(p->pagetable, r_stval(), 0))) & PTE_COW) != 0) {
    // cow handler
    if(cow_handler() == -1)
      p->killed = 1;
}
...
...
...
// allocate page for cow mapping
// and copy the content from
// old page.
int
cow_handler(){
  char* mem;
  uint64 pa;
  pte_t* pte;
  uint flags;

  pte = walk(pagetable, va, 0);
  pa = PTE2PA(*pte);

  // check the pin count
  int pin = pin_count[INDEX(pa)];
  if(pin == 2) {
    *pte &= ~PTE_COW;
    *pte |= PTE_W;
  }
  if(pin > 2) {
    // auquire a page size mem
    if((mem = kalloc())==0)
      goto err;
    // remap
    flags = PTE_FLAGS(*pte);
    flags = (flags & ~PTE_COW) | PTE_W;
    memmove(mem, (void*)PTE2PA(*pte), PGSIZE);
    uvmunmap(pagetable, PGROUNDDOWN(va), 1, 1);
    if(mappages(pagetable, PGROUNDDOWN(va), PGSIZE, (uint64)mem, flags) != 0){
      kfree(mem);
      goto err;
    }
  }

  return 0;

  err:
    return -1;
}
```

`riscv.h`

```C
#define PTE_COW (1L << 8) // 1 -> this is a cow mapping
#define INDEX(a) ((a-KERNBASE)>>12)
```

`defs.h`

```C
// vm.c
pte_t*          walk(pagetable_t, uint64, int);
```

`vm.c`

```C
int
mappages(pagetable_t pagetable, uint64 va, uint64 size, uint64 pa, int perm)
{
  ...
	// reference count up
	if(pa >= KERNBASE) pin_count[INDEX(pa)] += 1;
  ...
}


// Remove npages of mappings starting from va. va must be
// page-aligned. The mappings must exist.
// Optionally free the physical memory.
void
uvmunmap(pagetable_t pagetable, uint64 va, uint64 npages, int do_free)
{
  ...
  // reference count minus 1
	uint64 pa = PTE2PA(*pte);
	if(pa >= KERNBASE) pin_count[INDEX(pa)] -= 1;
	if(do_free && pin_count[INDEX(pa)] == 1){
		kfree((void*)pa);
	}
	*pte = 0;
  ...
}

// Given a parent process's page table, copy
// its memory into a child's page table.
// Copies both the page table and the
// physical memory.
// returns 0 on success, -1 on failure.
// frees any allocated pages on failure.
int
uvmcopy(pagetable_t old, pagetable_t new, uint64 sz)
{
  ...
	// set the pte read only
	*pte &= ~PTE_W;
	*pte |= PTE_COW;
	flags = PTE_FLAGS(*pte);
  
	if(mappages(new, i, PGSIZE, (uint64)pa, flags) != 0){
		goto err;
	}
  ...
}

// Copy from kernel to user.
// Copy len bytes from src to virtual address dstva in a given page table.
// Return 0 on success, -1 on error.
int
copyout(pagetable_t pagetable, uint64 dstva, char *src, uint64 len)
{
  ...
  if(*pte & PTE_COW) cow_handler(pagetable, dstva);
  ...
}
```

**代码思路**

本次实验是实现写时复制操作，本身逻辑比较清晰。

首先，在`usertrap()`中检查`COW`页面异常。在实现写时复制前，每个物理页面都只映射到唯一进程的地址空间之中，当修改`uvmcopy()`后，会出现一个物理页面映射到多个进程的地址空间的情况，因此此时会出现`r_scause()==15`的错误，该错误代表写指令造成的页面错误。

在`usertrap()`检测`COW`页面错误后，需要对这个错误进行处理。这里我选择单独写一个`cow_handler()`来处理错误，之所以这样决定是因为`copyout()`函数是通过`walk()`来访问页表，并不会引发硬件页表错误，因此也需要在`copyout()`中检测并并处理`COW`页面。因此，单独写一个`cow_handler()`能够实现代码复用，简化程序逻辑。

在总结`cow_handler()`处理逻辑前，我想先讲讲对页面引用计数的逻辑。我认为一个页面的引用计数应该和页面映射和解除映射这两个操作绑定，而不是和`kalloc`和`kfree`这两个操作绑定。因为只有当一个页面真正映射到了进程之中时，它的引用数才会增加。所以我将引用计数的更新操作放在映射和解映射操作中，以将这两个操作绑定，简化处理逻辑。

接下来，介绍`cow_handler()`的处理逻辑：检查物理页面的引用数，如果引用数为2，说明除了内核页表外只有一个进程的页表引用了这个物理页面，这种情况只需要将`PTE`的权限修改即可。如果引用数大于2，那么说明除内核外有多个进程引用了该物理页面，所以需要复制改物理页面，解除旧物理页面到该进程地址空间的映射，将新物理页面映射到该进程的地址空间中，并设设置正确的访问权限标识位。

**实验总结**

以上思路是很简洁明了的，代码可以很迅速写完。但是在调试过程中，这次实验给我带来了很多收获：

* 写代码之前，需要理清逻辑，画好流程图。这一步还是很重要的，没有流程图思路就会比较混乱，这次先画了流程图，写代码就很清晰。但是需要注意，流程图中的逻辑一定要正确，比如我在写回收页面的判断条件的时候，最初确定的是`pin==1`，这就忽略了内核在初始化的时候会进行一次映射，所以正确的判断应该是`pin==2`。

* 对于流程图中的每个模块的具体实现，需要仔细审视，确定每一步操作之间的依赖关系。这个相当重要，在最初的实现之中我想着就是解除映射，修改权限标志位，然后重新映射，复制页面就没问题了，操作的顺序就没有仔细考虑。这就导致调试了相当长的时间，而且觉得这段代码没有问题。事实上，在我的实现中，这4个操作之间是有相对依赖的。如果按照我错误的顺序来做，解除映射会将进程的`PTE`设置为0，然后我在该`PTE`权限基础上添加写权限和将`PTE_COW`位置0，所以得到了错误的权限，因为其他位都是0。而我的逻辑仍然是该`flags`代表了父进程的标志位。然后用错误的权限将新的物理页面映射到进程地址空间。接着的内存复制的源物理地址也依赖于`PTE`，此时`PTE`对应的物理页面已经修改为了新的物理页面，因此这段命令是原地复制。

  因此，明确指令间的依赖关系是很重要的，当感觉指令顺序可以随意调换的时候，最好慎重考虑一下。
