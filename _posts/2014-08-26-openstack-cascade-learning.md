---
layout: post
title: Openstack Cascade级联特性
description: "Openstack Cascade级联特性分析"
modified: 2014-08-26
category: articles
tags: [Openstack]
comments: true
share: true
---

## Openstack Cascade 级联特性

### 简介
Openstack Cascade解决方案设计用于解决大规模的分布式云场景，比如由多个数据中心组成的上百万级别的虚拟机场景。

Openstack Cascade主要通过Openstack API聚合组成，将由多个子Openstack整合成一个单一的Openstack，提供单一的API供租户访问（包括NOVA/Cinder/Neutron/Glance/Ceilometer API）。 对于租户来说，看到的是一个Openstack，多个子Openstack被一个父Openstack整合，提供给租户，租户可以看到的是仅仅是单一的父Openstack。每个租户同样可以看到多个available zone。 每个Openstack实际的工作组成类似amazon的availiablity zone。

<figure>
     <a href="{{ site.url }}/images/blog2014/openstack_cascade_01.png"><img src="{{ site.url }}/images/blog2014/openstack_cascade_01.png"></a>
     <figcaption>Openstack Cascade 级联架构图</figcaption>
</figure>

### 对于大级别云，两种方案的对比

####使用单一Openstack组建

* 使用单一Openstack管理100万以上的虚拟机或者10万以上的Host主机风险很大
* 不能想EC2的available zone一样隔离故障区域，由于数据库和RPC message的绑定所有的云都仅仅的绑定到一个Openstack中了。
* 单一的巨大的系统为维护团队的升级和配置管理带来了极大的挑战

#### 使用多Openstack组建Cascade

###Openstack Cascade 架构

架构演进可以参考这篇文章[OpenStack cascading and fractal](https://www.linkedin.com/today/post/article/20140729022031-23841540-openstack-cascading-and-fractal?trk=prof-post)

<figure>
     <a href="{{ site.url }}/images/blog2014/openstack_cascade_02.png"><img src="{{ site.url }}/images/blog2014/openstack_cascade_02.png"></a>
     <figcaption>Openstack Cascade 级联架构图</figcaption>
</figure>

* Nova-Proxy: Similar role like Nova-Compute. Transfer the VM operation to cascaded Nova. Also responsible for attach volume and network to the VM in the cascaded OpenStack.
* Cinder-Proxy: Similar role like Cinder-Volume. Transfer the volume operation to cascaded Cinder.
* L2-Proxy: Similar role like OVS-Agent. Finish L2-networking in the cascaded OpenStack, including cross OpenStack networking.
* L3-Proxy: Similar role like L3-Agent. Finish L3-networking in the cascaded OpenStack, including cross OpenStack networking.
* FW-Proxy: Similar role like FWaaS-Agent. TBD
* LB-Proxy: Similar role like LBaaS-Agent. TBD
* VPN-Proxy: Similar role like VPNaaS-Agent. TBD

Openstack Cascade 包含Nova、Cinder、Neutron、Glance和Ceilometer的级联。但是Keystone和Heat将作为在级联中分享的全局服务。



详细内容可以看这一段[Openstack Cascade官方文档](https://wiki.openstack.org/wiki/OpenStack_cascading_solution)


