# POSTER: Optimizing GPU Programs By Partial Evaluation

> 论文指路：https://dl.acm.org/doi/pdf/10.1145/3332466.3374507

## Abstract

在很多情况下，访存限制是影响提升程序性能的主要因素，不过相对应的也存在一些策略帮助进行更有效的访存，对此本文提出一个基于部分评估的自动访存优化方法，通过提前计算部分数据访存结果并将其直接嵌入程序，从而达到提高访存性能的目的。实验结果表明，应用这种优化方法可以将朴素字符串匹配算法在CUDA上的性能提高8倍。

## Introduction

> 主要从三个方面来总结：为什么要提出基于Partial Evaluation的内存优化方法？解决了诸如哪类问题？

目前大多数改善程序访存的手段，如sophisticated memory pooling、shared memory register spilling、automatic shared memory allocation等，本质上是利用GPU内存的一些特性来进行优化，一旦访存方式不正确，随着内存访问次数的增加，仍然会造成性能问题。故本文希望能从源头上解决这个问题，利用Partial evaluation技术，直接减少内存事件的数量。

所谓Partial evaluation技术，是将某程序的输入数据分为静态（已知）和动态（未知）数据两种，把已知的静态输入代入程序做优化，使得程序在运行时，针对动态数据产生和优化前相同的结果。

这样的应用场景有很多，比如查询数据库（待查询的字符串是静态数据），字符串匹配（匹配模式是静态数据），目前使用Partial evaluation技术，使得CPU上的数据库查询操作已经达到了比较好的性能，这种性能改进应该也对GPU有效，故本文主要针对多模式匹配的字符串问题，在GPU上进行优化，将匹配模式作为静态数据，在编译期间进行优化并直接将具体数值嵌入程序，从intruction cache中访问数据总胜过使用load指令访存而带来的开销，以此达到优化性能的目的。

## Reference

* [Partial Evaluation，部分评估](http://pages.cs.wisc.edu/~horwitz/CS704-NOTES/9.PARTIAL-EVALUATION.html)

