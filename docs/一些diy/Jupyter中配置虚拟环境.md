---
title: Jupyter中配置虚拟环境
slug: jupyterzhong-pei-zhi-xu-ni-huan-jing
cover: 
categories:
  - 环境部署
tags:
  - 服务器
  - 环境部署
halo:
  site: http://119.3.239.131:8080
  name: ae989a0e-57f0-4de9-8de2-d5bb65e2c377
  publish: false
---
  

**参考：**

```cardlink
url: https://blog.csdn.net/u014264373/article/details/119390267
title: "【最全指南】如何在 Jupyter Notebook 中切换/使用 conda 虚拟环境？_多个conda环境 notebook用的哪个-CSDN博客"
description: "文章浏览阅读7.3w次，点赞293次，收藏656次。服务器上配置有多个 conda 虚拟环境，在使用jupyter notebook时需要使用其中的一个环境，但是其默认还是使用 base 环境。我们该怎么切换环境呢。Jupyter 在一个名为 kernel 的单独进程中运行用户的代码。kernel 可以是不同的 Python 安装在不同的 conda 环境或虚拟环境，甚至可以是不同语言（例如 Julia 或 R）的解释器。简而言之，如何使用 conda 环境和 Jupyter 有三种选择：文章目录1.在 conda 环境中运行 Jupyter 服务_多个conda环境 notebook用的哪个"
host: blog.csdn.net
```


  

服务器上配置有多个 `conda` 虚拟环境，在使用jupyter notebook时需要使用其中的一个环境，但是其默认还是使用 base 环境。

如何在jupyter中配置虚拟环境，有三种方式：

### 1. 在 conda 环境中运行 Jupyter 服务器和内核

  

```bash

conda create -n my-conda-env   # creates new virtual env

conda activate my-conda-env    # activate environment in terminal

conda install jupyter     # install jupyter + notebook

jupyter notebook       # start server + kernel

```

  

推荐指数： ⭐️⭐️

  

这种方法就是为每一个 conda 环境 都安装 jupyter。

  

Jupyter 将完全安装在 conda 环境中。不同版本的 Jupyter 可用于不同的 conda 环境，但此选项可能有点**矫枉过正**。

  

在环境中包含内核就足够了，内核是运行代码的封装 Python 的组件。Jupyter notebook 的其余部分可以被视为编辑器或查看器，并且没有必要为每个环境单独安装它并将其包含在每个 env.yml 文件中。

  

因此，接下来的两个选项之一可能更可取，但这是**最简单的一个**，绝对没问题。

  

### 2. 为 conda 环境创建特殊内核

  

```

conda create -n my-conda-env    # creates new virtual env

conda activate my-conda-env     # activate environment in terminal

conda install ipykernel      # install Python kernel in new conda env

ipython kernel install --user --name=my-conda-env-kernel  # configure Jupyter to use Python kernel

jupyter notebook      # run jupyter from system

```

  

只有 Python 内核会在 conda 环境中运行，系统中的 Jupyter 或不同的 conda 环境将被使用——它没有安装在 conda 环境中。

  

通过调用`ipython kernel install`将 jupyter 配置为使用 conda 环境作为内核.

  

推荐指数： ⭐️⭐️⭐️⭐️

  

### 3. 使用 nb\_conda\_kernels 添加所有环境

  

第二种方法其实也挺不错的。有个缺点是，你新建一个环境，就要重复操作一次。

  

而这个方法就是一键添加所有 conda 环境，且不妙哉！

  

```

conda activate my-conda-env    # this is the environment for your project and code

conda install ipykernel

conda deactivate

  

conda activate base      # could be also some other environment

conda install nb_conda_kernels

jupyter notebook

```

  

推荐指数： ⭐️⭐️⭐️⭐️⭐️

  

注意：这里的 `conda install nb_conda_kernels` 是在 base 环境下操作的。

  

安装好后，打开 jupyter notebook 就会显示所有的 conda 环境啦，点击随意切换。  

![](https://i-blog.csdnimg.cn/blog_migrate/0cb8e1bd220e631c2aba343d176fdc8e.png)

  

可能存在的问题可以参考链接：[conda 环境问题](https://stackoverflow.com/questions/58068818/how-to-use-jupyter-notebooks-in-a-conda-environment/58068850#58068850 "conda 环境问题")