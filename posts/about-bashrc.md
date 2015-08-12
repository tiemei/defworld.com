---
title: 【大部分转载】.bashrc .bash_profile等文件区别
date: '2013-06-28'
description: bashrc
categories: 'linux'
tags: 

---
**/etc/profile**  
全局，用户第一次登录执行一次  
会从/etc/profile.d目录的配置文件中搜集shell的设置.给用户设置环境变量的地方，不管用户使用的什么shell(bash、zsh...)
  
**/etc/bashrc**  
全局，bash shell配置，一般由/etc/profile调用  
 
**~/.bash_profile**  
似有，用户登录(ssh远程登陆/本地新开一个terminal)时执行一次
一般会调用~/.bashrc  
  
**~/.bashrc**  
似有，每次执行bash命令时自动调用，或者非交互方式进入bash时运行  
  
**~/.bash_logout**  
当每次退出系统(退出bash shell)时,执行该文件.  

而如果系统是 ubuntu 或者 debian 的话, 就不会有 /etc/bashrc 而会有 /etc/bash.bashrc 文件.  

## shell的模式

系统的 shell 有很多种, 比如 bash, sh, zsh 之类的, 如果要查看某一个用户使用的是什么 shell 可以通过 finger [USERNAME] 命令来查看. 我们这里只说 shell 是 bash 的情况, 因为如果是 sh 或者其他 shell 显然不会运行 bashrc 的.

### login shell 和 no-login shell

“login shell” 代表用户登入, 比如使用 “su -l username“ 命令, 或者用 ssh 连接到某一个服务器上, 或者开启termal，都会使用该用户默认 shell 启动 login shell 模式.  
该模式下的 shell 会去自动执行 /etc/profile 和 ~/.profile 文件, 但不会执行任何的 bashrc 文件, 所以一般再 /etc/profile 或者 ~/.profile 里我们会手动去 source bashrc 文件.  
而 no-login shell 的情况是我们在终端下直接输入 bash 或者 bash -c “CMD” 来启动的 shell.  
该模式下是不会自动去运行任何的 profile 文件.  

### interactive shell 和 non-interactive shell

interactive shell 是交互式shell, 顾名思义就是用来和用户交互的, 提供了命令提示符可以输入命令.  
该模式下会存在一个叫 PS1 的环境变量, 如果还不是 login shell 的则会去 source /etc/bash.bashrc 和 ~/.bashrc 文件  
non-interactive shell 则一般是通过 bash -c “CMD” 来执行的bash.  
该模式下不会执行任何的 rc 文件, 不过还存在一种特殊情况这个我之后详细讲述  

## shell模式与文件执行的关系

login shell会去执行profile文件（no-login shell不会执行），而interactive no-login shell会去执行profile（interactive login shell不会执行）。  
特例是： 通过 “ssh server CMD” 执行的命令 或 通过程序执行远程的命令。  

### SSH login, sudo su - [USER] 或者 mac 下开启终端

ssh 登入和 su - 是典型的 interactive login shell, 所以会有 PS1 变量, 并且会执行  

```
/etc/profile
~/.profile
```

### 在命令提示符状态下输入 bash 或者 ubuntu 默认设置下打开终端

这样开启的是 interactive no-login shell, 所以会有 PS1 变量, 只会执行  

```
/etc/bash.bashrc
~/.bashrc
```

### 通过 bash -c “CMD” 或者 bash BASHFILE 命令执行的 shell

这些命令什么都不会执行, 也就是设置 PS1 变量, 不执行任何 RC 文件

### 最特殊! 通过 “ssh server CMD” 执行的命令 或 通过程序执行远程的命令

这是最特殊的一种模式, 理论上应该既是 非交互 也是 非登入的, 但是实际上他不会设置 PS1, 但是还会执行  

```
/etc/bash.bashrc
~/.bashrc
```

这里还有一点值得注意的是 /etc/bashrc 任何情况下都不会执行.  

## 小技巧

* `/etc/profile`中加入`export system_profile_loaded=1`，其他文件就可以根据 $system_profile_loaded 来判断是否已经载入过 profile


参考：  

[理解 bashrc 和 profile](https://wido.me/sunteya/understand-bashrc-and-profile/)  