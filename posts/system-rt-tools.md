---
title: 系统rt排查常用工具
date: '2015-05-07'
description:
categories: '性能'
---

# wireshark

## 特殊的filter
参考：https://www.wireshark.org/lists/wireshark-users/201211/msg00002.html

```
# http GET方法，切url符合一定的规则
http.request.method == "GET" && http.request.full_uri matches "index\.php\?.*="
http.request.method == "GET" && http.request.full_uri contains "index.php?"
```

## “TCP segment of a reassembled PDU”

参考：http://blog.csdn.net/rossini23/article/details/5424850  
http://blog.csdn.net/hldjf/article/details/7450565  


“TCP segment of a reassembled PDU”指的不是IP层的分片，IP分片在wireshark里用“Fragmented IP protocol”来标识。详细查了一下，发现“TCP segment of a reassembled PDU”指TCP层收到上层大块报文后分解成段后发出去。  

# BTrace

# 网卡 

如何查看是否1GB网卡
http://www.ctohome.com/FuWuQi/ea/517.html




