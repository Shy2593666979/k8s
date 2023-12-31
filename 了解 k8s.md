**关于K8s的基本概念，我们将会围绕如下七点展开：**

1.Docker 的管理痛点

2.什么是 K8s？

3.云架构 & 云原生

4.K8s 架构原理

5.K8s 核心组件

6.K8s 的服务注册与发现

7.关键问题

#### 一、Docker 的管理痛点

如果想要将 Docker 应用于庞大的业务实现，是存在困难的**编排**、**管理**和**调度**问题。于是，我们迫切需要一套管理系统，对 Docker 及容器进行更高级更灵活的管理。

Kubernetes 应运而生！Kubernetes，名词源于希腊语，意为「舵手」或「飞行员」。Google 在 2014 年开源了 Kubernetes 项目，建立在 Google 在大规模运行生产工作负载方面拥有十几年的经验的基础上，结合了社区中最好的想法和实践。

#### 二、什么是 K8s ？

![img](https://raw.githubusercontent.com/chenfengyanyu/my-web-accumulation/master/images/k8s/intro.png)
K8s 是一个可移植的、可扩展的开源平台，用于**管理容器化的工作负载和服务，可促进声明式配置和自动化**。K8s 拥有一个庞大且快速增长的生态系统。K8s 的服务、支持和工具广泛可用。

通过 K8s 我们可以：

1.快速部署应用

2.快速扩展应用

3.无缝对接新的应用功能

4.节省资源，优化硬件资源的使用

K8s 有如下特点:

1.可移植: 支持公有云，私有云，混合云，多重云 multi-cloud

2.可扩展: 模块化，插件化，可挂载，可组合

3.自动化: 自动部署，自动重启，自动复制，自动伸缩/扩展

#### 三、云架构 & 云原生

1.云和 K8s 是什么关系

云就是使用容器构建的一套服务集群网络，云由很多的大量容器构成。K8s 就是用来管理云中的容器。

2.常见几类云架构

- On-Premises (本地部署)
- iaas（基础设施即服务）

- - 用户：租用（购买|分配权限）云主机，用户不需要考虑网络，DNS，硬件环境方面的问题。
  - 运营商：提供网络，存储，DNS，这样服务就叫做基础设施服务

- paas（平台即服务）

- - mysql/es/mq/...

- saas（软件即服务）

- - 钉钉
  - 财务管理

- serverless

- - 无服务，不需要服务器。站在用户的角度考虑问题，用户只需要使用云服务器即可，在云服务器所在的基础环境，软件环境都不需要用户关心。

![img](https://raw.githubusercontent.com/chenfengyanyu/my-web-accumulation/master/images/k8s/iaas.png)



3.云原生
为了让应用程序（项目，服务软件）都运行在云上的解决方案，这样的方案叫做**云原生**。

云原生有如下特点：

- 容器化，所有服务都必须部署在容器中
- 微服务，Web 服务架构式服务架构
- CI/CD
- DevOps

#### 四、K8s 架构原理

1.K8s 架构

概括来说 K8s 架构就是一个 Master 对应一群 Node 节点。

![img](https://raw.githubusercontent.com/chenfengyanyu/my-web-accumulation/master/images/k8s/k8s.png)

下面我们来逐一介绍 K8s 架构图中的 Master 和 Node。

2.Master 节点结构如下：

- apiserver 即 K8s 网关，所有的指令请求都必须要经过 apiserver；
- scheduler 调度器，使用调度算法，把请求资源调度到某一个 node 节点；
- controller 控制器，维护 K8s 资源对象；
- etcd 存储资源对象；

3.Node节点

- kubelet 在每一个 node 节点都存在一份，在 node 节点上的资源操作指令由 kubelet 来执行；
- kube-proxy 代理服务，处理服务间负载均衡；
- pod 是 k8s 管理的基本单元（最小单元），pod 内部是容器，k8s 不直接管理容器，而是管理pod；
- docker 运行容器的基础环境，容器引擎；
- fluentd 日志收集服务；

在介绍完 K8s 架构后，我们又引入了很多技术名词。不要着急，先有**整体概念，再各个击破**。请耐心阅读下文，相信你一定会有不一样的收获。

#### 五、K8s 核心组件

1.K8s 组件

K8s 是用来管理容器，但是不直接操作容器，最小操作单元是 Pod （间接管理容器）

- 一个 Master 有一群 Node 节点与之对应
- Master 节点不存储容器，只负责调度、网管、控制器、资源对象存储
- 容器的存储在 Node 节点，容器是存储在 Pod 内部的）
- Pod 内部可以有一个容器，或者多个容器
- Kubelet 负责本地 Pod 的维护
- Kube-proxy 负责负载均衡，在多个 Pod 之间来做负载均衡

2.Pod 是什么？

- pod 也是一个容器，这个容器中装的是 Docker 创建的容器，Pod 用来封装容器的一个容器，Pod 是一个虚拟化分组；
- Pod 相当于独立主机，可以封装一个或者多个容器；

Pod 有自己的 IP 地址、主机名，相当于一台独立沙箱环境。

3.Pod 到底用来干什么？

通常情况下，在服务部署时候，使用 Pod 来管理一组相关的服务。一个 Pod 中要么部署一个服务，要么部署一组有关系的服务。

一组相关的服务是指：在链式调用的调用连路上的服务。

4.Web 服务集群如何实现？

实现服务集群：只需要复制多方 Pod 的副本即可，这也是 K8s 管理的先进之处，K8s 如果继续扩容，只需要控制 Pod 的数量即可，缩容道理类似。

5.Pod 底层网络，数据存储是如何进行的？

- Pod 内部容器创建之前，必须先创建 Pause 容器；
- 服务容器之间访问 localhost ，相当于访问本地服务一样，性能非常高；

6.ReplicaSet 副本控制器

控制 Pod 副本「服务集群」的数量，永远与预期设定的数量保持一致即可。当有 Pod 服务宕机时候，副本控制器将会立马重新创建一个新的 Pod，永远保证副本为设置数量。

副本控制器：标签选择器-选择维护一组相关的服务（它自己的服务）

```
selector：     
  app = web    
  Release = stable 
```

- ReplicationController 副本控制器：单选
- ReplicaSet 副本控制器：单选，复合选择

在新版的 K8s 中，建议使用 ReplicaSet 作为副本控制器，ReplicationController 不再使用了。

7.Deployment 部署对象

- 服务部署结构模型
- 滚动更新

ReplicaSet 副本控制器控制 Pod 副本的数量。但是，项目的需求在不断迭代、不断的更新，项目版本将会不停的的发版。版本的变化，如何做到服务更新？

部署模型：

- ReplicaSet 不支持滚动更新，Deployment 对象支持滚动更新，通常和 ReplicaSet 一起使用；
- Deployment 管理 ReplicaSet，RS 重新建立新的 RS，创建新的 Pod；

8.MySQL 使用容器化部署，存在什么样的问题？

- 容器是生命周期的，一旦宕机，数据丢失
- Pod 部署，Pod 有生命周期，数据丢失

对于 K8s 来说，不能使用 Deployment 部署**有状态**服务。

通常情况下，Deployment 被用来部署无状态服务，那么对于有状态服务的部署，使用 StatefulSet 进行有状态服务的部署。

什么是**有状态服务**？

- 有实时的数据需要存储
- 有状态服务集群中，把某一个服务抽离出去，一段时间后再加入机器网络，如果集群网络无法使用

什么是**无状态服务**？

- 没有实时的数据需要存储
- 无状态服务集群中，把某一个服务抽离出去，一段时间后再加入机器网络，对集群服务没有任何影响

9.StatefulSet

为了解决有状态服务使用容器化部署的一个问题。

- 部署模型
- 有状态服务

StatefulSet 保证 Pod 重新建立后，Hostname 不会发生变化，Pod 就可以通过 Hostname 来关联数据。

#### 六、K8s 的服务注册与发现

**1.Pod的结构是怎样的？**

- Pod 相当于一个容器，Pod 有独立 IP 地址，也有自己的 Hostname，利用 Namespace 进行资源隔离，独立沙箱环境。
- Pod 内部封装的是容器，可以封装一个，或者多个容器（通常是一组相关的容器）

**2.Pod网络**

- Pod 有自己独立的 IP 地址
- Pod 内部容器之间访问采用 Localhost 访问

Pod 内部容器访问是 Localhost，Pod 之间的通信属于远程访问。

**3.Pod是如何对外提供服务访问的？**

Pod 是虚拟的资源对象（进程），没有对应实体（物理机，物理网卡）与之对应，无法直接对外提供服务访问。

那么该**如何解决这个问题**呢？

Pod 如果想要对外提供服务，必须绑定物理机端口。也就是说在物理机上开启端口，让这个端口和 Pod 的端口进行映射，这样就可以通过物理机进行数据包的转发。

概括来说：先通过物理机 IP + Port 进行访问，再进行数据包转发。

**4.一组相关的** **Pod** **副本，如何实现访问负载均衡？**

我们先明确一个概念，Pod 是一个进程，是有**生命周期**的。宕机、版本更新，都会创建新的 Pod。这时候 IP 地址会发生变化，Hostname 会发生变化，使用 Nginx 做负载均衡就不太合适了。

所以我们需要依赖 Service 的能力。

**5.Service如何实现负载均衡？**
简单来说，Service 资源对象包括如下三部分：

- Pod IP：Pod 的 IP 地址
- Node IP：物理机 IP 地址

- Cluster IP：虚拟 IP ，是由 K8s 抽象出的 Service 对象，这个 Service 对象就是一个 VIP 的资源对象

**6.Service VIP更深入原理探讨**

- Service 和 Pod 都是一个进程，Service 也不能对外网提供服务；
- Service 和 Pod 之间可以直接进行通信，它们的通信属于局域网通信；

- 把请求交给 Service 后，Service 使用 iptable，ipvs 做数据包的分发；

**7.Service对象是如何和Pod 进行关联的？**

- 不同的业务有不同的 Service；
- Service 和 Pod 通过标签选择器进行关联；

```
selector：     
app=x 选择一组订单的服务 pod ，创建一个 service；     
通过 endpoints 存放一组 pod ip； 
```

Service 通过标签选择器选择一组相关的副本，然后创建一个 Service。

**8.Pod 宕机、发布新的版本的时候，Service 如何发现Pod 已经发生了变化？**

每个 Pod 中都有 Kube-Proxy，监听所有 Pod。如果发现 Pod 有变化，就动态更新（etcd 中存储）对应的 IP 映射关系。

#### 七、关键问题

**1.企业使用K8s主要用来做什么？**

- **自动化运维平台**

  创业型公司，中小型企业，使用 K8s 构建一套自动化运维平台，自动维护服务数量，保持服务永远和预期的数据保持一致性，让服务可以永远提供服务。这样最直接的好处就是降本增效。
  
- **充分利用服务器资源**

  互联网企业，有很多服务器资源「物理机」，为了充分利用服务器资源，使用 K8s 构建私有云环境，项目运行在云。**这在大型互联网公司尤为重要**。

- **服务的无缝迁移**

  项目开发中，产品需求不停的迭代，更新产品。这就意味着项目不停的发布新的版本，而 K8s 可以实现项目从开发到生产无缝迁移。

**2.K8s 服务的负载均衡是如何实现的？**

Pod 中的容器很可能因为各种原因发生故障而死掉。Deployment 等 Controller 会通过动态创建和销毁 Pod 来保证应用整体的健壮性。换句话说，Pod 是脆弱的，但应用是健壮的。每个 Pod 都有自己的 IP 地址。当 controller 用新 Pod 替代发生故障的 Pod 时，新 Pod 会分配到新的 IP 地址。

这样就产生了一个问题：如果一组 Pod 对外提供服务（比如 HTTP），它们的 IP 很有可能发生变化，那么客户端如何找到并访问这个服务呢？

K8s 给出的解决方案是 Service。 Kubernetes Service 从逻辑上代表了一组 Pod，具体是哪些 Pod 则是由 Label 来挑选。

Service 有自己 IP，而且这个 IP 是不变的。客户端只需要访问 Service 的 IP，K8s 则负责建立和维护 Service 与 Pod 的映射关系。无论后端 Pod 如何变化，对客户端不会有任何影响，因为 Service 没有变。

**3.无状态服务一般使用什么方式进行部署？**

Deployment 为 Pod 和 ReplicaSet 提供了一个 声明式定义方法，通常被用来部署无状态服务。

Deployment 的主要作用：

定义 Deployment 来创建 Pod 和 ReplicaSet 滚动升级和回滚应用扩容和索容暂停和继续。Deployment不仅仅可以滚动更新，而且可以进行回滚，如果发现升级到 V2 版本后，服务不可用，可以迅速回滚到 V1 版本。