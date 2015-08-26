---
title: 说说不可见字符
date: '2015-08-21'
description:
categories: 'linux'
---

## 查看

`cat -A file` 显示所有可见、不可见字符;`cat -T`只显示不见的tab；`cat -E`只显示不可见的行尾字符    
`:%!cat -A` vim中调用cat显示  
`:set invlist` vim中直接显示不可见字符  
`:set nolist` vim中隐藏不可见字符  

```
# vim中设置不可见字符的显示方式
set listchars=eol:$,tab:>-,trail:~,extends:>,precedes:<
set list
```


## 输出

`echo $'\001\002'`  


