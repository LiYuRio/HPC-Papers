# Overlapping Host-to-Device Copy and Computation using Hidden Unified Memory

> 论文指路：https://dl.acm.org/doi/pdf/10.1145/3332466.3374531

## Abstract

本论文提出一套上层语法糖——HUM，在不进行任何代码修改的基础上，尽可能的用host端或者kernel的计算，隐藏统一内存架构下（Unified Memory）host端到device端数据拷贝的时间开销。HUM在运行时首先后检查是否可以直接将该同步内存拷贝操作变成异步的，若存在数据依赖，则先暂停host端或者kernel的后续计算，同时将原来一个连续的内存拷贝操作分成多个块，用round-robin的方式进行调度，使kernel尽可能早的开始执行。本论文通过评估来自Parboil，Rodinia和CUDA Code Samples的51个应用，发现使用HUM进行运行时优化的程序，平均可以获得1.21倍的性能提升。

## Introduction

> 

## Reference

