---
title: jvm工具-hprof
date: '2015-07-16'
description:
categories: 'java'
---

jvm启动的时候需要指定参数，去加载 JVM native agent library ，这样才能打开hprof记录；在j2se 1.5之后，hprof是基于Java Virtual Machine Tool Interface ([JVM TI](http://docs.oracle.com/javase/7/docs/technotes/guides/jvmti/index.html))实现的。  

HPROF is capable of presenting CPU usage, heap allocation statistics, and monitor contention profiles.  In addition, it can also report complete heap dumps and states of all the monitors and threads in the Java virtual machine.   
可查看帮助文档：  

```
$ java -agentlib:hprof=help

     HPROF: Heap and CPU Profiling Agent (JVMTI Demonstration Code)

hprof usage: java -agentlib:hprof=[help]|[<option>=<value>, ...]

Option Name and Value  Description                    Default
---------------------  -----------                    -------
heap=dump|sites|all    heap profiling                 all
cpu=samples|times|old  CPU usage                      off
monitor=y|n            monitor contention             n
format=a|b             text(txt) or binary output     a
file=<file>            write data to file             java.hprof[{.txt}]
net=<host>:<port>      send data over a socket        off
depth=<size>           stack trace depth              4
interval=<ms>          sample interval in ms          10
cutoff=<value>         output cutoff point            0.0001
lineno=y|n             line number in traces?         y
thread=y|n             thread in traces?              n
doe=y|n                dump on exit?                  y
msa=y|n                Solaris micro state accounting n
force=y|n              force output to <file>         y
verbose=y|n            print messages about dumps     y

Obsolete Options
----------------
gc_okay=y|n

Examples
--------
  - Get sample cpu information every 20 millisec, with a stack depth of 3:
      java -agentlib:hprof=cpu=samples,interval=20,depth=3 classname
  - Get heap usage information based on the allocation sites:
      java -agentlib:hprof=heap=sites classname

Notes
-----
  - The option format=b cannot be used with monitor=y.
  - The option format=b cannot be used with cpu=old|times.
  - Use of the -Xrunhprof interface can still be used, e.g.
       java -Xrunhprof:[help]|[<option>=<value>, ...]
    will behave exactly the same as:
       java -agentlib:hprof=[help]|[<option>=<value>, ...]

Warnings
--------
  - This is demonstration code for the JVMTI interface and use of BCI,
    it is not an official product or formal part of the JDK.
  - The -Xrunhprof interface will be removed in a future release.
  - The option format=b is considered experimental, this format may change
    in a future release.
```


可以使用eclipse插件MAT（http://www.eclipse.org/mat/）分析堆栈；原来的分析工具HAT（Heap Analysis Tool ，http://jovial.com/hat/）从java 1.6开始已经集成到发行版中，成为[jhat](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/jhat.html)。这里有jhat使用的[一个例子](http://zeroturnaround.com/rebellabs/the-6-built-in-jdk-tools-the-average-developer-should-learn-to-use-more/)。      

参考:  
[HPROF: A Heap/CPU Profiling Tool](http://docs.oracle.com/javase/7/docs/technotes/samples/hprof.html)  

