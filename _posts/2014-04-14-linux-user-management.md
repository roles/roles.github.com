---
layout: post
title: "linux用户管理"
description: ""
category: 
tags: ["linux"]
---
{% include JB/setup %}

##文件和目录的权限

每个文件和目录都有9为权限符，如`rwxr-x---(750)`，所有者以及所有者用户组   
前三位表示所有者用户的权限为读写和执行   
中间三位表示所有者用户组的权限为读和执行  
最后三位表示其他用户的权限为不可读写执行

对于目录而言，写权限即所有涉及修改目录内文件结构的操作  
执行权限则为能否进入该目录成为工作目录

修改的命令主要有`chmod，chgrp，chown`

##添加和修改用户信息

在/etc/passwd中保存了用户的信息   
如dmtsai:x:503:504::/home/dmtsai:/bin/bash表示  
用户名为dmtsai  
UID为503  
GID为504  
工作目录为/home/dmtsai  
默认shell为/bin/bash  

一个用户可以同时属于多个组，但初始分组是当用户登录时设定的分组
{% highlight bash %}
#创建分组
groupadd data

#添加用户，创建/home/yys作为其默认目录
useradd -g data yys

#添加用户，不创建目录，以/home/yys为目录并且以data为初始分组，www为有效分组
useradd -g data -G www -M -d /home/yys yys

#设定用户密码，root执行
passwd yys

#添加heatmap作为有效分组
usermod -G heatmap yys

#查看用户信息
finger yys
id yys

#切换当前用户的主分组为www
newgrp www
{% endhighlight %}
