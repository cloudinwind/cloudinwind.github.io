---
title: ubuntu 22.04 安装 docker
tags:
  - 服务器
  - 环境部署
slug: ubuntu-22.04-an-zhuang-docker
cover: ""
categories:
  - 工作
halo:
  site: http://162.14.74.139:8080
  name: 573c892f-7886-40ce-9b19-7470f5af9184
  publish: false
---

## 参考


```cardlink
url: https://blog.csdn.net/guo_zhen_qian/article/details/132254406
title: "Ubuntu22.04安装docker和docker-compose-CSDN博客"
description: "文章浏览阅读9.7k次，点赞8次，收藏26次。ubuntu22.0.4系统安装docker，并修改默认镜像下载目录_ubuntu22.04安装docker"
host: blog.csdn.net
```


[容器与云|如何在 Ubuntu 22.04 LTS 中安装 Docker 和 Docker Compose](https://linux.cn/article-14871-1.html)

## 安装过程

### docker 安装

1. 更新包管理器

```
apt update
apt-get update
```


2. 安装必要的软件包，以便允许 apt 使用 HTTPS 仓库

   ```
   sudo apt install apt-transport-https ca-certificates curl software-properties-common

   ```


3. 使用下面的 `curl`
   ```
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

4. 将 Docker APT 软件源添加到你的[系统](https://so.csdn.net/so/search?q=%E7%B3%BB%E7%BB%9F&spm=1001.2101.3001.7020)

```
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

5. 再次更新包管理器

```
apt update
apt-get update
```

6. 安装docker

果安装最新版，直接输入如下命令即可:

```
apt install docker-ce docker-ce-cli containerd.io
```



### docker-compose 安装

**1. 首先使用 pip3安装**

安装命令：
```
pip install docker-compose
```

查看版本号：
```
docker-compose --version
```

一般安装后的二进制文件为 `/usr/local/bin/docker-compose`

> 一般来说使用 pip 安装的 docker-compose 版本会偏低

**2. 更新 docker-compose**

官网：https://github.com/docker/compose/releases/ 查看最新版本的 docker-compose

下载命令：
```shell
# 根据在官网查看的最新版本 对命令中的版本号进行修改, 其中 -o 表示下载的地址
sudo curl -L "https://github.com/docker/compose/releases/download/v2.6.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
> 也可以手动根据 `https://github.com/docker/compose/releases/download/v2.6.1/docker-compose-$(uname -s)-$(uname -m)` 链接进行下载, 然后移动到 `/usr/local/bin`中（先删除之前pip下载的docker-compose二进制文件）


赋予二进制文件可执行权限：
```
sudo chmod +x /usr/local/bin/docker-compose
```

查看 docker-compose 的版本号：
```
docker-compose --version
```

