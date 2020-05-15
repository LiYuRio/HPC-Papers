# Optimizing batched Winograd convolution on GPUs

> 论文指路：https://www.cse.ust.hk/~weiwa/papers/yan-ppopp20.pdf

## Abstract

本篇论文提出了一个“基于英伟达Volta架构和Turing架构GPU的，单精度Winograd卷积操作实现”。基于V100和RTX2070设备，与传统cuDNN 7.6.1中的优化实现相比，分别有2.13倍和2.65倍的性能提升，并最高可达到峰值性能的93%。

同时本篇论文还开源了一个实用型工具，[Turingas](https://github.com/andravin/wincnn)，主要针对Volta架构和Turing架构的一个SASS汇编器，帮助在汇编层面更好的改善程序的性能。

## Reference

* [Winograd卷积操作](http://shuokay.com/2018/02/21/winograd/)