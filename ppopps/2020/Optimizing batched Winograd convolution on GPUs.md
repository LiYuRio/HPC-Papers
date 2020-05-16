# Optimizing batched Winograd convolution on GPUs

> 论文指路：https://www.cse.ust.hk/~weiwa/papers/yan-ppopp20.pdf

## Abstract

本篇论文提出了一个“基于英伟达Volta架构和Turing架构GPU的，单精度Winograd卷积操作实现”。基于V100和RTX2070设备，与传统cuDNN 7.6.1中的优化实现相比，分别有2.13倍和2.65倍的性能提升，并最高可达到峰值性能的93%。

同时本篇论文还开源了一个实用型工具，[Turingas](https://github.com/andravin/wincnn)，主要针对Volta架构和Turing架构的一个SASS汇编器，帮助在汇编层面更好的改善程序的性能。

## Introduction

> 主要从三个方面来总结：为什么要优化这个算法？解决问题过程中有什么困难？用什么技术来优化的？

卷积神经网络（CNN）在计算机视觉等方面有广泛应用，但是在大规模数据集上需要很高的训练成本，卷积操作是CNN的核心操作，也是改善CNN性能的重点。Winograd算法相比于原始的卷积计算过程，可以有效减少所需要的算术操作次数，理论证明，使用$3\times 3$的卷积核，可以将卷积操作的计算复杂度降低2.25倍。目前主流的高性能深度学习库，如cuDNN和MKL-DNN，内部对该操作都有支持。但是实验发现，Winograd算法在GPU平台上未能达到其理论性能加速比。

优化Winograd算法过程中的困难：

* Winograd算法本身涉及两个转置操作，所以要设计数据的存储方式，使其能合并内存访问而且避免bank conflict。
* 相比于GEMM计算密集程度较低，延迟不那么好隐藏。
* GPU硬件提供的regular registers和predicate registers数量有限，实现算法过程中需要满足这个限制。

对应本论文的优化方案：

* 重新设计了一种任务和数据划分方式，使得访存合并且没有back conflict。
* 尽可能的增大block的大小（相当于增加了一个块内活跃的线程数），同时也使用软件层面的流水线隐藏访存延迟。
* 保证寄存器的使用满足硬件限制，同时将predicate registers打包放入regular register中，减少重复的zero-padding mask计算。

优化过程中遇到的问题和对应的解决方法：

* 增大block的大小，意味着不同warp之间的负载均衡问题会影响整体的性能。
* 没有高效的接口支持predicate registers到regular registers的转换，如果简单使用多个regular registers代替predicate registers，很容易造成寄存器溢出（spilling）
* 以上两个问题只能在SASS层面解决，故提出针对Volta架构和Turing架构GPU的SASS汇编器，从而可以更好的进行性能调优。

## Reference

* [Winograd卷积操作](http://shuokay.com/2018/02/21/winograd/)
* [Winograd卷积操作原理](https://blog.usejournal.com/understanding-winograd-fast-convolution-a75458744ff)