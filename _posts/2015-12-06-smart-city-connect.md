---
layout: post
title: smart city connection 智慧城市的链接系统
description: "智慧城市的链接系统"
modified: 2015-12-06
category: articles
tags: [smartcity]
comments: true
share: true
---

###万物互联下的新局势

<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449389719490.png" width="1385"/>

1. Internet：  4 billion devices 互联，在2020年互联网将会有20billion 设备。预示着Internet self has to change
2. Sensors ： Every device, Every building, every vehicle
3. Things :  为了让所有的东西互联，我们需要将他们连接上互联网，然后通过软件来控制他们。将会是后面十年的主流方向。
4. Services: 将来面向用户的都终将成为服务，这些服务包括：Ecommerce、Egoverment和其他相关领域。

这些将组成为一个整体的smart city。

而作为其中最重要的又将是作为所有基础设施的重中之重的Internet服务。

对于Internet的观点来说，只有三种类型的building，我们称之为 ： rezidentsia building, commercial building, Data center.

TCP/IP 作为联通城市的最基础协议。

<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449390186468.png" width="1679"/>

###无限带宽技术(InfiniBand) 

在数据中心内部，需要的带宽技术实际上更大，所以我们设计了新技术来取代TCP/IP，我们称其为InfiniBand。
TCP/IP只能用到带宽量的10-20%，但是InfiniBand技术可以使用到带宽量的95%。 则表示了InfiniBand技术是TCP/IP传输效率的10倍。但是Infiniband只能用于在短距离传输，一般用于数据中心内部。而TCP/IP可用于远距离传输。

在数据中心内部我们使用InfiniBand来控制设计数据中心内部的传输，以达到更高的效率，我们称为DataCenter Fabric
<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449390635285.png" width="1680"/>

而对于整体的设计就会升级成在数据中心内部使用InfiniBand，而在数据中心之间使用TCP/IP。
<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449390744922.png" width="1680"/>

###扩展的无线带宽技术 Extending Infinband

使用交换设备将Infinband扩展的20000 kilometers
<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449391048583.png" width="1680"/>
这个技术可以解决两点间的互通问题，我们可以用这个技术来替换数据中心间的两点间的互通，将数据中心之间使用InfiniBand技术来使得我们的网络利用率更高。
<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449391200780.png" width="1680"/>
从而让我们都形成InfiniBand之间的互通。

他们提供三种相应的芯片来解决不同地域之间的互通问题，可以看看
<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449391348584.png" width="1677"/>
数据中心和数据中心之间的使用Fabric to the cloud， 可以提供20000km距离，100Gb/s的量， 城市间的可以使用100km 10Gb/s的量级，而家庭之间的可以使用1Km 1Gb/s的。
最终还是解决互联之间的带宽问题。

加上这三个级别的交换设备，我们就可以解决城市内部的联通，来替代传统的TCP/IP网络
<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449391926403.png" width="1677"/>

这个说明了如何来替代我们的城域网络，然后进一步的，我们使用小型的交换机来解决城市内部的小型网络，将我们整个城市的所有building互通。
<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449392013206.png" width="1677"/>

从而进入了每个Building的每个room， 这样我们在building的机房内设置相应的大楼级交换机来进入每个room.

然后将房间内的每个设备都连上传感器来加入InfiniBand网络。同时也可以提供Extend Wifi等系统。

将这整个一套成为New generation Network。提供了新一代的连接方式。片子结尾将这定义为2050年的网络。

###各大介绍

百度是这么介绍的[http://baike.baidu.com/view/1549309.htm](http://baike.baidu.com/view/1549309.htm)
IBM的相关介绍[http://www.ibm.com/developerworks/cn/linux/l-cn-infiniband/index.html](http://www.ibm.com/developerworks/cn/linux/l-cn-infiniband/index.html)
思科也在其中 [http://www.cisco.com/web/CN/products/products_netsol/datacenter/sol/0716_1.html](http://www.cisco.com/web/CN/products/products_netsol/datacenter/sol/0716_1.html)
[数据中心光纤融入InfiniBand](http://tech.sina.com.cn/roll/2008-12-17/1000918871.shtml)
[专访Mellanox CEO Eyal Waldman：InfiniBand与以太网齐驱并进 性能更胜一筹](http://www.csdn.net/article/2015-04-01/2824366-interview-Mellanox-CEO-Eyal-Waldman)


####北京石竹科技 []

引用一下相关解决方案：
<img src="http://7xocbv.com1.z0.glb.clouddn.com/test/1449394579708.png" width="834"/>
上述数据中心中，分三层网络结构，主要包括一层核心层，二层汇聚层，三层为业务接入层。划分网络与系统功能大致分为：
* 业务运行系统：负责企业支撑、生产等业务系统运行。一般业务系统可通过以太网组网，进行不同应用系统间数据的共享与处理；当需要大数据量计算与处理时，或者需要高速互联传输时，可通过InfiniBand组网，实现高性能的互联计算，解决以太网遇到的I/O瓶颈问题。
* 安全与管理系统：以太网组网，包括监控管理中心和安全中心，监控管理负责设备运行与维护，包括性能管理、设备策略配置、故障管理、告警管理等网络与设备的管理与监控。安全中心负责企业证书认证、安全设备配置管理、入侵防护与告警监控等功能。
* 大容量存储系统：通过光纤交换机接入存储系统。
* 异地容灾备份中心：支持数据容灾（即数据复制）和应用的远程切换（即发生灾难时，应用可以很快在异地切换）。
* 办公网络系统：通过以太网交换机组网，企业ERP或内部管理系统，用于日常办公事务、项目、商务、外协等管理。
 
 
1.2 业务系统

业务系统是数据中心的处理核心，由数百台内置InfiniBand网卡刀片服务器组成，业务处理节点通过InfiniBand网络通讯。本方案中服务器按业务区别管理，如WEB应用业务系统用于负责应用界面的提供，接受客户端请求并返回最终结果，是业务系统和数据的对外界面；逻辑计算业务系统负责数据的计算、业务流程整合；数据管理业务系统，负责数据的存储，供业务系统进行读写和随机调用。

业务运行系统运行着企业内部的支撑系统或重要的生产系统，是企业核心业务，作为重要的系统资源，不仅从业务保证方面，要提高系统运行的高效率，更要求承载网络和运行的平台具备及高的安全性。

一般来讲，不同的业务系统，其应用会安装在不同的运行平台上，根据需求配置服务器和软件环境，从上述拓扑中，以太网络连接不同的应用，其数据也是在不同的路径中进行传输，其根据请求提交的运行结果，也会通过各种访问方式，进行响应。

1.3 安全与管理系统

安全管理系统包括安全中心和监控管理中心。

安全中心负责对全网内的安全设备进行统一管理，其功能包括对认证证书的统一管理，对安全设备如防火墙的策略设置管理，对入侵防护设备的预警配置管理，对安全审计系统的定期检测与策略更新，对全网防病毒系统的更新管理，对网关防毒、防攻击的安全设置与管理，对内网流量的安全监控等。
监控管理中心负责对网络实行全面网络管理与实时状态的监控，负责设备运行与维护，包括性能管理、设备策略配置、故障管理、告警管理等网络与设备的管理与监控，异常访问及异常流量的监控。同时还对运行的软件进行远程管理与设置。

上述的安全与管理系统是数据中心日常运维必须用到的，通过监控与管理，使网络系统透明化，运行状态实时可见，监控机房是数据中心重要的设施。

1.4 网络系统

本方案有多种网络分类方式：按照网络功能划分可以将网络分成安全与管理网络、办公网络、业务网络、容灾备份网络、外接网络；按照网络带宽不同可以分为以太网、InfiniBand网络、光纤网络；按照网络交换层次不同可分为核心层、汇聚层和接入层三层结构。
    
网络功能分区设计，易于各个独立管理；网络带宽兼容设计，可以充分节约成本。网络架构分层设计，充分隔离了各个功能区，易于网络故障分离排错，易于系统扩展伸缩。三种分类方式根据性能需求、成本条件、安全可靠等问题的统筹考虑，合理融合，相互渗透，形成了灵活易扩展、易管理的解决方案。

其中，各个交换环节都采用双机热备形式，更有光纤专线连入异地灾备区，这种全冗余设计充分保障安全性、可靠性。
另外，在本方案的应用环境需要同时搭建InfiniBand网络、Ethernet网络和FC存储网络，三个独立网络的搭建带来了巨大的工作量和非常高的成本，同时也大大增加了后期维护的难度及成本。而本方案采用最新技术，使用网关产品或下一代SwitchX 系列产品，实现了以太网、InfiniBand、FC 三种网络的融合。这大大减少了服务器上I/O卡的数量，降低了网络的复杂程度，降低了客户的成本，降低了客户的后期维护的难度，减少了网络维护的成本。

1.5 存储系统

本方案存储设备采用高密度，高可靠性，易扩展的SFA10000高性能，可扩展性存储平台，支持FC和IB交换网络，主机支持10 GB/s 写、12+GB/s读吞吐量；单系统最大支持88U/1,200 驱动器/3.6PB容量；单个机架最大支持48U/1.8PB容量；

 另外，方案采用FC和IB网络传输数据，将极大的扩充数据通道的速度。光纤传输的优点是速度快、抗干扰能力强, 为高性能的数据存储提供了保障。InfiniBand可以在相对短的距离内提供高带宽、低延迟的传输，而且在单个或多个互联网络中支持冗余的I/O 通道，因此能保持数据中心在局部故障时仍能运转。它是一个统一的互联结构，既可以处理存储I/O、网络I/O，也能够处理进程间通信(IPC) ，实现高的可靠性、可用性、可扩展性和高的性能。










