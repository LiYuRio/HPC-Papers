# POSTER: A Parallel Sparse Tensor Benchmark Suite on CPUs and GPUs

> 论文指路：https://dl.acm.org/doi/pdf/10.1145/3332466.3374513

## Abstract

在目前的很多应用中都涉及张量的计算，提升张量计算的性能需要研究数据的排布方式（layout），执行方式（schedule）以及如何并行（parallelism），本论文提出一套基于任意顺序COO和HiCOO格式的稀疏张量kernel的基准实现，并在Intel CPUs和NVIDIA GPUs两个硬件平台上对张量kernel的实现进行验证。

## Introduction

张量的计算尤其是稀疏张量的计算逐渐变成某些实际应用中的性能瓶颈，如何评估不同实现方法的性能就变得比较重要，而如何优化张量计算kernel的性能，因为维度问题、张量形状的转换、不规则访问等问题是优化工作需要解决的难点，于是本论文提出一套有关常见张量kernel的基准实现，帮助研究者在不同性能平台和数据下，有一个参考实现的基准对比。

## Reference



