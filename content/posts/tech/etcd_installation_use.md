---
title: "etcd 安装及使用"
date: 2022-06-16T23:17:21+08:00
lastmod: 2022-06-16T23:17:21+08:00
author: ["SuperEggs"]
tags:
- etcd
---

## 介绍

[etcd](https://etcd.io/)是使用Go语言开发的一个开源的、高可用的分布式key-value存储系统，可以用于配置共享和服务的注册和发现。

类似项目有zookeeper和consul。

etcd具有以下特点：

- 完全复制：集群中的每个节点都可以使用完整的存档
- 高可用性：Etcd可用于避免硬件的单点故障或网络问题
- 一致性：每次读取都会返回跨多主机的最新写入
- 简单：包括一个定义良好、面向用户的API（gRPC）
- 安全：实现了带有可选的客户端证书身份验证的自动化TLS
- 快速：每秒10000次写入的基准速度
- 可靠：使用Raft算法实现了强一致、高可用的服务存储目录

### etcd应用场景

#### 服务发现

服务发现要解决的也是分布式系统中最常见的问题之一，即在同一个分布式集群中的进程或服务，要如何才能找到对方并建立连接。本质上来说，服务发现就是想要了解集群中是否有进程在监听 udp 或 tcp 端口，并且通过名字就可以查找和连接。

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahwkak9cj20w80isgmf.jpg)

#### 配置中心

将一些配置信息放到 etcd 上进行集中管理。

这类场景的使用方式通常是这样：应用在启动的时候主动从 etcd 获取一次配置信息，同时，在 etcd 节点上注册一个 Watcher 并等待，以后每次配置有更新的时候，etcd 都会实时通知订阅者，以此达到获取最新配置信息的目的。

#### 分布式锁

因为 etcd 使用 Raft 算法保持了数据的强一致性，某次操作存储到集群中的值必然是全局一致的，所以很容易实现分布式锁。锁服务有两种使用方式，一是保持独占，二是控制时序。

- **保持独占即所有获取锁的用户最终只有一个可以得到**。etcd 为此提供了一套实现分布式锁原子操作 CAS（`CompareAndSwap`）的 API。通过设置`prevExist`值，可以保证在多个节点同时去创建某个目录时，只有一个成功。而创建成功的用户就可以认为是获得了锁。
- 控制时序，即所有想要获得锁的用户都会被安排执行，但是**获得锁的顺序也是全局唯一的，同时决定了执行顺序**。etcd 为此也提供了一套 API（自动创建有序键），对一个目录建值时指定为`POST`动作，这样 etcd 会自动在目录下生成一个当前最大的值为键，存储这个新的值（客户端编号）。同时还可以使用 API 按顺序列出所有当前目录下的键值。此时这些键的值就是客户端的时序，而这些键中存储的值可以是代表客户端的编号。

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahwhisa6j216w0iywg5.jpg)

### 为什么用 etcd 而不用ZooKeeper？

etcd 实现的这些功能，ZooKeeper都能实现。那么为什么要用 etcd 而非直接使用ZooKeeper呢？

#### 为什么不选择ZooKeeper？

1. 部署维护复杂，其使用的`Paxos`强一致性算法复杂难懂。官方只提供了`Java`和`C`两种语言的接口。
2. 使用`Java`编写引入大量的依赖。运维人员维护起来比较麻烦。
3. 最近几年发展缓慢，不如`etcd`和`consul`等后起之秀。

#### 为什么选择etcd？

1. 简单。使用 Go 语言编写部署简单；支持HTTP/JSON API,使用简单；使用 Raft 算法保证强一致性让用户易于理解。
2. etcd 默认数据一更新就进行持久化。
3. etcd 支持 SSL 客户端安全认证。

最后，etcd 作为一个年轻的项目，正在高速迭代和开发中，这既是一个优点，也是一个缺点。优点是它的未来具有无限的可能性，缺点是无法得到大项目长时间使用的检验。然而，目前 `CoreOS`、`Kubernetes`和`CloudFoundry`等知名项目均在生产环境中使用了`etcd`，所以总的来说，etcd值得你去尝试。

### etcd集群

etcd 作为一个高可用键值存储系统，天生就是为集群化而设计的。由于 Raft 算法在做决策时需要多数节点的投票，所以 etcd 一般部署集群推荐奇数个节点，推荐的数量为 3、5 或者 7 个节点构成一个集群。

#### 搭建一个3节点集群示例：

在每个etcd节点指定集群成员，为了区分不同的集群最好同时配置一个独一无二的token。

下面是提前定义好的集群信息，其中`n1`、`n2`和`n3`表示3个不同的etcd节点。

```bash
TOKEN=token-01
CLUSTER_STATE=new
CLUSTER=n1=http://10.240.0.17:2380,n2=http://10.240.0.18:2380,n3=http://10.240.0.19:2380
```

在`n1`这台机器上执行以下命令来启动etcd：

```bash
etcd --data-dir=data.etcd --name n1 \
	--initial-advertise-peer-urls http://10.240.0.17:2380 --listen-peer-urls http://10.240.0.17:2380 \
	--advertise-client-urls http://10.240.0.17:2379 --listen-client-urls http://10.240.0.17:2379 \
	--initial-cluster ${CLUSTER} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}
```

在`n2`这台机器上执行以下命令启动etcd：

```bash
etcd --data-dir=data.etcd --name n2 \
	--initial-advertise-peer-urls http://10.240.0.18:2380 --listen-peer-urls http://10.240.0.18:2380 \
	--advertise-client-urls http://10.240.0.18:2379 --listen-client-urls http://10.240.0.18:2379 \
	--initial-cluster ${CLUSTER} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}
```

在`n3`这台机器上执行以下命令启动etcd：

```bash
etcd --data-dir=data.etcd --name n3 \
	--initial-advertise-peer-urls http://10.240.0.19:2380 --listen-peer-urls http://10.240.0.19:2380 \
	--advertise-client-urls http://10.240.0.19:2379 --listen-client-urls http://10.240.0.19:2379 \
	--initial-cluster ${CLUSTER} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}
```

etcd 官网提供了一个可以公网访问的 etcd 存储地址。你可以通过如下命令得到 etcd 服务的目录，并把它作为`-discovery`参数使用。

```bash
curl https://discovery.etcd.io/new?size=3
https://discovery.etcd.io/a81b5818e67a6ea83e9d4daea5ecbc92

# grab this token
TOKEN=token-01
CLUSTER_STATE=new
DISCOVERY=https://discovery.etcd.io/a81b5818e67a6ea83e9d4daea5ecbc92


etcd --data-dir=data.etcd --name n1 \
	--initial-advertise-peer-urls http://10.240.0.17:2380 --listen-peer-urls http://10.240.0.17:2380 \
	--advertise-client-urls http://10.240.0.17:2379 --listen-client-urls http://10.240.0.17:2379 \
	--discovery ${DISCOVERY} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}


etcd --data-dir=data.etcd --name n2 \
	--initial-advertise-peer-urls http://10.240.0.18:2380 --listen-peer-urls http://10.240.0.18:2380 \
	--advertise-client-urls http://10.240.0.18:2379 --listen-client-urls http://10.240.0.18:2379 \
	--discovery ${DISCOVERY} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}


etcd --data-dir=data.etcd --name n3 \
	--initial-advertise-peer-urls http://10.240.0.19:2380 --listen-peer-urls http://10.240.0.19:2380 \
	--advertise-client-urls http://10.240.0.19:2379 --listen-client-urls http:/10.240.0.19:2379 \
	--discovery ${DISCOVERY} \
	--initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}
```

到此etcd集群就搭建起来了，可以使用`etcdctl`来连接etcd。

```bash
export ETCDCTL_API=3
HOST_1=10.240.0.17
HOST_2=10.240.0.18
HOST_3=10.240.0.19
ENDPOINTS=$HOST_1:2379,$HOST_2:2379,$HOST_3:2379

etcdctl --endpoints=$ENDPOINTS member list
```

## 知识拓展

### 什么是 Raft 算法

[Raft算法详解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/32052223)

## 下载

去官方的GitHub下载最新版本的etcd。https://github.com/etcd-io/etcd/releases

以最新版本v3.3.13为例，打开上面的链接将页面向下滑动，直到看到类似下面的内容：

​        ![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahv3fbkij20r40ef752.jpg)        



根据自己的平台下载对应的文件。

## 安装

### Windows平台

下载[etcd-v3.3.13-windows-amd64.zip](http://etcd-v3.3.13-windows-amd64.zip)文件到本地。然后解压得到如下文件：

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahwaxb51j20k008zwfc.jpg)        

双击**etcd.exe**就是启动了etcd。其他平台解压之后在bin目录下找etcd可执行文件。

默认会在2379端口监听客户端通信，在2380端口监听节点间通信。        ![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahw8lflcj20rl0ef782.jpg)        



**etcdctl.exe**可以理解为一个客户端或本机etcd的控制端。

### Mac平台

方法一：

下载[etcd-v3.3.13-darwin-amd64.zip](http://etcd-v3.3.13-darwin-amd64.zip)，解压即可。

方法二：

```bash
brew install etcd
```

### Linux平台

下载[etcd-v3.3.13-linux-arm64.tar.gz](http://etcd-v3.3.13-linux-arm64.tar.gz)，解压即可。

## 连接etcd

**注意：**

默认的`etcdctrl`使用的时v2版本的命令。

我们需要设置环境变量ETCDCTL_API=3来使用v3版本的API，而默认的也就是环境变量为 ETCDCTL_API=2是使用v2版本的API。

### **修改环境变量指定使用API的版本**

#### **Windows平台**

~~~bash
set ETCDCTL_API=3
~~~

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahw34wgmj20o60efmxy.jpg)        

#### **Mac&Linux平台**

~~~bash
export ETCDCTL_API=3
~~~

## **简单使用**

### 开启 etcd

~~~bash
./etcd
~~~

### put

~~~bash
./etcdctl --endpoints=http://127.0.0.1:2379 put nazha "dsb"
~~~

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahw06aw4j20r40ef3zn.jpg)        

### get

~~~bash
./etcdctl --endpoints=http://127.0.0.1:2379 get nazha 
~~~

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahwyqt02j20r40ef752.jpg)        



## 鸣谢

文章内容来自李文周老师的教学
