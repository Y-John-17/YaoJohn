---
title: Linux下安装Git和Hugo
author: Yao John
date: '2022-11-30'
slug: Linux下安装Git和Hugo
categories:
  - Linux
tags:
  - Linux
  - Git
  - Hugo
---

# Linux下安装 Git 和 Hugo

## 1.安装git

```Linux
yum install git
git # 查看是否安装成功
git config --global user.name "你的昵称" 
git config --global user.email "你的邮箱"
```

## 2.安装[Hugo](https://github.com/gohugoio/hugo/releases)

```linux
# 第一种方式是上传安装包  `hugo_0.65.1_Linux-64bit.deb`  在可视化桌面下点击打开即可安装
 
# 第二种方法采用压缩包安装（操作更加简便）
hugo_0.65.1_Linux-64bit.tar.gz
sudo tar -zxvf hugo_0.65.1_Linux-64bit.tar.gz -C /usr/local/bin/  # 无需配置环境变量

```

>[Hugo Themes](https://themes.gohugo.io/)


- 你能在这个网站挑选一个你喜欢的主题，并下载到本地，方便后面博客的美化

