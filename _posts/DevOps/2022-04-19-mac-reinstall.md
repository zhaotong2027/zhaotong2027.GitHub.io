---
layout: post
title: Mac重装系统配置
tags: 运维
---

# Mac重装系统

注意事项：

1、不要选择当前最新版本软件，而是选择落后最新一两个版的稳定版
2、老Mac OS的shell用的bash，新版本用zsh

- 环境：MacBook Pro (15-inch, 2017)
- 原装系统：Big Sur

## 1. 准备
备份好各种数据，用磁盘工具抹掉数据

关机

开机的同时按command+r

再次磁盘工具抹掉数据

安装原装系统（大约2h，视网络而定，期间不可断电断网）

初始化设置

升级系统到最新版（下载到安装约2h）

AppStore安装Xcode（约2h）

## 2. 安装基础环境
- 2.1 安装homedrew

（参考：https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/）

新系统的zshrc文件需要生成：touch ~/.zshrc

homedrew的安装路径是/usr/local/Homebrew

- 2.2 安装homedrew-cask

（参考：https://www.cnblogs.com/boboblue/p/13292533.html、https://www.aimtao.net/homebrew/）

terminal执行

```
brew tap homebrew/homebrew-cask
brew install brew-cask-completion
```

- 2.3 drew安装最新版java和java11

```
brew search java
brew install java
sudo ln -sfn /usr/local/opt/openjdk/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk
java --version
brew install java11
sudo ln -sfn /usr/local/opt/openjdk@11/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-11.jdk
java --version

```

设置环境变量~/.zshrc（配完记得启用生效source ~/.zshrc）

```
# java切换环境
# JDK 18 20220401最新jdk默认在openjdk.jdk,不带版本号
export JAVA_18_HOME="/Library/Java/JavaVirtualMachines/openjdk.jdk/Contents/Home"
# JDK 11
export JAVA_11_HOME="/Library/Java/JavaVirtualMachines/openjdk-11.jdk/Contents/Home"
#默认JDK 18
export JAVA_HOME=$JAVA_18_HOME
#alias命令动态切换JDK版本
alias jdk18="export JAVA_HOME=$JAVA_18_HOME"
alias jdk11="export JAVA_HOME=$JAVA_11_HOME"
```

- 2.4 drew安装python

```
brew search python
brew install python@3.9
```

设置环境变量~/.zshrc（配完记得启用生效source ~/.zshrc）

```
# Setting PATH for Python 3.9
export PATH="/usr/local/opt/python@3.9/libexec/bin:$PATH"
alias python="/usr/local/bin/python3
```

alias python的地址可以用which python3查找

20230524 python3.9和3.10很多不兼容，切换回3.7或3.8

```
brew search python
brew install python@3.8
```

设置环境变量~/.zshrc（配完记得启用生效source ~/.zshrc）

```
# Setting PATH for Python 3.8
export PATH="/usr/local/opt/python@3.8/libexec/bin:$PATH"
alias python="/usr/local/bin/python3
```

2.4.2 安装pip

```
curl https://bootstrap.pypa.io/get-pip.py | python3
pip3 --version
```

2.4.3 安装pip_search

```
pip install pip-search
pip_search tensorflow
```

- 2.5 drew安装node.js

```
brew search node
brew install node@14
```
设置环境变量~/.zshrc（配完记得启用生效source ~/.zshrc）
```
export PATH="/usr/local/opt/node@14/bin:$PATH"
```
验证
```
node -v

v14.19.1

npm -v

6.14.16
```

## 3.brew安装软件
- 3.1 sublime-text

```
brew search sublime
brew install sublime-test --cask
```

- 3.2 git管理 sourcetree (弃置)

```
brew search sourcetree
brew install sourcetree --cask
brew uninstall sourcetree --cask
```

另：sourcetree 生成的公钥配置中（vim ~/.ssh/config）需要手动补充用户的username，useremail

调教git：https://blog.csdn.net/qq_43768946/article/details/90411154

- 3.3 xmind

```
brew search xmind
brew install xmind --cask
brew uninstall xmind --cask
```

- 3.4 postman

```
brew search postman
brew install postman --cask
```

- 3.5 jmeter

```
brew search jmeter
brew install jmeter
brew info jmeter
jmeter # 启动jmeter
```

## 4.网页下载软件
- 4.1 jetbrains全家桶 

下载2020.3版，https://www.jetbrains.com/pycharm/download/other.html

激活，http://www.520xiazai.com/soft/jetbrains-2020-pojie.html

- 4.2 adobe 全家桶

https://sameen.art/software-tool/adobe2020vposy.html

- 4.3 vssh

下载地址，https://www.osxwin.com/s/vssh-1-11-1

**软件损坏等问题，https://mac.orsoon.com/news/1087137.html**

- 4.4 navicat

https://xclient.info/s/navicat-premium.html

- 4.5 mindmanager

https://lapulace.com/mindmanager2020.html saner/123456

- 4.6 obsidian

https://obsidian.md/

## 5.开发环境安装
- 5.1 allure
```
brew search allure
brew install allure
```
设置环境变量~/.zshrc（配完记得启用生效source ~/.zshrc）

```
export PATH="/usr/local/Cellar/allure/2.17.3/bin:$PATH"
```
参考：https://blog.csdn.net/z1107445981/article/details/118468592

- 5.2 macdown(markdown的mac工具软件)

```
brew search macdown
brew install macdown --cask
brew uninstall macdown --cask
```

被支持mermaid画图的Typora取代，在Typora网上下载即可，后期收费版本brew cask无法直接下载。

- 5.3 ruby(mac中rubygems和ruby是一体的)

```
brew search ruby
brew install ruby
```

设置环境变量~/.zshrc（配完记得启用生效source ~/.zshrc）

```
export PATH="/usr/local/opt/ruby/bin:$PATH"
```

验证

```
ruby -v
gem -v
```

- 5.4 jekyll

```
gem install jekyll （ruby确保配置成功）

Successfully installed jekyll-4.2.2
```

设置环境变量~/.zshrc（配完记得启用生效source ~/.zshrc）

```
#jekyll
export PATH=${PATH}:/Users/zhaotong/.local/share/gem/ruby/3.1.0/bin
```

验证

```
jekyll -v
```

卸载hexo

```
npm uninstall hexo-cli -g
npm uninstall hexo
```

- 5.5 jupyter

```
pip3 list
pip3 install jupyter
jupyter notebook  #	启动
```

- 5.6 tesseract（ocr-google）

```
brew search tesseract
brew install tesseract
tesseract # 启动
```

- 5.7 maven

```
brew search maven
brew install maven
```

验证（高版本不用配置环境了）

```
mvn -v
```

- 5.8 gradle

```
brew search gradle
brew install gradle
```

设置环境变量~/.zshrc（配完记得启用生效source ~/.zshrc）

```
#gradle
export PATH=${PATH}:/Users/zhaotong/.local/share/gem/ruby/3.1.0/bin
```

验证

```
gradle -v
```

- 5.9 selenium

```
pip --version
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple selenium

下载对应版本的webdriver
```

- 5.10 curl

```
brew search curl
brew install curl
...
echo 'export PATH="/usr/local/opt/curl/bin:$PATH"' >> ~/.zshrc
```

设置环境变量~/.zshrc（配完记得启用生效source ~/.zshrc）

```
#curl
export PATH="/usr/local/opt/curl/bin:$PATH"
```

- 5.11 mysql

官网下载安装，账密：root 爱你我，现运行mysql版本为V8.0.30

参考：https://zhuanlan.zhihu.com/p/360858309

常用命令：

启动数据库	sudo mysql.server start/stop/status

mysql -u root -p

查看配置文件位置：mysql --help|grep my.cnf	当前使用的配置文件为 /usr/local/mysql/etc/my.cnf

- 5.12 jira

官网下载安装，需要的java版本为11或8，现运行java版本为11，jira版本为V8.8.0

JIRA Home Directory：/Users/zhaotong/jira

参考：https://zhuanlan.zhihu.com/p/114192816

常用命令：

启动jira	/usr/local/atlassian-jira-software-8.8.0-standalone/bin/start-jira.sh	默认地址	localhost:8080

- 5.13 grafana

```
brew install grafana
```

参考：https://cloud.tencent.com/developer/article/1481832#:~:text=%E4%BD%BF%E7%94%A8%E5%A6%82%E4%B8%8B%E5%91%BD%E4%BB%A4%E5%90%AF%E5%8A%A8%20Grafana%20%EF%BC%9A%20brew,services%20start%20grafana%201.3%20%E9%85%8D%E7%BD%AE

5.14 tenserflow

```
python -v
python -m pip --version
pip install tensorflow

pip install tensorflow-gpu
>> activate tensorflow
>> python (activating python shell)
>> import tensorflow as tf
>> hello = tf.constant(‘Hello, Tensorflow!’)
>> sess = tf.Session()
>> print(sess.run(hello))
```

