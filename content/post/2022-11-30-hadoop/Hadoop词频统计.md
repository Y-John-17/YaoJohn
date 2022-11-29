---
title: Hadoop词频统计
author: Yao John
date: '2022-11-30'
slug: Hadoop词频统计
categories:
  - JAVA
tags:
  - Big Data
  - Distributed Computer System
  - Hadoop
---

# Hadoop词频统计

## 前期准备

> 该实验采用Hadoop单分布模式即可满足，需要自己前期搭建

**Hadoop守护进程启动**

```JAVA
cd ./hadoop/sbin/start-all.sh
```

- 查看相关守护进程是否已经启动
- 使用`jps`命令

```JAVA
Jps
NodeManager
SecondaryNameNode
NameNode
DataNode
ResourceManager
```

**数据集预处理**

1. 数据集编码格式`UTF-8`,所采用数据集中词频采用统一分隔符，否则需要自行编写函数
2. 在Linux用户目录下新建一个`wordcount`文件夹，导入数据集
3. 在hdfs新建文件目录

- `hdfs dfs -mkdir /wordcount`
- `hdfs dfs -mkdir /wordcount/input`
- `hdfs dfs -mkdir /wordcount/output`

4. 将数据集存储到hdfs

- `cd /tmp/wordcount`
- `hdfs dfs -put wordcount.txt /wordcount/input`
- `hdfs dfs -ls /input`

## 使用Hadoop自带的Jar文件脚本处理数据

**数据集处理**

- `cd ./hadoop/share/hadoop/mapreduce/` 
- `ll hadoop-mapreduce-examples-3.1.3.jar`

- `hadoop  jar ./hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar  wordcount   wordcount/input/wordcount.txt  wordcount/output`
  #jar表示运行脚本类型，后面是具体脚本存储路径，wordcount是脚本中Java主函数类名，后面是数据集存储路径和输出路径
  **数据集处理结果查看**
- `hdfs dfs -cat /wordcount/output/part-r-00000`
  **数据集处理结果下载**
- `hdfs dfs -get /wordcount/output/`

## End !