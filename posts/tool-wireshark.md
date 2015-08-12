---
title: tool-wireshark
date: '2015-08-03'
description:
categories: 'tool'
---
## 常见技巧

* 安装，通过官网下载wireshark，然后提示你下载x11，到x11官网下载x11，然后重启电脑即可使用
* 使用`Statistics -> IO Graph`工具来对比多种filter条件下的连续趋势
* 使用`Statistics -> Protocol Hierarchy`显示各种协议的概览



## IO Graphs

这里的不是所有filter跟函数都使用与filter框中使用。  图编号越低表示在前台显示，可能会覆盖较高图号。    

### 常用filter

```
tcp.analysis.lost_segment：表明已经在抓包中看到不连续的序列号。报文丢失会造成重复的ACK，这会导致重传。

tcp.analysis.duplicate_ack：显示被确认过不止一次的报文。大凉的重复ACK是TCP端点之间高延时的迹象。

tcp.analysis.retransmission：显示抓包中的所有重传。如果重传次数不多的话还是正常的，过多重传可能有问题。这通常意味着应用性能缓慢和/或用户报文丢失。

tcp.analysis.window_update：将传输过程中的TCP window大小图形化。如果看到窗口大小下降为零，这意味着发送方已经退出了，并等待接收方确认所有已传送数据。这可能表明接收端已经不堪重负了。

tcp.analysis.bytes_in_flight：某一时间点网络上未确认字节数。未确认字节数不能超过你的TCP窗口大小（定义于最初3此TCP握手），为了最大化吞吐量你想要获得尽可能接近TCP窗口大小。如果看到连续低于TCP窗口大小，可能意味着报文丢失或路径上其他影响吞吐量的问题。

tcp.analysis.ack_rtt：衡量抓取的TCP报文与相应的ACK。如果这一时间间隔比较长那可能表示某种类型的网络延时（报文丢失，拥塞，等等）
```

### 常用函数

```
max
min 
ava

count 
```

参考：  

[一站式学习wireshark系列](http://blog.jobbole.com/70907/)  
[一站式学习Wireshark（四）：网络性能排查之TCP重传与重复ACK](http://blog.jobbole.com/71427/)  
[一站式学习Wireshark（五）：TCP窗口与拥塞处理](http://blog.jobbole.com/71925/)  
[一站式学习Wireshark（六）：狙击网络高延时点](http://blog.jobbole.com/73477/)       