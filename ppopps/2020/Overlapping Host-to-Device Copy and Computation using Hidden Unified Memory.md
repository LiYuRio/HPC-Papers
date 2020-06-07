# Overlapping Host-to-Device Copy and Computation using Hidden Unified Memory

> 论文指路：https://dl.acm.org/doi/pdf/10.1145/3332466.3374531

## Abstract

本论文提出一套底层运行时工具——HUM，在不进行任何代码修改的基础上，利用统一内存架构下（Unified Memory）和错误恢复机制的帮助，尽可能的用host端或者kernel的计算，隐藏host端到device端数据拷贝的时间开销。HUM在运行时首先后检查是否可以直接将该同步内存拷贝操作变成异步的，若存在数据依赖，则先暂停host端或者kernel的后续计算，同时将原来一个连续的内存拷贝操作分成多个块，用round-robin的方式进行调度，使kernel尽可能早的开始执行。本论文通过评估来自Parboil，Rodinia和CUDA Code Samples的51个应用，发现使用HUM进行运行时优化的程序，平均可以获得1.21倍的性能提升。

## Introduction

CUDA提供了统一内存的概念，相当于又做了一层抽象，在用户看来，不同硬件都可以访问一个统一的内存，由CUDA内部的机制自动进行数据传输。这是一种给用户提供编程方便，但不保证性能的方法。导致使用者为了追求性能的提升，还是转而使用原始的`cudaMalloc`和`cudaMemcpy`方法，或者显式的使用`cudaMemPrefetchAsync`方法手动干涉传输工作的时机。为了尽可能的隐藏由于传输数据带来的时间开销，本文主要从两个方面来隐藏传输：

* 尽可能的用host端的计算来隐藏传输时间，对此CUDA已提供`cudaMemcpyAsync`接口，但有些后续计算会更改正在传输的数据，导致计算错误，在这种情况下，不应该使用异步传输。HUM会捕捉对未完成传输的数据页的访问，暂停其访问直到传输完成。
* 用kernel端的计算来隐藏延迟，在使用Unified Memory的前提下，CUDA会默认异步执行kernel，对此HUM帮助用户，在不使用Unified Memory时，也可以异步执行kernel。

HUM给用户提供运行时帮助，使得在不更改程序源代码或者使用Unified Memory的基础上，也能自动的隐藏数据传输过程的开销，达到提供程序性能的目的。

## CUDA Unified Memory

HUM受UM处理缺页错误的启发，来管理内存。统一内存概念的提出使程序员从来回拷贝数据中解脱出来，分别给GPU和CPU分配一个UM内存表，由CUDA runtime进行管理，对应实际的物理内存（CPU端的物理内存为pinned memory）。对Pascal架构及以上，使用`cudaMallocManaged`时，不会进行实际的内存分配，当某一端开始访问数据时，才会有实际的内存被分配。CUDA runtime会根据需要自动对数据进行传输，当某一端的访问造成缺页错误时，NVIDIA driver会捕捉到这一错误，并在CPU和GPU的UM间传输缺失页。另外，NVIDIA driver本身采取某些启发式手段，尽量减少访问时的页缺失。

## Design and Implementation



## Reference

