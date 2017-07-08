# 安装MongoDB

> 最近需要学习node.js，要开始写项目。在github上找学习资料时发现大多数node小项目都是mongodb，这让我觉得有必要去了解和尝试一下这个数据库。

## MongoDB 概述
不说什么关系数据库、非关系数据库这种头疼的东西，使用mongodb的优点是：

	1. 面向文档的存储方式：数据被以JSON风格文档形式存放
	2. 任何属性均可索引
	3. 具有复制和高可用特性
	4. 自动数据分片
	5. 丰富的查询功能
	6. 快速的数据库升级
	7. 有mongodb（10gen)公司提供支持
	
这是我从某个博客看到的，而我最直观的感受是：

	1.以json风格存放数据确实有利于后台读取数据后前后交互
	2.查询功能更简单

其他优点可能需要我从以后的使用中逐渐发现。

## MongoDB安装
我先在Windows下下载并安装了mongodb，发现bin文件夹下的mongod和mongo文件打开就闪退。一般来说大家遇到的都是这个问题。
我在Windows和Ubuntu虚拟机下都安装了一遍mongodb，分别介绍一下这两个系统下的安装方法：

### Ubuntu 14.04 下安装
在网上搜了一堆资料，先下载压缩包、再解压、再加data和log文件夹，再#%￥%……
后来折腾一遍发现用不了，再搜，最终发现：
可以直接用``` sudo apt-get install mongodb ```下载。
![](/img1.jpg)

关于这个下载的资料也有不少，图方便可以用这个。下载完成之后我在终端用mongo命令直接连上了数据库，然后从官网下载了Robomongo，可视化mongodb管理软件，用``` tar -xzf robomongo-0.9.0-linux-x86_64-0786489.tar.gz   ```解压一下就可以用了。
然而好景不长，当我重启Ubuntu后，发现mongo连不上了！
然后研究了半天，发现还是需要跟Windows下一样先启动mongod，输入dbpath，才可以打开mongo。因为是自动下载的，所以dbpath是默认存在的，找了很久发现在/var/lib/mongodb中。
 ``` sudo mongod --dbpath=/var/lib/mongodb ```启动服务，mongo指令连接客户端。

### Windows 下安装
先在官网下载文件，安装，然后得到一个bin目录。
bin目录里东西一大堆，需要注意的有两个：

	mongo.exe
	mongoDB的客户端
	mongod.exe
	用于启动mongoDB的Server

**mongodb需要先启动server，也就是打开mongod，才能打开mongo客户端连接数据库**
然而，无论是终端还是直接双击，mongod文件都是闪退或者没用，这是因为我们要给mongod指定一个存放数据的data路径。
所以单有一个bin目录是不够的，还需要自己添加**data目录**和**log**目录，很简单就是在bin的同级目录下建两个文件夹，一个叫data，一个叫log。
经测试log不是必要的。
然后用指令```mongod --dbpath F:\MongoDB\data --logpath F:\MongoDB\logs\mongodb.log```启动mongodb server,重新开一个终端，或者直接打开mongo.exe，此时则能连上数据库。
同样，可以下载一个可视化管理工具管理数据库。

## 优化
每次打开数据库都要输入一长串指令，太麻烦。所以写一个批处理文件，直接通过该文件打开会方便很多。
做法为：

	在bin目录下建立一个bat文件
	bat文件里面填写启动mongod的指令 mongod --dbpath F:\MongoDB\data --logpath F:\MongoDB\logs\mongodb.log
	保存后，双击bat文件则自动开启mongod，接下来去连接mongo数据库就好了。





