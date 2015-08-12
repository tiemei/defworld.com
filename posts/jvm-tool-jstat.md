---
title: jvm工具-jstat
date: '2015-07-16'
description:
categories: 'java'
---

`jstat [ generalOption | outputOptions vmid [interval[s|ms] [count]] ]`  

generalOption只有两个选项，且使用时不能加其他参数及选项：  

```
-help Display help message
-options Display list of statistics options. 
```

outputOptions有多个选项，另外可以加上-h, -t, and -J，接在outputOptions后面:  

```
Option              Displays...
class               Statistics on the behavior of the class loader.
compiler            Statistics of the behavior of the HotSpot Just-in-Time compiler.
gc                  Statistics of the behavior of the garbage collected heap.
gccapacity          Statistics of the capacities of the generations and their corresponding spaces.
gccause             Summary of garbage collection statistics (same as -gcutil), with the cause of the last and current (if applicable) garbage collection events.
gcnew               Statistics of the behavior of the new generation.
gcnewcapacity       Statistics of the sizes of the new generations and its corresponding spaces.
gcold               Statistics of the behavior of the old and permanent generations.
gcoldcapacity       Statistics of the sizes of the old generation.
gcpermcapacity      Statistics of the sizes of the permanent generation.
gcutil              Summary of garbage collection statistics.
printcompilation    HotSpot compilation method statistics.
```

```
-h n
Display a column header every n samples (output rows), where n is a positive integer. Default value is 0, which displays the column header above the first row of data.

-t
Display a timestamp column as the first column of output. The timestamp is the time since the start time of the target JVM.

-JjavaOption
Pass javaOption to the java application launcher. For example, -J-Xms48m sets the startup memory to 48 megabytes. For a complete list of options, see java - the Java application launcher
```


例子：`jstat -gcoldcapacity -t 21891 250 3`  

参考： 

[jstat - Java Virtual Machine Statistics Monitoring Tool](http://docs.oracle.com/javase/7/docs/technotes/tools/share/jstat.html)  

