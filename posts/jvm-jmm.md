---
title: JVM——JMM，java内存模型
date: '2015-07-08'
description:
categories: 'java'
---

## 简介JMM与硬件内存架构的关系

java虚拟机包含一个完整的计算机模型，java内存模型跟硬件内存架构之间的关系是：  

![jmm-cpu](https://farm1.staticflickr.com/328/19329026068_2c37a4027e.jpg)  

stack、heap有可能出现在寄存器、cpu缓存层、主内存中，也因此可能出现可见性（某个线程对一个对象的修改，不被另一个线程看见）、race conditions（竞争状态，修改同一个对象时可能会出现问题）。  

## 

参考：  

[Java内存模型](http://ifeve.com/java-memory-model-6/)  
[深入理解java内存模型系列文章](http://ifeve.com/java-memory-model-0/)  
[Java内存模型FAQ](http://ifeve.com/jmm-faq/)  
[同步和Java内存模型](http://ifeve.com/syn-jmm/)  
[遗失的java堆内存](http://it.deepinmind.com/jvm/2015/02/13/jvm-having-access-to-less-memory-than-xmx.html)  
[java对象内存布局](http://coderbee.net/index.php/java/20140811/979)  
<<<<<<< HEAD
=======
 
>>>>>>> 98d4643bd95cbf846411e2339f24a4c4ec6ec42c


