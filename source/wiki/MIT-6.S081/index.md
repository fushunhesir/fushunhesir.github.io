---
layout: wiki
wiki: MIT-6.S081
title: 环境配置
order: 0
---

## 本地环境

**设备**：*MacBook Air 2020 ( m1 芯片 )*

**编译器**：*Apple clang version 14.0.0 (clang-1400.0.29.202)*

## 问题清单

### 无穷递归

**问题描述**：*error: infinite recursion detected [-Werror=infinite-recursion]*

* **解决方法**：在**sh.c**中**runcmd**函数签名上添加*\__attribute__((noreturn))*
