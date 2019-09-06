---
title: win下git多账号配置
date: 2019-09-06 10:57:18
tags: 随笔
categories:
toc:
---

本文主要记录了如何在一台电脑上配置多个git账号

<!--more-->

### 首次生成ssh

1.默认情况下，用户的 ssh 密钥存储在其 ~/.ssh 目录下，进入该目录并列出其中的内容，可以查看自己是否已拥有密钥：

```
cd ~/.ssh
ls
```

2.需要 id_rsa 或 id_dsa 命名的文件，其中一个带有 .pub 扩展名。.pub 文件是你的公钥，另一个则是私钥。
如果没有这样的文件，或者没有 .ssh 目录，则需要生成它们。

```
ssh-keygen -t rsa -C "youremail@qq.com"
```

3.添加密钥到ssh

```
ssh-add ~/.ssh/id_rsa
```

4.在 github 上添加 ssh 密钥，即 "id_rsa.pub" 里的公钥

5.测试

```
ssh git@github.com
```

### 多个 ssh key 管理

在 ~/.ssh 目录下新建一个 config 文件配置

1.生成 ssh

```
ssh-keygen -t rsa -C "youremail@gmail.com"
```

注意：不要一路回车，在选择存放key的时候写个名字，例如 id_rsa_gitlab

2.添加私钥

```
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_gitlab
```

3.创建并修改 config 文件

在 win 下创建一个 txt 文本，然后将名字和后缀一起改成 config 即可

添加内容

```
# one                                                                       
Host gitlab               # 域名地址的别名
HostName git.xxx.com      # 这里填写公司的 git 网址即可、真实的域名地址
User gitlab               # 配置使用用户名
IdentityFile ~/.ssh/id_rsa_gitlab   # id_rsa 的地址
# two                                                                           
Host github
HostName github.com
User github
IdentityFile ~/.ssh/id_rsa
```

4.测试

```
ssh -T git@github   // 或者 ssh -vT git@github  -v 是输出编译信息
ssh -T git@gitlab
```

5.clone 项目到本地

```
# 之前的方式：单个账号
git clone git@github.com:firstAccount/xxx.git # 缺省config配置时
git clone git@github:firstAccount/xxx.git  # config 配置后，等价于第一句

# 现在的方式：git clone git@域名的别称:用户名/项目名
git clone git@gitlab:secondAccount/xxx.git   # 使用域名地址的别名来区分

```


6.配置本地 git 用户名和邮箱（可选）

```
# 全局配置
git config --global user.name "yourName"
git config --global user.email "yourEmail@qq.com"

# 局部配置
git config user.name "yourName"
git config user.email "yourEmail@qq.com"
``` 

没有局部配置，默认用全局配置，否则优先使用局部配置