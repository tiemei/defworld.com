---
title: 压力测试-jvm启动时负载较高问题
date: '2015-11-03'
description:
categories: 'stress-test'
type: draft
---

# 准备工作

* 输出jvm启动加载了从那些位置加载了那些类: `java -verbose:class`启动
* [如何查询cpu消耗较高的线程](http://defworld.com/2015/11/03/stress-test_java-thread-cpu.html)

#  问题症结：C2 CompilerThread

> Java程序在启动的时候所有代码的执行都处于解释执行模式，只有在运行了一段时间后，根据代码方法执行的次数，或代码里循环的执行次数等达到一定的阈值才会编译成机器码，编译成机器码后执行效率会得到大幅提升，而随着执行时间进一步拉长，JVM的各种更高级的编译优化手段就会逐渐加上，例如if条件的执行状况，逃逸分析等。这里的C2 CompilerThread线程干的就是编译优化的活。

# 解决思路

## 预热

* 程序

## 加快编译速度

> 1. Interpretation :You could think of interpretation similar to using a dictionary: for a specific word (bytecode instruction) there is an exact translation (machine code instruction). 额外解释工作负担加上没有做相应平台的指令集优化，导致性能较差。
> 2. 

操作方案：  

1. **-XX:CICompilerCount** 指定多个线程进行编译
2. **-XX:+TieredCompilation** 开启Tiered模式，  编译方式有三种：1）Client模式；2）Server模式；3）Tiered模式。我们服务默认是Server模式。Client比Server模式编译更快触发，但编译后程序效率不如Server，Tiered这种，一开始用Client模式，后来用Server模式。性能比较见下图。

![compiler performance comparisons]()   








参考：

[发布或重启线上服务时抖动问题解决方案](http://www.cnblogs.com/LBSer/p/3703967.html)  

[JVM performance optimization, Part 1: A JVM technology primer](http://www.javaworld.com/article/2078623/core-java/jvm-performance-optimization-part-1-a-jvm-technology-primer.html)  
[JVM performance optimization, Part 2: Compilers](http://www.javaworld.com/article/2078635/enterprise-middleware/jvm-performance-optimization-part-2-compilers.html)  
[JVM performance optimization, Part 3: Garbage collection](http://www.javaworld.com/article/2078645/java-se/jvm-performance-optimization-part-3-garbage-collection.html) 
[JVM performance optimization, Part 4: C4 garbage collection for low-latency Java applications](http://www.javaworld.com/article/2078661/java-concurrency/jvm-performance-optimization--part-4--c4-garbage-collection-for-low-latency-java-ap.html)  
[JVM performance optimization, Part 5: Is Java scalability an oxymoron?](http://www.javaworld.com/article/2078731/java-platform/jvm-performance-optimization--part-5--is-java-scalability-an-oxymoron-.html)  


