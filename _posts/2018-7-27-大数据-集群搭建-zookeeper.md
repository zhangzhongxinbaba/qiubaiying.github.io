---
layout:     post
title:      大数据集群安装
subtitle:   zookeeper
date:       2018-7-27
author:     zhangzhongxin
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - 大数据
---

## 前言
zookeeper 在分布式应用中使用的相当广泛，在高可用集群中，可以实现主节点之间的数据同步

## zookeeper 简介
zookeeper 核心功能：文件系统

1.每个目录都是一个znode 节点

2.znode 节点可直接存储数据

3.类型：持久化，持久化顺序，临时，临时顺序

-- 1）持久化节点，永远不会删除的节点

-- 2）持久化顺序，会带一个id，新建顺序节点按照id 自增

-- 3）临时节点，与客户端断开连接就自动删除

--4 ）临时顺序，带顺序，断开连接自动删除

常用：临时，临时顺序

临时：故障转移，Ha

临时顺序：分布式锁

zookeeper 核心功能：通知机制

1.客户端监听关心的 znode 节点

2.znode 节点有变化（数据改变/删除/子目录添加删除）时，通知客户端


## 正文

安装zookeeper 集群
```objc
1、下载 zookeeper-3.4.5-cdh5.7.0.tar.gz 包
2、对zookeeper-3.4.5-cdh5.7.0.tar.gz 进行解压缩：tar -zxvf zookeeper-3.4.5-cdh5.7.0.tar.gz
3、对zookeeper目录进行重命名：mv zookeeper-3.4.5 zk。
4、配置zookeeper相关的环境变量
vi ~/.bash_profile
export ZOOKEEPER_HOME=/data/workdir/zk
export PATH=$ZOOKEEPER_HOME/bin
source ~/.bash_profile

配置zoo.cfg:
cd zk/conf
mv zoo_sample.cfg zoo.cfg
vi zoo.cfg
修改：dataDir=/usr/local/zk/data
新增：
server.0=hadoop01:2888:3888
server.1=hadoop02:2888:3888
server.2=hadoop03:2888:3888

配置zk 节点标识：

cd zk
mkdir data
cd data
vi myid
0

搭建zk 集群：
1、在另外两个节点上按照上述步骤配置ZooKeeper，使用scp将zk和 ~/.bash_profile 拷贝到 hadoop02 和 hadoop03 上即可。
2、唯一的区别是 hadoop02 和 hadoop03 的标识号分别设置为1和2。

启动zookeeper 集群：
1、分别在三台机器上执行：zkServer.sh start。
2、检查ZooKeeper状态：zkServer.sh status。

```

zookeeper 集群安装很简单 是不是?对不对  no no no

zookeeper 的强大不是在集群好不好搭建，而是基于zookeeper 临时节点的唯一性 和 节点监听特性来提供各种服务

后续我会推出基于 zookeeper 的分布式命名，分布式锁，服务监听 等学习笔记，欢迎来关注

