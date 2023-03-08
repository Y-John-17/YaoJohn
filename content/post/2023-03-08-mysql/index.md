---
title: MySQL服务及操作平台安装
author: Yao-John
date: '2023-03-08'
slug: mysql
categories:
  - MySQL
tags:
  - MySQl
---

## 数据库分类：

> `数据库`：按照数据结构组织、存储和管理数据的仓库，是一个长期存储在计算机内的、有组织的、可共享的、统一管理的大量数据的集合
>
> `关系数据库TRDB`：二维表方式存储，列式存储和行存储，可定义数据之间的关系，查询时采用列约束和行约束，遵循[ACID原则](原子性、一致性、独立性、持久性)。
>
> `非关系型数据库NoSQL`：数据存储之间无相关性，通过键值对方式映射存储，[遵循BASE原则](基本可用、软连接、最终一致性)，存储方式有键值对存储、文档存储、列存储和图存储。

## 一、数据库和结构化查询语言简介

1. 关系数据库

借助于集合代数等概念和方法来处理数据库中的数据，是一个被组织成一组拥有正式描述性的表格（二维表形式）

2. 结构化查询语言`SQL`

一种特殊目的的编程语言，一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统(增、删、改、查)，**通过结构化查询语言操作、管理数据库**

## 二、MySQL数据库操作平台安装



1.官网下载地址：`https://dev.mysql.com/downloads/workbench/`

2. 点击`Go to Download Page`跳转页面选择完整版安装（选较大的安装包），涵括MySQL数据库系统、操作平台及附件安装

   [![ppefgiT.jpg](https://s1.ax1x.com/2023/03/08/ppefgiT.jpg)](https://imgse.com/i/ppefgiT)

3. `No thanks,just start my download`开始下载

4. 下载完成后运行安装程序

选择"Full",这样就能把MySQL Workbench一并安装。如果没安装Python，因此会在Check Requirement中提醒，点击Execute让其自动安装。最后提示“One or more product requirements have not been satisfied”，直接点击Yes，继续。
[![ppef2JU.jpg](https://s1.ax1x.com/2023/03/08/ppef2JU.jpg)](https://imgse.com/i/ppef2JU)

5. 点击"Execute"

[![ppefyd0.jpg](https://s1.ax1x.com/2023/03/08/ppefyd0.jpg)](https://imgse.com/i/ppefyd0)

6、自动弹出Microsoft Visual C++的安装，常规安装即可

7、等它安装完后，点击下一步

[![ppefbFK.jpg](https://s1.ax1x.com/2023/03/08/ppefbFK.jpg)](https://imgse.com/i/ppefbFK)

8、点击“Yes”

[![ppefxOA.jpg](https://s1.ax1x.com/2023/03/08/ppefxOA.jpg)](https://imgse.com/i/ppefxOA)

9、点击，然后它就会自动安装

[![ppehiY8.png](https://s1.ax1x.com/2023/03/08/ppehiY8.png)](https://imgse.com/i/ppehiY8)

10、等它安装完成后，点击下一步

[![ppehZOs.png](https://s1.ax1x.com/2023/03/08/ppehZOs.png)](https://imgse.com/i/ppehZOs)

11、点击下一步

[![ppehnwq.png](https://s1.ax1x.com/2023/03/08/ppehnwq.png)](https://imgse.com/i/ppehnwq)

12、后来到这个页面，Dedicated Computer，允许其占用更多内存。Development Computer占用最小内存，适合电脑上还有许多其他程序的情形。Server Computer占用内存中等。如果3306端口用过了，要改成其他，比如3305

[![ppehQYT.jpg](https://s1.ax1x.com/2023/03/08/ppehQYT.jpg)](https://imgse.com/i/ppehQYT)

[![ppehlfU.png](https://s1.ax1x.com/2023/03/08/ppehlfU.png)](https://imgse.com/i/ppehlfU)

13、选择，点击下一步

[![ppeh3pF.png](https://s1.ax1x.com/2023/03/08/ppeh3pF.png)](https://imgse.com/i/ppeh3pF)

14、设置完密码，根据引导就可完成安装

默认安装路径`C:\Program Files\MySQL\MySQL Workbench 8.0`

> 
>
> MySQL服务默认后台开启，如果发现数据库连接不上时，需要重启MySQL服务
>
> 1. win+R输入命令
>
> `services.msc`
>
> 找到你的`MYSQL_XXX`，比如我的`MYSQL80`
>
> [![ppehs6H.png](https://s1.ax1x.com/2023/03/08/ppehs6H.png)](https://imgse.com/i/ppehs6H)