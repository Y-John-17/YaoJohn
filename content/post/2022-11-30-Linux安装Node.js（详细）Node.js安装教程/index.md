---
title: Linux安装Node.js（详细）Node.js安装教程
author: Yao John
date: '2022-11-30'
slug: Linux安装Node.js（详细）Node.js安装教程
categories:
  - Linux
tags:
  - Linux
  - Node.js
---

# linux安装Node.js（详细）Node.js安装教程

## 文章目录

- linux安装Node.js（详细）Node.js安装教程

- - [1：下载](1：下载)

- - [2：解压](2：解压)

- - [3：移动目录](3：移动目录)

- - - [1：创建目录](1：创建目录)

  - - [2：移动目录并重命名](2：移动目录并重命名)

- - 4：[设置环境变量](4：设置环境变量)

- - 5：[刷新修改](5：刷新修改)

- - 6：[安装完成，查看版本号](6：安装完成，查看版本号)

### 1：下载

`wget https://nodejs.org/dist/v14.17.4/node-v14.17.4-linux-x64.tar.xz`

更多版本选择：

---->[更多nodejs版本下载]([Download | Node.js (nodejs.org)](https://nodejs.org/en/download/))

`版本太高可能会导致其他不兼容，在选择版本时根据个人配置选择`

### 2：解压

`tar xf node-v14.17.4-linux-x64.tar.xz`

可以查看当前目录下的文件，执行：`ls` （命令）

**解压成功后可以选择删除压缩包**：`rm -rf node-v14.17.4-linux-x64.tar.xz`

其中：`-f` 会提醒是否删除 ；`-rf` 会强制删除，不会提醒。（使用rf，因为有些人不知道如何操作等待回车的对话线）

### 3：移动目录

#### 1：创建目录

`mkdir /usr/local/lib/node`

如果目录已经存在，则无需创建，也可以根据自己的喜好设置目录名称

#### 2：移动目录并重命名

`mv node-v14.17.4-linux-x64 /usr/local/lib/node/nodejs`

这里执行了两个步骤，首先将文件移动到node文件夹，然后将文件重命名为nodejs

### 4：设置环境变量

**注意：这一步需要管理员权限或者对该文件的写入权限。**

执行：

`sudo vim ~/.bash_profile`

输入 `i` 即可对文件进行编辑。

在文件底部添加环境变量：

```java
NODEJS_HOME=/usr/local/lib/node/nodejs
export PATH=$NODEJS_HOME/bin
```

### 5：刷新修改

`source /etc/profile`

### 6：安装完成，查看版本号

node版本号：

`node -v`

npm版本号：

`npm -v`