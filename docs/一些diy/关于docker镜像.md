---
title: 关于docker镜像
cover: https://pic.imgdb.cn/item/66a08708d9c307b7e9ef76f7.webp
categories:
  - 环境部署
tags:
  - 服务器
  - 环境部署
slug: guan-yu-dockerjing-xiang
halo:
  site: http://119.3.239.131:8080
  name: 00fc2532-01d0-47e4-84f3-f6afea4d53aa
  publish: true
---

## 真正能用的（重要）

参考：https://www.youtube.com/watch?v=SFMJvuvqPcg


### 1. 更换可用的镜像源

> 参考：https://mixuying.com/archives/1719753069678

目前可用的镜像源:

```
https://dockerpull.com
## 建议使用
https://docker.1panel.live
https://dockerproxy.cn
https://docker.hpcloud.cloud
```


首先 `vim /etc/docker/daemon.json`

```shell
{  
  "registry-mirrors": ["请替换为您自己的代理服务ip或者域名"]   
}
```

然后重启 docker

```shell
sudo systemctl daemon-reload  
sudo systemctl restart docker
```


## 2. 部署 docker 代理到第三方平台

参考：https://github.com/dqzboy/Docker-Proxy

1. 选择部署到 Render

![image.png](https://raw.githubusercontent.com/cloudinwind/images/main/markdown_images/image.png)

2. 登录 Render

3. New -> Web Service 

![image.png](https://raw.githubusercontent.com/cloudinwind/images/main/markdown_images/image.png)

4. 下一步->Free(免费的)->直接部署

然后会出现部署过程：
![image.png](https://raw.githubusercontent.com/cloudinwind/images/main/markdown_images/image.png)

![image.png](https://raw.githubusercontent.com/cloudinwind/images/main/markdown_images/image.png)

5. 和第一步一样, 修改镜像源为 镜像加速地址





## 目前可用的


**docker-drag**

> 版本不对应

项目地址：https://github.com/NotGlop/docker-drag

环境要求：本地有 `python` 环境

使用方式：

1. 下载到本地

```
git clone https://github.com/NotGlop/docker-drag.git

```

2. pull 镜像

```
python docker_pull.py mysql/mysql-server:8.0
```

3. 从 `tar`  文件中加载镜像

```
docker load -i library_ubuntu.tar
```

> 目前在 windows 中唯一可用的, 但是有的镜像无法下载


##  也许能用的

> 参考：https://www.passerma.com/article/78/




## 一些在线工具

参考：https://www.hi-linux.com/posts/46425.html

> dp 的延伸：https://mritd.com/2020/03/31/how-to-download-docker-image-without-docker/#%E4%BA%94%E3%80%81skopeo-copy-%E4%BD%BF%E7%94%A8

参考：https://blog.csdn.net/cced1934/article/details/137435091

参考：https://www.bilibili.com/read/cv35387254/

## 阿里云同步镜像（推荐）

> 应该能实现

同步镜像到阿里云  

参考一：[https://github.com/ikrong/sync-docker-image](https://github.com/ikrong/sync-docker-image)  
同步DockerHub上的镜像仓库到阿里云容器镜像仓库  
Docker 的一些服务所在域名被封杀，无法直接访问和拉取镜像。国内的镜像源又宣布停止服务，所以需要一个工具将DockerHub上的镜像同步到阿里云容器镜像仓库。  

参考二：https://www.lvbibir.cn/posts/tech/docker-download-foreign-images/


## docker 设置代理

参考：[https://gist.github.com/y0ngb1n/7e8f16af3242c7815e7ca2f0833d3ea6](https://gist.github.com/y0ngb1n/7e8f16af3242c7815e7ca2f0833d3ea6)

