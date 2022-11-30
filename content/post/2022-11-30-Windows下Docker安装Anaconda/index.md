---
title: Windows下Docker安装Anaconda
author: Yao John
date: '2022-11-30'
slug: Windows下Docker安装Anaconda
categories:
  - Windows
tags:
  - Windows
  - Docker
  - Anaconda
---

# Windows下Docker安装Anaconda

> docker是一个开源的应用容器引擎，你可以简单地把它理解为虚拟机（其实和虚拟机还是有区别的）。不管你的电脑是windows，linux还是mac，只要使用相同的docker镜像运行一个容器，就可以在容器中运行你的程序，不必担心依赖和兼容性问题。
>
> 安装docker的步骤不是本文的重点，可以参考网络上其它教程。windows电脑可能需要开启hyper-v，并且有可能需要在bios中启用虚拟化技术，或者需要安装vbox等虚拟机软件。

## 配置Docker-Desktop环境

-   控制面板-->程序-->启用Windows功能-->打开`hyper-v`和`适用于Liunx的Windows子系统`功能重启电脑

-   [Docker](https://www.docker.com/products/docker-desktop/)

-   选择Windows操作系统的安装包下载并安装，可能会涉及到重启电脑

## 拉取anaconda镜像到本地

-   安装好docker并确认服务启动后

-   在`powershell`命令行下直接运行如下命令就可以**基于官方的anaconda3镜像实例化一个本地容器**：

```Git
docker run -it --name="anaconda" -p 8888:8888 continuumio/anaconda3 /bin/bash
```

-   参数`-it`是启用交互式终端，`--name="anaconda"`是给容器起名字（只要你记得住，可以换成别的名字），`-p 8888:8888`是将容器的8888端口映射到本地的8888端口，便于访问jupyter。

-   docker会自动从docker hub下载最新的anaconda3镜像并创建容器，之后你就进入容器中了。在容器中，运行如下命令**安装jupyter笔记本**

```Git
conda install -c conda-forge jupyterlab
```

## 启动jupyter lab：

- 在`docker-desktop`中选择`anaconda`容器并运行
- 点击容器图标，进入命令行，执行如下命令

```bash
cd ~
jupyter lab --ip='*' --port=8888 --no-browser --allow-root
```

- 然后你应该能看到类似

```bash
[I 13:37:11.236 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 13:37:11.239 LabApp] 
    
    To access the notebook, open this file in a browser:
        file:///root/.local/share/jupyter/runtime/nbserver-30-open.html
    Or copy and paste one of these URLs:
        http://10f788d1f6a3:8888/?token=***********
     or http://127.0.0.1:8888/?token=**********
```

- 点击最下面的其中一条链接进入浏览器，就能看到熟悉的jupyter lab界面了。
  首先按Control+C退出jupyter笔记本，然后运行命令

```bash
exit
```

