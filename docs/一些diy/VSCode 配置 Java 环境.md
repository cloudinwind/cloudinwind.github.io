---
title: VSCode 配置 Java 环境
slug: "1718178585772"
cover: https://pic2.58cdn.com.cn/nowater/webim/big/n_v25dc670d9db3843eba3198a9a75280901.png
categories:
  - 工作
tags:
  - 服务器
  - 环境部署
NotionID-cloudinwind: 88e5f195-11d7-42a4-b14f-d0a6d835cb01
link-cloudinwind: https://cloudinwind.notion.site/VSCode-Java-88e5f19511d742a4b14fd0a6d835cb01
halo:
  site: http://162.14.74.139:8080
  name: ce6deda4-b2c7-43f7-b6aa-2743db84746c
  publish: false
---
# VSCode 配置 Java 环境


## 首先需要安装 JDK

比如安装目录为：

`D:\Programs\work\Java\jdk-17`

在命令行下查看相关环境配置是否正确：

`java -version` 

![](https://pic.imgdb.cn/item/66695384d9c307b7e96840db.png)


## 在 VSCode 下进行相关配置

### 必须安装的三个插件

![](https://pic.imgdb.cn/item/6669545ed9c307b7e96a0ff7.png)


### 相关配置

进入设置页面（快捷键 `Ctrl + ,`）：

![](https://pic.imgdb.cn/item/666954a5d9c307b7e96ab8ae.png)


**设置 JDK 的路径：**

输入框输入 `java` -> 定位到插件 `Language support for Java` -> 进入 `setting.json`

![](https://pic.imgdb.cn/item/66695569d9c307b7e96c563d.png)

在 `setting.json` 中设置 JDK 路径：

![](https://pic.imgdb.cn/item/666955a5d9c307b7e96d0301.png)

> 补充：不要直接进入 `setting.json ` 中添加，不同的插件版本 前面的 `java.jdt.ls.java.home` 的名称可能不一样


重启VSCode，进行java文件运行测试



> 参考： [知乎](https://zhuanlan.zhihu.com/p/684068017)


