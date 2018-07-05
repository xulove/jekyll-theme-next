---
title: ubuntu下安装nodejs并升级到最新版
description: ubuntu下nodejs的安装，利用npm安装和n来安装nodejs的最新版
categories:
 - ubuntu
 - nodejs
tags:
 - ubuntu
 - nodejs
---

keywords: "ubuntu下nodejs的安装，nodejs升级到最新版"

# ubuntu版本信息如下

```
Linux muir 4.13.0-16-generic #19-Ubuntu SMP Wed Oct 11 18:35:14 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
```

[nodejs](https://www.centos.bz/tag/nodejs/)最新版本为：8.8.1

# 一、安装之前首先update

```
sudo apt-get update
sudo apt-get upgrade
```

## 二、安装nodejs

```
sudo apt-get install nodejs-dev
sudo apt-get install npm
```

## 三、查看版本

```
node -v
npm -v
```

可以看到版本是6.xxx.xx

## 四、升级node版本

```
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
```

![img](https://www.centos.bz/wp-content/uploads/2017/11/1-9.png)

再看版本号:

![img](https://www.centos.bz/wp-content/uploads/2017/11/2-3.png)