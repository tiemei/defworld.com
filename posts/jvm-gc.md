---
title: jvm——gc
date: '2015-07-08'
description:
categories: 'java'
---

## 几种常见的垃圾回收算法

### 引用计数法(Reference Counting Collector)

![Reference Counting Collector](https://farm1.staticflickr.com/542/18944422264_f6d3a376ce.jpg)  

堆中每个对象维护一个引用计数，一个对象被创建后，每一个新的引用执行这个对象，引用计数+1；每减少一个新的引用，或者原来的引用超过了生命周期，引用计数-1；当一个引用计数为0的对象被垃圾回收，它所有引用的所有对象的计数-1。  
缺点是，有些独立的引用环没法回收。此时需要额外的方式来回收，比如“弱引用”，比如特殊的算法回收引用环。Perl，Python，PHP就是使用类似的算法来回收引用环。  
优点是，执行快，不需要打断应用程序执行。  

### tracing算法(Tracing Collector) 或 标记-清除算法(mark and sweep)

![mark and sweep](https://farm1.staticflickr.com/313/19380455499_60b373cbf7.jpg)  

从根节点开始扫描可达对象，称为标记；将不可达对象删除，称为清理；因为清理会产生内存碎片，所以需要额外的整理动作（compacting）。  
java中可作为GC Root的对象有：  
1.虚拟机栈中引用的对象（本地变量表）
2.方法区中静态属性引用的对象
3. 方法区中常量引用的对象
4.本地方法栈中引用的对象（Native对象）

### copying算法(Compacting Collector)

将内存分成两块，从root开始找到所有可达对象，拷贝到空闲区，然后清理当前使用区。一般需要暂停应用程序执行。  
这个算法可以作为标记-清楚算法的补充。  

### generation算法(Generational Collector)

![generation](https://farm1.staticflickr.com/286/19571763781_69ca22ff15.jpg)  

术语解释：  

> 分代  
> Young Generation：eden区，两个survivor(survivor0,survivor1)区，容量比约为8:1:1。  
> Old Generation（Tenured Space）  
> Permanent Generation  

> gc种类  
> Young GC (Minor GC/ygc)  
> Full GC (Major GC/fgc)  

大部分对象在Eden区生成，ygc将Eden区可达对象复制到survivor0，并清空Eden区。当survivor0满了，ygc时将Eden区、survivor0区可达对象复制到


## 几种常见垃圾收集器

## gc日志实例分析

## 待解决问题

1. 新生代触发young gc的条件？
2. 

主要参考：  
[深入理解java垃圾回收机制](http://www.cnblogs.com/sunniest/p/4575144.html)  
[]()  

[淘宝搜索-JVM GC简介和实例](http://www.searchtb.com/2013/07/jvm-gc-introduction-examples.html)  
https://blogs.oracle.com/poonam/entry/understanding_cms_gc_logs
https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/cms.html


[gc日志分析](http://coderbee.net/index.php/jvm/20131216/646) 
[JVM实用参数（八）GC日志](http://ifeve.com/useful-jvm-flags-part-8-gc-logging/) 

[不同的垃圾回收器的比较](http://it.deepinmind.com/gc/2014/09/12/garbage-collectors-serial-vs-parallel-vs-cms-vs-the-g1-and-whats-new-in-java-8.html)  
[Swap对响应时间敏感应用的影响](http://blog.hesey.net/2014/05/swap-impact-on-rt-sensitive-apps.html?utm_source=rss&utm_medium=rss&utm_campaign=swap-impact-on-rt-sensitive-apps)  
[full gc是否真的存在](http://it.deepinmind.com/gc/2015/03/03/minor-gc-vs-major-gc-vs-full-gc.html)  
[JVM的GC简介和实例](http://www.searchtb.com/2013/07/jvm-gc-introduction-examples.html)  
[一次CMS GC的调优工作](http://blog.hesey.net/2013/11/cms-gc-optimize.html) 
[聊聊JVM（四）深入理解Major GC, Full GC, CMS](http://blog.csdn.net/iter_zc/article/details/41825395)   
[《JVM故障诊断指南》之4 —— Java 8:从持久代到metaspace](http://ifeve.com/jvm-troubleshooting-guide-4/)  
[JVM实用参数（八）GC日志](http://ifeve.com/useful-jvm-flags-part-8-gc-logging/) 

 
  


系列文章：  
http://it.deepinmind.com/categories.html#GC-ref


