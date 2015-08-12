---
title: osx-制作USA安装盘，并迁移老MAC
date: '2015-07-10'
description:
categories: 'osx'
---

# 制作安装盘并重装系统

1. 直接到app store下载最新版的osx系统：https://itunes.apple.com/cn/app/os-x-yosemite/id915041082?mt=12
2. 命令行执行下面的命令，制作安装盘，注意将U盘路径替换成自己，系统路径也需要匹配。这一步需要比较长时间，耐心等待。`sudo /Applications/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/QY --applicationpath /Applications/Install\ OS\ X\ Yosemite.app`
3. 插上U盘，按下Option键重启电脑，选择重新安装系统。

# 从老电脑将内容迁移过来

<<<<<<< HEAD
https://support.apple.com/en-us/HT204350  
=======
官网说的两台mac连在同一wifi上就能成功，试了没成功。后来找到一个方式：    

1. 老mac创建一个network，并连上这个网络  
![](https://farm1.staticflickr.com/328/19577943046_2f1a554344.jpg)  

![](https://farm1.staticflickr.com/487/19577980266_0cf6d9ea45.jpg)  

2. 老mac设置网络共享，注意这里可能需要选择Share your connection from : ThunderBolt Ethernet，To computers using: Wi-Fi。并注意打开防火墙  

![](https://farm4.staticflickr.com/3803/18981584834_c24a7b9168.jpg)  

3. 新mac连上老mac的网络  
4. 两者打开Migration Assitant传递数据
5. 等待好几个小时直到完成。

因为无线是在太慢，如果有ThunderBolt to ThunderBolt转接线，就不用这么麻烦了。  

参考：  
[Move your content to a new Mac](https://support.apple.com/en-us/HT204350)  
[use-migration-assistant-with-macbook-air-over-airport-wifi](http://marcushesse.com/2012/use-migration-assistant-with-macbook-air-over-airport-wifi/)    
>>>>>>> 98d4643bd95cbf846411e2339f24a4c4ec6ec42c
