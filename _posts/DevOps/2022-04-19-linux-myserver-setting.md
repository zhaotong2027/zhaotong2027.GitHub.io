---
layout: post
title: 服务端配置备忘
tags: 运维
---

# 服务端配置备忘

## CentOS7.9

| username   | root |
| :----- | ----------: |
| IP |  192.168.1.102  |
|  |  /dev/sda1  |
|  |  /boot |
| 测试通过的安装包位置 |  ~ |

## 域名

| 域名  | 备注 |
| :----- | ----------: |
| zhaotong.fun | GitHub.io  |
| confluence.zhaotong.fun   | nginx:8090 |
| crowd.zhaotong.fun   | nginx:8095 |
| jira/server.zhaotong.fun   | nginx:8093 |
| ssl证书位置 | /usr/local/nginx/conf/cert/ |

https://blog.csdn.net/cuman/article/details/112394423

## 端口配置

| 端口号   | 软件名 |     用户名 |
| :----- | :--: | -------: |
| 3306 |  mysql  | root |
| 8080 |  jinkins  | admin |
| 8082 |  gitlab  | root |
| 8095 |  crowd  | crowd |
| 8093 |  jira-HTTP  | jira |
| 8090 |   confluence  | confluence / admin |
| 8005 |  jira-RMI(Control) |  |
| 8000 |  confluence-RMI(Control) |  |
| 80 |  nginx  |  |
|  |  redis  |  |
| 10888 |  Don't Starve  |  |
| 10999 |  Don't Starve  |  |
| 10995 |  Don't Starve  |  |
| 8079 |  禅道  | Apach |
| 8077 |  禅道  | mysql |

##### 各软件位置及常用命令

| 软件名 | 重启命令 | 是否开机启动 |
| :---- | :-------: | -------: |
| nginx | nginx -s reload / nginx stop / nginx quit | Y |
| mysql | systemctl restart mysqld / systemctl start mysqld / $ systemctl status mysqld / mysql -u root -p | Y |
| crowd | /opt/atlassian/crowd/start_crowd.sh /opt/atlassian/crowd/stop_crowd.sh  | Y |
| confluence | /opt/atlassian/confluence/bin/start-confluence.sh /opt/atlassian/confluence/bin/stop-confluence.sh | Y |
| jira | /opt/atlassian/jira/bin/stop-jira.sh /opt/atlassian/jira/bin/start-jira.sh / /opt/atlassian/jira/uninstall | Y |


| 启动顺序 | 软件名   | 位置 | 配置文件位置 |    日志位置 |
| :----- | :--: | :-------: | :-------: | :-------: |
| 1 | nginx | /usr/local/nginx-1.8.0 | /usr/local/nginx/conf/ |      |
| 2 | mysql | /usr/bin/mysql | /etc/my.cnf /etc/profile | /var/log/mysqld.log |
| 5 | jira | /opt/atlassian/jira /var/atlassian/application-data/jira | /etc/profile /opt/atlassian/jira/conf/server.xml  |  |
| 4 | confluence | /opt/atlassian/confluence /var/atlassian/application-data/confluence | /opt/atlassian/confluence/confluence/WEB-INF/classes/confluence-init.properties /opt/atlassian/confluence/conf/server.xml | /var/atlassian/application-data/confluence/logs/atlassian-confluence.log |
| 3 | crowd | /opt/atlassian/crowd /var/crowd-home | /opt/atlassian/crowd/crowd-webapp/WEB-INF/classes/crowd-init.properties | /var/crowd-home/logs/atlassian-crowd.log |
| - | gitlab | /etc/gitlab | /etc/gitlab/gitlab.rb /opt/gitlab/embedded/service/gitlab-rails/config/gitlab.yml | /var/log/gitlab |


https://cloud.tencent.com/developer/article/1841809

https://www.cnblogs.com/ling-yu-amen/p/10487739.html


## 换网设置ip的位置
```
# 静态ip
vim /etc/sysconfig/network-scripts/ifcfg-ens32 #最后一个是网卡名
bootproto=static #修改
onboot=yes #修改
IPADDR=192.168.1.160 #新增
NETMASK=255.255.255.0 #新增
GATEWAY=192.168.1.1 #新增
DNS1=119.29.29.29 #新增
DNS2=8.8.8.8 #新增

# 服务器
vim /var/crowd-home/crowd.properties  # 第4、8行
vim /var/crowd-home/crowd.cfg.xml  # 第17行，连数据库地址
vim /opt/atlassian/confluence/conf/server.xml  # baseurl存在atlassian服务器上，导致该文件改了也没用(这个版本已经不需要改sever.xml，用域名代替ip)
vim /opt/gitlab/embedded/service/gitlab-rails/config/gitlab.yml  # 第13行，host 修改gitlab地址
vim /etc/gitlab/gitlab.rb  # 第13行，external_url 修改旧项目地址
sudo gitlab-ctl reconfigure  # 修改完重启
```
库crowd里表cwd_application_address修改数据库ip

```
# 本机
vim ~/.ssh/config  # HostName

# 阿里云平台/域名解析
```

参考：[换ip配置server](https://mp.weixin.qq.com/s?__biz=MzU0NzUxMzYzOA==&mid=2247483731&idx=1&sn=d509821104bc28f60b8e4c20ef42b03d&chksm=fb4c751acc3bfc0c050b2700f2cb3dd0a0f821c79560a18ad9ba7cb92cd7db677a9fa2643f77&token=1232437534&lang=zh_CN#rd)


## 常用命令

| 操作   | 命令 |     备注 |
| :----- | :--: | -------: |
| 查看日志最新100行 |  tail -n 100 XX.log  | |
| 查找文件 |  find / -name XX  | |
| 查看端口XX |  lsof -i:XX  | |
| 软件XX版本 |  rpm -qa|grep XX  | |
| 安装 |  yum install -y XX  | |
| 分区 |  df -h  | |
| 磁盘 |  fdisk -l  | |
| 新建 |  m  | |
| 新建文件夹 |  mkdir  | |
| 新建配置文件 |  touch  | |
| 删除 |  rm -rf | |
| 删除空文件夹 |  rmdir | |
| 重命名 |  mv XX XX.bak  | |
| vim查找 |  / 或 ？ XX  | |
| 强制取消 |  ctrl+c  | |
| 上翻页 |  shift+pageup  | |
| 查看当前目录 |  pwd  | |
| 返回上次目录 |  cd -  | |
| XX文件夹下数 |  tree XX  | |

## self

| 软件名   | 账户 |     备注 |
| :----- | :--: | -------: |
| mysql |  zhaotong  | |
| gitlab |  zhaotong  | iloveu |
| crowd |  zt/zcx  | |
| confluence |  -  | |
| jira |  jira  | |
| jenkins |  zt  | |
| redis |    | 123456 |
|  |          |        |
|            |          | |
