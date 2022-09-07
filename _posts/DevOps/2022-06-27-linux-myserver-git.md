---
layout: post
title: git常用命令、报错及服务器上的配置
---

本文用于梳理不同地方git的使用习惯，含gitlab、github，留作备查备忘。



## 一、须知

1、服务器环境：[服务端配置备忘]({{ site.baseurl }}{% link _posts/DevOps/2022-04-19-linux-myserver-setting.md %})

2、本地git配置：[同一台Mac配置多个github账户]

 

## 二、环境配置逻辑

本地共配置了两个github的root账户，一个gitlab的developer账户，详情见注2。

github都在外网，root账户方便对master分支的增删改查。优点是提交方便，缺点是版本繁多，难以回滚等，但私人项目很少用的上，所以需要多打tag弥补无法跟踪重要版本的问题。

gitlab在内网，为了写严谨的项目，developer账户不可直接修改master分支，需要创建私人分支，提交合并请求，再由root账户在网页上合并。



## 三、常用命令

| git pull                                                     | 更新                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| git status                                                   | 当前状态                                                     |
| git commit -am ""                                            | 提交并添加注释                                               |
| git add .                                                    |                                                              |
| git push                                                     |                                                              |
| git rm --cached a.a                                          | 移除文件(只从缓存区中删除)                                   |
| git branch                                                   | 查看本地分支                                                 |
| git branch init-zhaotong-20220627                            | 创建本地分支init-zhaotong-20220627                           |
| git checkout master<br />git checkout init-zhaotong-20220627 | 切换到本地分支master<br />或init-zhaotong-20220627           |
| git push --set-upstream origin init-zhaotong-20220627        | 新建远程分支init-zhaotong-20220627                           |
| 开发合并最好在页面端操作<br />提交合并前必须仔细阅读每行改动代码，确保万无一失 | 项目-->资源-->分支-->在待合并分支上提交合并请求-->填写提交细节<br />study-demo-->Repository-->branch-->init-zhaotong-20220627-->Merge Request-->填写提交细节 |
| 管理员合并，一次合并完成                                     | root账户，审核完提交的代码，提交注释、大版本提交tag，合并，删除已提交的分支 |



## 四、遇到的问题

1、gitlab 向 master 分支提交 push 显示无法跟踪

```git
fatal: 当前分支 master 没有对应的上游分支。
为推送当前分支并建立与远程上游的跟踪，使用

    git push --set-upstream origin master
```

可拉去更新不能推送，按照程序执行了也没有用。因为本地developer权限没有推送权限，新建本地分支，推送到远端，再发起合并即可。