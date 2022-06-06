---
layout: post
title: 同一台Mac配置多个github账户
date: 2022-05-27 02:22:46
tags:

---

参考链接：https://juejin.cn/post/6987392622407254029



背景：已有github账户再配一个



1、配置~/.ssh/config

```
# github-zhaotong2027
Host github_zhaotong2027.com
    HostName github.com
    User zhaotong2027
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/1****2027@163.com
# github-shamingai
Host github_sha****.com
    HostName github.com
    User sha****
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/sha****@foxmail.com
# gitlab-zhaotong
Host gitlab_myserver.com
    HostName 192.168.1.9
    User zhaotong
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/myserver@163.com
```

2、拉取代码设置

zhaotong2027账户：

git clone git@github_zhaotong2027.com:zhaotong2027/algorithm-leetcode-java.git

```
git clone git@github.com:zhaotong2027/test-excel-conformity.git
修改为：
git clone git@github_zhaotong2027.com:zhaotong2027/test-excel-conformity.git
```

Sha****账户：

```
git clone git@github.com:shamingai/study-python-demo.git
修改为：
git clone git@github_sha****.com:sha****/study-python-demo.git
```

我的服务器账户：

```
git clone git@192.168.1.9:server/test.git
修改为：
git clone git@gitlab_myserver.com:server/test.git
```

3、已有项目关联

zhaotong2027账户：

git remote add origin git@**github_zhaotong2027.com**:zhaotong2027/zhaotong2027.GitHub.io.git

```
cd ~/code-zt2027/test-excel-conformity
git status
git pull  # 此时已报错
git remote rm origin
git remote add origin git@github_zhaotong2027.com:zhaotong2027/test-excel-conformity.git
git pull
git branch –set-upstream-to=origin/main main
git push --set-upstream origin main
```

Sha****账户：

```
步骤同上，只是把git@github_zhaotong2027.com替换为git@github_sha****.com
```

4、若ssh-agent罢工

```
ssh -i ~/.ssh/1****2027@163.com -vT git@github.com
ssh -i ~/.ssh/sha****@foxmail.com -vT git@github.com

ssh-agent -s
ssh-add ~/.ssh/1****2027@163.com
```



写在最后：

ssh本就不同于基于账户密码那种http类的协议，本地配置过程中不必过多关注各账户的账密信息，ssh key设置成功最重要。

