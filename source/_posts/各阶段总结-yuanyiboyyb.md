---
title: 各阶段总结_yuanyiboyyb
date: 2025-05-26 00:36:06
categories:
tags:
    - author:yuanyiboyyb
    - https://github.com/LearningOS/rCore-Tutorial-Guide-2025S.git
---
# 各阶段总结

## 第一阶段

rust的基本学习，挺多有用的函数记不住，还需要多写代码

## 第二阶段


- Chapter 1: 应用程序与基本执行环境

删除了标准库，适应一下内核级别的编程，不过其实core库很多都是直接引用std库的哈哈哈

- Chapter 2: 批处理系统

操作系统必须负责保存所有的通用寄存器以及一系列关键的控制状态寄存器（CSRs），例如sstatus（记录CPU当前的特权级别）、sepc（记录发生trap时的指令地址，即trap返回后应继续执行的地址）、scause/stval（分别记录trap发生的原因及相关的附加信息）。这些CSRs对于用户态程序而言通常是透明的。出于安全性的考量，操作系统必须为内核态的执行维护一个独立的内核栈，而非在用户栈的基础上进行扩展。RISC-V架构为此提供了sscratch寄存器，可用于保存内核栈的地址。当陷入（trap）发生时，操作系统可以从sscratch寄存器中获取内核栈的起始地址，并切换至内核栈上开始后续处理。另外，在此阶段，所有的内存操作均基于物理地址进行，尚未实现内核态与用户态之间的内存隔离。理论上，应用程序能够访问包括内核栈在内的所有物理内存区域，这具有极大的潜在风险。此内存隔离问题将在后续实现虚拟内存机制时得到解决。

- Chapter 3: 多道程序与分时多任务

当初学操作系统时对中断说实话没什么概念，原来真的是中中又断断啊，真的中断触发时直接跳转到中断处理程序。当然中断处理程序的地址由一个专门的寄存器来储存

- Chapter 4: 地址空间

由于内核与应用程序的地址空间通常是不同的（或者说，它们各自拥有不同部分的内存映射），在从用户态陷入内核态之前，往往需要借助一个位于固定虚拟地址的“跳板页”（trampoline page）来实现上下文的平滑、安全切换。是的，也会有一个寄存器用来存储根页表的地址，将根页表的地址放到寄存器里之后，mmu会接管地址访问，也就是说，之后的地址转换由硬件完成，而不是软件。

- Chapter 5: 进程

我原以为最为复杂的进程与shell机制，原来如次简单。

- Chapter 6: 文件系统

其实文件系统的底层还是客户操作系统的read write系统调用，只不过封装成block的形式，真实操作系统里应该会依赖与硬件特性以及驱动的实现

- Chapter 7: 进程间通信

这一章虽然没有题，但是我还是建议好好看看代码，解答了过往我的很多疑惑

- Chapter 8: 并发

“纸上得来终觉浅，绝知此事要躬行”，当初操作系统课上我觉得银行家算法是一个没有实际应用价值的东西，通过实验的一点点试错尝试，我还是最终推导出了银行家算法，其解决的不是所有死锁问题，只是解决死锁中的a进程获得a锁，b进程获得了b锁，然后a想获得b锁，b想获得a锁这种问题，受益匪浅啊

## 第三阶段

这一块的由于杂事颇多还需好好研究一下代码以及项目组织架构，几道题中有意思的是Hipervisor，其通过一个循环不多进入虚拟机状态，如果虚拟机有解决不了或权限不足无法执行的指令，就通过陷入由陷入处理函数来帮助虚拟机执行，然后在进入虚拟机模式，挺有意思，需要好好研究一下之后的东西。

