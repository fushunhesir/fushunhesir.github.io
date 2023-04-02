---
title: cs144
date: 2023-01-30 10:18:30
tags: ['course' ,'cs-network', 'lab']
category: base-course

---

# RISCV

## Chapter 4

* **陷阱**：三种强制CPU放弃执行当前指令而执行特殊代码的事件
  * **系统调用**：软中断
  * **异常**：零除；无效的虚拟地址
  * **设备中断**：比如磁盘完成读写
* **从陷阱退出后，应该恢复至发生陷阱前的状态**
  1. 进入陷阱，强制进入kernel
  2. kernel保存当前运行状态
  3. kernel执行中断处理代码
  4. kernel用保存数据恢复状态
  5. 退出陷阱，继续正常执行代码

### RISC-V trap mechinery

