---
title: 服务器安装redis
slug: fu-wu-qi-an-zhuang-redis
cover: https://pic.imgdb.cn/item/66cd7246d9c307b7e9c3f356.png
categories:
  - 环境部署
tags:
  - 服务器
  - 环境部署
halo:
  site: http://119.3.239.131:8080
  name: d87a882f-9995-4976-9a14-5386d5bdda2e
  publish: true
---

## 安装 redis

1. 更新软件包索引

首先，确保包管理器是最新的：`sudo apt update`

2. 安装 Redis

使用命令安装 Redis：`sudo apt install redis-server`

3. 验证安装

安装完成后，你可以通过检查 Redis 服务的状态来验证是否正确安装：`sudo systemctl status redis`

如果安装成功，应该会看到 Redis 正在运行的状态信息。


## 配置外部访问

1. 修改配置文件 `/etc/redis/redis.conf`

```shell
# 1. 修改  bind 127.0.0.1 ::1 为 bind 0.0.0.0
bind 0.0.0.0

# 2. 关闭 protected-mode 从而允许外部连接
protected-mode no

# 3. 设置访问密码(可选)
requirepass yourpassword
```

> 也可以修改端口号 port


2. 重启 redis 服务

```shell
sudo systemctl restart redis
```


3. 在服务器的安全组中开放 6379 端口