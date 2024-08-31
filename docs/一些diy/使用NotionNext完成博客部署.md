---
title: 使用NotionNext完成博客部署
cover: https://raw.githubusercontent.com/cloudinwind/images/main/markdown_images/202407241312107.png
summary: NotionNext 相关配置
categories:
  - 工具
  - 环境部署
tags:
  - 工具
halo:
  site: http://119.3.239.131:8080
  name: 14fdde81-00d3-4ff9-a551-0d429eab765e
  publish: true
slug: test
---


## NotionNext 完成博客部署

> 说明：使用云服务器实现
> 
> 参考：https://docs.tangly1024.com/article/deploy-notion-next-on-vps#eba68182d6ee443489149cbc0a5e1dae
> NotionNext 开源地址：https://github.com/tangly1024/NotionNext

### 首先获取 Notion 中的PageId

**1. 复制 notion 页面的模板**

模板地址：https://tanghh.notion.site/02ab3b8678004aa69e9e415905ef32a5

点击 **Duplicate** 完成模版复制：

![AA](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F35421745-2316-47bf-a6ce-7cd1aeb3deb3%2FUntitled.png?table=block&id=15646cce-2570-46d0-8c70-c56832db302b&t=15646cce-2570-46d0-8c70-c56832db302b&width=624&cache=v2)


**2. 分享页面**

**Share -> Published -> Share To Web**

![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F1fc010fe-32a5-4934-947b-fb1e076735bd%2FUntitled.png?table=block&id=586d2451-f192-449f-93d0-b7ce35e1e13d&t=586d2451-f192-449f-93d0-b7ce35e1e13d&width=528&cache=v2)

**3. 获取页面ID**

页面ID在共享链接中，是域名后面的 **32位字母和数字**

![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6fd734be-198d-4764-8eb4-2a1755c5020b%2FUntitled.png?table=block&id=7ae734b0-fe29-43ba-a715-4aa3adea7c57&t=7ae734b0-fe29-43ba-a715-4aa3adea7c57&width=576&cache=v2)

> 请注意：
> 
> 比如共享链接为：https://cloudinwind.notion.site/<mark style="background: #FF5582A6;">0286ace3452b4429a7493804f269116e</mark>?v=42bb9716da254d5fbfe38ef64d307b0e
> 
> 那么页面ID为：<mark style="background: #FF5582A6;">0286ace3452b4429a7493804f269116e</mark>
> 
> 即**标红**加粗部分才是**页面ID**！要忽略`?v=XX` 后面的英文和数字。


### 其次在服务器中完成NotionNext环境配置

**1. 从 github 下载源码**

```bash
# 将Git上的代码下载到服务器中 ; 
git clone https://github.com/tangly1024/NotionNext && cd NotionNext
```

**2. 安装 NodeJS 环境**

推荐使用`nvm`进行安装，NVM（Node version manager）是nodejs的专用版本管理器，可以快速方便地**安装**并**切换**nodejs的版本，方便以后升级NodeJS环境。

1. 使用git下载nvm源代码

```bash
# 用git从github下载nvm源码。
git clone https://github.com/cnpm/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags
```

使用 gitee

```
git clone https://gitee.com/koalakit/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags`
```

2. 使用nvm安装Nodejs

```
# 1. 将nvm设置为系统环境变量
echo "source ~/.nvm/nvm.sh" >> ~/.bashrc

# 2. 更新变量环境
source ~/.bashrc

# 3. 安装 nodejs ， 这里举例使用v20.10.0版本，其它更新的版本也可以用
nvm install v20.10.0
```

>   题外话： 
>   
>   用 `nvm list-remote` 命令可查看所有可安装的nodejs版本 
>   
>   用 `nvm ls`可查看所有已安装到本地的nodejs 版本
>   
>   用 `nvm alias default 20.10.0` 可以更改默认的 node 版本



3. 安装yarn环境

NodeJS 自带了 NPM（Node Package Manager），他可以用来安装打包编译NotionNext这类基于Webpack打包的项目。不过npm不太好用，这里安装一个npm升级版yarn

```
# 1.【可选步骤】 NPM切换国内淘宝网镜像，便于加速安装。
npm config set registry http://registry.npm.taobao.org

# 2. npm 全局安装 yarn 
npm install -g yarn
```

**3. 运行 NotionNext **

1. 安装NotionNext依赖的库
> 首先进入 NotionNext 所在的目录

```
# 执行以下命令，使用yarn安装项目所有依赖的脚本和库
yarn
```

> 如果没有安装yarn，也可以用 `npm install` 命令进行安装依赖


2. 项目编译

每次`修改代码`，包括修改 `blog.config.js` 文件的内容后，都需要重新执行这步骤

```
# 2.打包编译
yarn build

# 3.将你的notion_page_id设为环境变量(在第一大部分中获取的)，例如：
#   也可以直接在 blog.config.js 中修改
NOAGE_ID=29d5ia78b858e4a3bbc13e51b5400fb82
```

![](https://pic.imgdb.cn/item/669e0127d9c307b7e9a3f56f.png)

>在收集页面信息的时候可能出错，可以重新运行

3. 项目运行

```
yarn start -p 8081
```

> 说明：
> 1. 如果不指定端口号，默认端口号为 3000, 可以通过 http://ip:3000 访问
> 2. 如果指定了端口号，需要在安全组中将 端口号 加入允许访问


### 补充

**1. 后台运行**

```
nohup yarn start >/dev/null 2>&1 &

## 输出到日志文件中
nohup yarn start > notionnext.log 2>&1 &

```

**2. 关闭进程**
```
pkill -f "yarn start"
```

**3. 网站崩溃问题**

> 服务器运行NotionNext站点后，总会出现过一段时间 站点进程莫名其妙被杀死，导致网站暂停
> 
> 如果站点出现这种情况，可以尝试这种做法，创建一个启动脚本启动网站，并且实时检测网站关闭时自动启动站点

 在服务器`NotionNext` 文件夹下，创建一个脚本`start_yarn.sh`, 脚本的内容如下:
```shell
#!/bin/bash

while true; do
    # 检查 Yarn 进程是否存在
    if pgrep -x "yarn" > /dev/null; then
        echo "Yarn process is already running."
    else
        # 执行命令
        nohup yarn start -p 8081 >/dev/null 2>&1 &

        # 获取最新启动的进程ID
        pid=$!

        echo "Yarn process started with PID: $pid"
    fi

    # 等待一段时间后重新执行
    sleep 5
done
```

运行脚本

```bash
## 为脚本添加可执行权限
chmod +x start_yarn.sh

## 运行启动脚本
nohup ./start_yarn.sh > /dev/null 2>&1 &

## 关闭运行的脚本
pkill -f "yarn"
```

### NotionNext如何自动更新

```bash
#!/bin/bash

# 切换到工作目录
cd NotionNext

# 从仓库更新拉取最新代码
git pull https://github.com/tangly1024/NotionNext

# 编译新版代码
yarn build

# 启动
yarn start -p 8081
```


