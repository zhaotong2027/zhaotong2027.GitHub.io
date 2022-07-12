---
layout: post
title: spring 源码
---

#### 一、目的

##### 形成习惯：

1、常看抽象类的类图：类名右击 -> diagrams -> show diagram 或快捷键 option+command+u

2、常看接口实现化：接口左侧向上向下带 I 标签的绿色箭头

3、多看类的结构：Idea右侧的Structure面板（idea自动生成父类方法，选中父类，ctrl+o）



#### 二、本地配置

参考：https://blog.csdn.net/u013713832/article/details/81227701

1、下载Java1.8并配置好（目前最新的Spring也是java1.8，部分特性1.9就没有了，最好下载Java1.8）

2、拉取源码：https://github.com/spring-projects/spring-framework.git

打开spring-framework-5.0.x\gradle\wrapper\gradle-wrapper.properties查看源码使用的gradle的版本号

```
distributionUrl=https\://services.gradle.org/distributions/gradle-7.4.2-bin.zip
```

3、[下载](https://gradle.org/releases/)、配置 gradle

brew下载gradle 7.4.2，在 .zrcsh 中配置，命令 gradle -v 查看是否安装成功

4、Idea里调试



#### 三、问题

1、堆栈溢出

全局gradle.properties文件设置配置：

```
version=5.2.9.BUILD-SNAPSHOT
## 设置此参数主要是编译下载包会占用大量的内存，可能会内存溢出
org.gradle.jvmargs=-Xmx2048M
## 开启 Gradle 缓存
org.gradle.caching=true
## 开启并行编译
org.gradle.parallel=true
## 启用新的孵化模式
org.gradle.configureondemand=true
## 开启守护进程 通过开启守护进程，下一次构建的时候，将会连接这个守护进程进行构建，而不是重新fork一个gradle构建进程
org.gradle.daemon=true
```

2、声明调用失败

高版本java已经去掉了annotation，需要在使用该声明项目的build.gradle里添加依赖：

```
    compile group: 'javax.annotation', name: 'javax.annotation-api', version: '1.3.2'
```

