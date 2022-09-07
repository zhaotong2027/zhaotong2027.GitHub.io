---
layout: post
title: 在github上搭建个人博客
tags: 运维
---

# 在github上搭建个人博客

参考：

https://www.zhihu.com/question/20962496

https://zhuanlan.zhihu.com/p/111614119

https://www.yshawlon.cn/1.html



### 准备

- 注册阿里账户
- 填写实名认证信息（审核约一个工作日）
- 挑选并购买域名
- 配置解析 @ 即可
- 申请免费的SSL（审核约一个工作日）



### 设置博客

- 申请github账号
- 创建仓库名为 username.GitHub.io
- sitting->pages，选择theme，设置购买的域名，勾选SSL



### 写博客

丰富博客内容或者简洁版写博客，其中链接为git上该资源链接。



下文有Jekyll版搭建及Hexo版搭建，两版类似建议搭建jekyll项目，理由是和github使用的一致可少走弯路，hexo很多深度定制化没经验的小白整理起来很耗时间，研究搭建固然好，但博客还是专注个人总结，不要忘记初衷。

另：本文建议先阅读Hexo版再根据Jekyll版搭建





搭建环境：Mac+github



### 一、Jekyll版

步骤类似：下载ruby---->rubygems----->jekyll---->创建

下载ruby、rubygem及jekyll参考重装系统5.3、5.4

创建：https://www.cnblogs.com/baiyangcao/p/jekyll_basic.html

### 二、jekyll问题排查

#### 2.1、报错：**cannot load such file -- minima**

```
gem install minima
```

#### 2.2、报错：**cannot load such file -- webrick**

原因：新版本ruby不带webrick，需要下载添加依赖进去

解决：

```
bundle add webrick 

#如果报错，可以下载一下
gem install webrick
```

#### 2.3、长时间不用项目报错，新建项目测试本地jekyll

```
cd ~/jekyll/
jekyll new myblog  # 修改myblog文件夹下_config.yml theme: minima
jekyll clean
jekyll build
jekyll server
```

打开http://127.0.0.1:4000/成功则本地jekyll没问题，删除myblog文件夹。

#### 2.4、站内链接报错

https://jekyllrb.com/docs/liquid/tags/



# ===================================================



### 一、Hexo版

#### 1.1、本地安装hexo
注：先安装node.js

```
npm install hexo-cli -g
hexo -v
where hexo
```
拉取博客代码到本地并复制一套出来做备份，我的备份位置是~/code/zhaotong2027.GitHub.io.bat，然后新建文件夹~/code/zhaotong2027.GitHub.io。
在文件夹下执行：

```
hexo init # 初始化hexo
npm install
hexo g # 生成静态文件
hexo s # 本地运行成功，按照提示打开连接可查看页面
```
#### 1.2、配置主题
到theme文件夹下，下载想用的主题，比如

```
mkdir themes/next
git clone --branch v5.1.2 https://github.com/iissnan/hexo-theme-next themes/next
```

本地绑定git，将备份中.git文件夹复制到新建文件夹，修改_config文件

```
...

# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://zhaotong.fun # 修改自己的域名
permalink: :title.html

...

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next # 修改想用的主题

...

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@github.com:zhaotong2027/zhaotong2027.github.io.git # 自己的仓库地址
  branch: master
```

#### 1.3、hexo推送代码

再次本地运行，运行成功则在dos中用hexo上传代码：

```
npm i hexo-deployer-git --save # 安装扩展
hexo clean # 清除缓存（public文件夹）
hexo g # 生成静态文件
hexo d # 部署到服务器（GitHub）
```
#### 1.4、hexo写文章
```
hexo new post "article title"
```
在zhaotong2027.github.io.git/source/_posts文件夹下找到md文件，用本地markdown工具修改即可

主题NexT说明：http://theme-next.iissnan.com/getting-started.html

### 三、hexo设置及问题解决

#### 2.1、hexo个性化设置

hexo帮助文档：https://hexo.io/docs/

首页：https://zhuanlan.zhihu.com/p/138500516

#### 2.2、配置画图插件mermaid
```
npm install hexo-filter-mermaid-diagrams
```

配置文件末尾添加mermaid配置

```
# mermaid chart
mermaid: ## mermaid url https://github.com/knsv/mermaid
  enable: true  # default true
  version: "9.0.0" # default v7.1.2 latest v9.0.0
  options:  # find more api options from https://github.com/knsv/mermaid/blob/master/src/mermaidAPI.js
    #startOnload: true  // default true
```

主题的配置文件打开 themes/next/_config.yml mermaid相关参数

```
# Mermaid tag
mermaid:
  enable: true
  # Available themes: default | dark | forest | neutral
  theme: forest
```

NexT主题已支持 mermaid，hexo运行一次测试是否成功

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```

#### 2.3、hexo本地失效办法

```
cd ~
mv hexo hexo.bat
mkdir hexo
cd ~/hexo
hexo init
npm install
hexo g
hexo s
```

看是否成功运行，成功则继续：

```
mkdir zhaotong2027.GitHub.io
git clone git@github.com:zhaotong2027/zhaotong2027.GitHub.io.git
cd ~/hexo/zhaotong2027.GitHub.io
hexo clean
hexo g
hexo s
hexo d # 提交
```

正常运行则hexo.bat可删除

#### 2.4、git页面不更新

报错：.gitmodules 中没有发现路径 '.deploy_git' 的子模组映射

复现：本地hexo d正常结束后，git不更新，查看Actions，发现deploy失败

解决：在根目录下新建.gitmodules文件，并写入如下内容，保存上传到github

```
[submodule ".deploy_git"]
	path = .deploy_git
	url = https://github.com/zhaotong2027/zhaotong2027.GitHub.io.git
```

##### git页面不更新

无报错，git用的UTC时间，国内UTC+8，只发布git最新时间的，理论上延迟8小时

