---
layout: post
title: allure在mac中配置不成功
tags: 运维
---



写这篇文章的起源是想安装allure看好看图表报告，但在安装allure后出现在终端中测试allure版本没问题，在pycharm测试allure版本也没问题，使用配置文件还能简单的生成allure json文件，但程序运行中却始终找不到allure命令。

**先说解决方案，使用zsh作为默认shell，重新去~/.zshrc中配置一遍allure变量。**

再来继续捋事情经过，报错如下：
```
sh: allure: command not found
```
出现问题百度一下，遍寻答案找到了避坑文章： [Mac中配置allure报告，用pycharm打开避坑](https://blog.csdn.net/z1107445981/article/details/118468592) 

一一比对，反复测试了十几遍，配置没问题但问题始终存在，那么自己应该遇上技术黑洞了，肯定是哪块还没整明白。没办法了，只能自己一点点扒问题找原因。

## 从现有条件分析
首先已经确认allure安装了，接下来确认版本能打印出来，环境变量看上去也是配置成功了，配置文件测试时，能使用部分allure功能，说明allure的使用没问题，分析走到了死胡同。
## 从报错问题分析
sh: allure: command not found，sh是shell的脚本，shell找不到allure命令，很大概率指向是环境没配置成功。

apple官网查到了 [在 Mac 上将 zsh 用作默认 Shell](https://support.apple.com/zh-cn/HT208050)
我的macOS是最新的，很明显默认是zsh。由于采用避坑文章的设置，自定义了bash为用户默认shell。
就是说mac升级修改了默认shell，我还在用旧方式，所以运行成功了一半，可能问题就在这。

查一下Mac的默认shell是bash：
```
echo $SHELL

/bin/bash
```
查看系统用户默认shell，root用户用的是sh，其他用户用的是bash
```
cat /etc/passwd | grep sh

root:*:0:0:System Administrator:/var/root:/bin/sh
_sshd:*:75:75:sshd Privilege separation:/var/empty:/usr/bin/false
_update_sharing:*:95:-2:Update Sharing:/var/empty:/usr/bin/false
_mbsetupuser:*:248:248:Setup User:/var/setup:/bin/bash
```

逆向分析，报错的sh是pycharm安装在系统用户下的，调用了sh。但我只设置了bash，虽然能打印，pycharm调用系统用户sh时，是不能找到的。

所以解决方案是根据官方推荐，使用zsh做默认shell，将allure的环境变量配置写到zsh中，问题解决。
（经此一事，在新版本macOS中，我直接弃用bash，所有环境配置都改到zsh中了。）

逆向分析是根据结果推测的，不一定对，虽然问题解决了，说不定是歪打正着呢。
猜测是在macOS中pycharm运行配置文件和进程走的不同命令模式，具体原因还是没最终确认，下次再说吧。

参考文章：[终极 Shell——ZSH](https://zhuanlan.zhihu.com/p/19556676)
