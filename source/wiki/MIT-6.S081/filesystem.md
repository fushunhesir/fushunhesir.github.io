---
layout: wiki
title: 文件系统
wiki: MIT-6.S081
order: 10
date: 2023-05-019 14:01:34


---

文件系统为用户提供了何种抽象？这种抽象为用户提供了哪些服务？需要使用哪些机制来保证这些服务的正常运行？

## 文件系统分层结构

<img src="https://cdn.jsdelivr.net/gh/fushunhesir/blog-images@main/imgs/%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E5%88%86%E5%B1%82%E6%A8%A1%E5%9E%8B.png" alt="image-20230522094350769" style="zoom:30%;" />

其中Disk层提供对磁盘块的读写服务，Buffer cache层缓存文件块，同时使用同步机制来限制同一时间只能有一个内核线程访问某个数据块。Logging层将进程对多个磁盘块的更新打包为事务，并将其设置为原子操作，以便在面对系统崩溃的时候进行恢复。Inode层提供了文件的概念，用一个带有唯一数字标号的inode以及多个磁盘块来代表一个文件。Directory层是特殊的inode，内部包含多个文件名和inode标号。Pathname层提供分层路径名，如/usr/rtm/xv6/fs.c，并通过递归查找进行解析。File descripter层使用文件系统接口抽象了许多Unix资源（例如，管道、设备、文件等），简化了应用程序程序员的生活。

## Buffer cache层

这一层主要提供两个服务，其一是限制对磁盘块的同步访问，以确保一个磁盘块在内存中的映射只有一个以及同一时间只有一个内核线程能够访问某个磁盘块映射。
