---
layout: wiki
title: 文件系统
wiki: MIT-6.S081
order: 10
date: 2023-05-019 14:01:34


---

文件系统为用户提供了何种抽象？这种抽象为用户提供了哪些服务？需要使用哪些机制来保证这些服务的正常运行？

## 文件系统的功能

文件系统是为组织和存储数据而设计的，通常具备共享性和持久化的性质。文件系统解决如下问题：

* 需要特殊的数据结构来表示目录文件树、记录每个文件使用的磁盘块的标示号和空闲磁盘块。该数据结构存储在磁盘上。
* 需要能够在崩溃后进行恢复。当文件系统正在进行更新操作的时候崩溃，系统可能会处于不一致状态，需要在重启后恢复。
* 需要解决并发问题。同一时间可能有多个进程和文件系统进行交互，需要保证这些操作不破坏其中的不变性。
* 需要解决速度匹配问题。对磁盘的操作是很慢的，所以文件系统需要在内存中缓存磁盘块来提高效率。

## 文件系统分层结构

<img src="https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E5%88%86%E5%B1%82%E6%A8%A1%E5%9E%8B.png" alt="image-20230522094350769" style="zoom:30%;" />

其中Disk层提供对磁盘块的读写服务，Buffer cache层缓存文件块，同时使用同步机制来限制同一时间只能有一个内核线程访问某个数据块。Logging层将进程对多个磁盘块的更新打包为事务，并将其设置为原子操作，以便在面对系统崩溃的时候进行恢复。Inode层提供了文件的概念，用一个带有唯一数字标号的inode以及多个磁盘块来代表一个文件。Directory层是特殊的inode，内部包含多个文件名和inode标号。Pathname层提供分层路径名，如/usr/rtm/xv6/fs.c，并通过递归查找进行解析。File descripter层使用文件系统接口抽象了许多Unix资源（例如，管道、设备、文件等），简化了应用程序程序员的生活。

这里需要区分两个概念。通常磁盘作为硬件，出厂设置的默认分块大小为512bytes，称为`sector`。而在操作系统通常会将多个`sector`合并成一个更大的操作单元，称为`block`。在`xv6`中，`block`的大小为2个`sector`，也就是1024bytes。`xv6`在用结构体`buf`来表示缓存在内存的磁盘块。

```c
struct buf {
  int valid;   // has data been read from disk?
  int disk;    // does disk "own" buf?
  uint dev;
  uint blockno;
  struct sleeplock lock;
  uint refcnt;
  struct buf *prev; // LRU cache list
  struct buf *next;
  uchar data[BSIZE];
};
```

文件系统通常吧磁盘划分为不同区域。其中`block0`通常是不使用的，该磁盘块用来存储操作系统的`boot sector`(引导程序)。`block1`被称为`superblock`，通常用来存储文件系统的元数据，包括文件系统的大小，`block`的块数等等。然后是日志块，`inodes`块和位图块，最后是数据块。

![image-20230523085628472](https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/%E7%A3%81%E7%9B%98%E5%88%92%E5%88%86.png)

## Buffer cache层

这一层主要提供两个服务，其一是限制对磁盘块的同步访问，以确保一个磁盘块在内存中的映射只有一个以及同一时间只有一个内核线程能够访问某个磁盘块映射。内存中的`cache`大小是有限的，因此只能缓存固定数量的磁盘块，当内核线程访问一个不在`cache`的磁盘块时，需要根据LRU策略来用新的磁盘块替换旧磁盘块。
