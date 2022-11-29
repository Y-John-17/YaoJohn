---
title: Windows-Docker-安装Centos系统
author: Yao John
date: '2022-11-30'
slug: Windows
categories:
  - Windows
tags:
  - Docker
  - Windows
  - Centos

---

# windows docker 安装centOS系统

## （1)  拉取镜像

输入命令`docker pull centos:7 `从仓库拉取centos7的镜像

## （2)  查看本地镜像

命令：`docker images`

可以查看已经把centos的镜像拉取到本地

```html
<div class="figure">
<img src="{{< blogdown/postref >}}index_files/figure-html/1.png" alt="A fancy pie chart." width="672" />
<p class="caption">Figure 1: A fancy pie chart.</p>
</div>
```

## （3)  创建并运行容器

命令：`docker run -d -i -t 470671670cac /bin/bash`

命令格式：`docker run -d -i -t <IMAGE ID> /bin/bash`

执行完命令后，在命令下方会出现容器ID。

## （4)  进入使用容器

命令：`docker exec -it 82d5f bash`

在此处容器的ID只输入前5位，系统会自动识别容器ID

命令格式：`docker exec -it <CONTAINER ID> bash`

## （5)  安装工具

输入ifconfig命令查看网卡信息，系统找不到

输入命令安装：`yum install -y net-tools`

`docker exec -it 91fe8 bash`