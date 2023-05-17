---
layout: wiki
title: Copy-On-Write
wiki: MIT-6.S081
order: 26
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
  struct proc* p = myproc();
  char* mem;
  uint64 pa, va;
  pte_t* pte;

  // check the pin count
  va = r_stval();
  pa = walkaddr(p->pagetable, va);
  if(pin_count[INDEX(pa)] <= 1){
    pte = walk(p->pagetable, va, 0);
    *pte ^= PTE_W;
    *pte &= !PTE_COW;
  } else {
    // auquire a page size mem
    if((mem = kalloc())==0)
      goto err;
    // copy mem from old physical mem
    memmove(mem, (char*)pa, PGSIZE);
    // remap
    uvmunmap(p->pagetable, PGROUNDDOWN(va), 1, 0);
    if(mappages(p->pagetable, PGROUNDDOWN(va), PGSIZE, (uint64)mem, PTE_W|PTE_X|PTE_R|PTE_U) != 0){
      kfree(mem);
      goto err;
    }
    pin_count[INDEX(pa)] -= 1;
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

`kalloc.c`

```c
extern int pin_count[];

// Free the page of physical memory pointed at by v,
// which normally should have been returned by a
// call to kalloc().  (The exception is when
// initializing the allocator; see kinit above.)
void
kfree(void *pa)
{
  struct run *r;

  if(((uint64)pa % PGSIZE) != 0 || (char*)pa < end || (uint64)pa >= PHYSTOP)
    panic("kfree");

  // if reference count of physical page is not 0
  if(pin_count[INDEX((uint64)pa)]) return ;

  // Fill with junk to catch dangling refs.
  memset(pa, 1, PGSIZE);

  r = (struct run*)pa;

  acquire(&kmem.lock);
  r->next = kmem.freelist;
  kmem.freelist = r;
  release(&kmem.lock);
}

// Allocate one 4096-byte page of physical memory.
// Returns a pointer that the kernel can use.
// Returns 0 if the memory cannot be allocated.
void *
kalloc(void)
{
  struct run *r;

  acquire(&kmem.lock);
  r = kmem.freelist;
  if(r)
    kmem.freelist = r->next;
  release(&kmem.lock);

  if(r){
    memset((char*)r, 5, PGSIZE); // fill with junk
    pin_count[INDEX((uint64)&r)]=1;
  }
  return (void*)r;
}
```

`vm.c`

```C
// Given a parent process's page table, copy
// its memory into a child's page table.
// Copies both the page table and the
// physical memory.
// returns 0 on success, -1 on failure.
// frees any allocated pages on failure.
int
uvmcopy(pagetable_t old, pagetable_t new, uint64 sz)
{
  pte_t *pte;
  uint64 pa, i;
  uint flags;
  // char *mem;

  for(i = 0; i < sz; i += PGSIZE){
    if((pte = walk(old, i, 0)) == 0)
      panic("uvmcopy: pte should exist");
    if((*pte & PTE_V) == 0)
      panic("uvmcopy: page not present");
    pa = PTE2PA(*pte);

    // set the pte read only
    *pte &= !PTE_W;
    if((*pte & PTE_COW) != 0)
      *pte ^= PTE_COW;
    flags = PTE_FLAGS(*pte);

    // if((mem = kalloc()) == 0)
    //   goto err;
    // memmove(mem, (char*)pa, PGSIZE);
    
    if(mappages(new, i, PGSIZE, (uint64)pa, flags) != 0){
      // kfree(mem);
      goto err;
    }
  }
  return 0;

 err:
  uvmunmap(new, 0, i / PGSIZE, 1);
  return -1;
}
```

