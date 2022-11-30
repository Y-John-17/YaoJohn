---
title: Hadoop
author: Yao John
date: '2022-11-30'
slug: hadoop
categories:
  - Hadoop
tags:
  - Hadoop
  - Big Data
  - Distributed System Environment
---

# Hadoop-3.1.4 完全分布式集群搭建

>Hadoop是一个由Apache基金会所开发的分布式系统基础架构。用户可以在不了解分布式底层细节的情况下，开发分布式程序。充分利用集群的威力进行高速运算和存储。Hadoop实现了一个分布式文件系统（ Distributed File System），其中一个组件是HDFS（Hadoop Distributed File System）。HDFS有高容错性的特点，并且设计用来部署在低廉的（low-cost）硬件上；而且它提供高吞吐量（high throughput）来访问应用程序的数据，适合那些有着超大数据集（large data set）的应用程序。HDFS放宽了（relax）POSIX的要求，可以以流的形式访问（streaming access）文件系统中的数据。Hadoop的框架最核心的设计就是：HDFS和MapReduce。HDFS为海量的数据提供了存储，而MapReduce则为海量的数据提供了计算。

## 虚拟机安装和网卡启用

- 虚拟机安装过程掠过，自行百度查阅了解

- 本文所采用的虚拟机版本为 cetos7

- IP地址的设置要确保和你的虚拟网卡网络地址网段相同

| 主机名 |     IP地址      |       备注       |
| :----: | :-------------: | :--------------: |
| node1  | 192.168.153.151 | 主节点，数据节点 |
| node2  | 192.168.153.152 | 副节点，数据节点 |
| node3  | 192.168.153.153 |     数据节点     |

**启用网卡**

- `vi /etc/sysconfig/networscripts/ifcfg-ens33`

```JAVA
ONBOOT = yes
DNS1 = 8.8.8.8
```

- `ping baidu.com`     # ping通即可

**主机名和IP地址映射**

- 在原有文件下添加如下字段
- `vim /etc/hosts`

```JAVA
192.168.153.151 node1
192.168.153.152 node2
192.168.153.153 node3
```

## JAVA 环境部署

- Hadoop的创始人是Doug Cutting， 同时也是著名的基于Java的检索引擎库Apache Lucene的创始人。Hadoop本来是用于著名的开源搜索引擎Apache Nutch，而Nutch本身是基于Lucene的，而且也是Lucene的一个子项目。因此Hadoop基于Java就很理所当然了，所以，Hadoop是由Java编写的。

- Hadoop采用Java编写，因而Hadoop天生支持Java语言编写作业，但在实际应用中，有时候，因要用到非Java的第三方库或者其他原因，要采用C/C++或者其他语言编写MapReduce作业，这时候可能要用到Hadoop提供的一些工具。

- 如果你要用C/C++编写MpaReduce作业，可使用的工具有Hadoop Streaming或者Hadoop Pipes。

- 如果你要用Python编写MapReduce作业，可以使用Hadoop Streaming或者Pydoop。

- 如果你要使用其他语言，如shell，php，ruby等，可使用Hadoop Streaming。

**安装**

- 进入/opt路径创建并进入/software，将`jdk-8u281-linux-x64.rpm`文件上传到/opt/software
- `cd /opt`
- `mkdir software`
- `cd software`
- `rpm -ivh jdk-8u281-linux-x64.rpm`

**配置环境变量**

- 设置用户环境变量`vim ~/.bash_profile`

```JAVA
export JAVA_HOME=/usr/java/jdk1.8.0_261-amd64
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
export PATH
```

- 查看安装是否成功，`java -version`，出现`Java`版本即代表安装成功

## Hadoop安装和配置

### 安装部署

- 将`hadoop-3.1.4.tar.gz`文件包上传到`/opt/software`
- 执行如下操作：
- `cd /opt/software`
- `tar -zxvf hadoop-3.1.4.tar.gz`
- 添加环境变量
- `vim ~/.bash_profile`

```JAVA
JAVA_HOME=/usr/java/jdk1.8.0_261-amd64
HADOOP_HOME=/opt/software/hadoop-3.1.4
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
export PATH
```

- 查看是否安装成功`hadoop --version`，出现版本号即代表成功

### Hadoop集群配置

- 在每台主机下修改,进入如下目录，一共修改四个文件
- `cd /opt/software/hadoop-3.1.4/etc/hadoop`

**hadoop-env.sh**

- 添加本机Java环境路径，删除`export JAVA_HOME=`后面的路径配置，修改为如下内容，并保存退出
- 修改指定操作进程的用户为root，在文件里找不到，就加在文件的最后
- 查找命令：按Esc后键入查找内容
- `vim hadoop-env.sh`

```JAVA
export JAVA_HOME = /usr/java/jdk1.8.0_261-amd64
export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
```

**core-site.xml**

- 配置core-site.xml文件，在configuration标签加入下面配置，指定NameNode所在节点和配置元数据存放位置
- `vim core-site.xml`

```JAVA
<property>
	<name>fs.defaultFS</name>
	<value>hdfs://node1:9820</value>
</property>
 
<!-- 指定Hadoop运行时产生文件的存储目录 -->
<property>
	<name>hadoop.tmp.dir</name>
	<value>/opt/hadoopdata</value>
</property>
```

**hdfs-site.xml**

- 在hdfs-site.xml文件的configuration标签加入Secondary NameNode的节点配置
- `vim hdfs-site.xml`

```JAVA
<property>
	<name>dfs.namenode.secondary.http-address</name>
	<value>node2:9868</value>
</property>
```

**workers**

- 在workers文件中配置DataNode节点
- `vim works`

```JAVA
node1
node2
node3
```

### 克隆虚拟机

- 选择完整克隆，并对虚拟机相关配置做如下修改

**配置固定IP地址**

- 修改或添加如下内容
- `vi  /etc/sysconfig/network-scripts/ifcfg-ens33`

```JAVA
ONBOOT = yes
BOOTPROTO = static
IPADDR = 192.168.153.151  # 具体根据主机配置IP(152/153)
GATEWAY = 192.168.153.2
NETMASK = 255.255.255.0
DNS1 = 8.8.8.8
DNS2 = 192.168.153.2
```

`service network restart`  #替代命令，三者等效`systemctl restart netwrok` `reboot`
`ping baidu.com`           # ping通即可

**修改主机名**

- 删除localhost，修改内容如下
- `vi /etc/hostname`

```JAVA
node1   #(node2/node3)
```

**重启虚拟机**

- 重启虚拟机，让其配置生效
- `reboot`或者`init 6`

### 配置免密登录

- 在每台机器下都执行下面操作
- 生成公钥：`ssh-keygen`，执行完会在当前目录下生成一个.ssh文件夹，文件夹里面有id_rsa和id_rsa.pub文件
- `ll -all`
- 把公钥文件复制到node1,node2,node3节点上，执行下面3条命令
- `ssh-copy-id -i ./id_rsa.pub root@node1`
- `ssh-copy-id -i ./id_rsa.pub root@node2`
- `ssh-copy-id -i ./id_rsa.pub root@node3`

### 格式化Hadoop

- 格式化操作只需执行一次，在主节点下执行如下命令
- `hdfs namenode -format`
- 配置过环境变量，可执行`start-all.sh`启动集群（该命令执行时会报错，但不影响节点启动）
- 使用`jps`命令查看Hadoop守护进程

> 监听端口`192.168.153.151:9870`和`192.168.153.151:8088`

node1（主节点）

```JAVA
NameNode
DataNode
Jps
```

node2

```JAVA
DataNode
SecondaryNameNode
Jps
```

node3

```JAVA
DataNode
Jps
```

## Hadoop-3.1.4  Yarn环境搭建

- 进入`/opt/software/hadoop-3.1.4/etc/hadoop` 目录

**mapred-site.xml**

- `vim mapred-site.xml`

```JAVA
<property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
</property>
```

**yarn-site.xml**

- `vim  yarn-site.xml`

```JAVA
<property>
	<name>yarn.nodemanager.aux-services</name>
	<value>mapreduce_shuffle</value>
</property>
<property>
	<name>yarn.resourcemanager.resource-tracker.address</name>
	<value>192.168.153.151:8031</value>
</property>
<property>
	<name>yarn.resourcemanager.address</name>
	<value>192.168.153.151:8032</value>
</property>
<property>
	<name>yarn.resourcemanager.scheduler.address</name>
	<value>192.168.153.151:8034</value>
</property>
<property>
	<name>yarn.resourcemanager.webapp.address</name>
	<value>192.168.153.151:8088</value>
</property>
<property>
	<name>yarn.log-aggregation-enable</name>
	<value>true</value>
</property>
<property>
	<name>yarn.log.server.url</name>
	<value>http://node1:19888/jobhistory/logs/</value>
</property>
```

**将文件发送到子节点**
`scp -r mapred-site.xml root@node2:/opt/software/hadoop-3.1.4/etc/hadoop/`
`scp -r mapred-site.xml root@node3:/opt/software/hadoop-3.1.4/etc/hadoop/`
`scp -r yarn-site.xml root@node2:/opt/software/hadoop-3.1.4/etc/hadoop/`
`scp -r yarn-site.xml root@node3:/opt/software/hadoop-3.1.4/etc/hadoop/`

**查看是否启动成功**

- `start-all.sh
- (1)查看线程是否，一台机器ResourceManager，其他机器有NodeManager
- (2)浏览器输入启动ResourceManager主机的192.168.153.151:8088点击Nodes可以看到NodeManager节点

**node1**

```JAVA
NodeManager
Jps
JournalNode
DFSZKFailoverController
QuorumPeerMain
ResourceManager
NameNode
DataNode
```

**node2**

```JAVA
Jps
DataNode
NameNode
QuorumPeerMain
DFSZKFailoverController
JournalNode
NodeManager
```

**node3**

```JAVA
NodeManager
Jps
QuorumPeerMain
JournalNode
DataNode
```

