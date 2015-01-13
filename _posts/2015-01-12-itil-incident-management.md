---
layout: post
title: ITIL——事件管理
description: "ITIL——事件管理"
modified: 2015-01-12
category: articles
tags: [ITIL]
comments: true
share: true
---

###ITIL--事件管理

####事件管理

* 事件：在某一服务中不属于标准操作的并能导致、或可能导致这个服务的终端或服务质量下降的任何事件。
* ITIL将服务台接受到得几乎所有呼叫——只要这种呼叫是可以记录的和可监控的——都称之为事件。
* **事件不仅包括软件与硬件故障，还包括服务请求。**事件管理设计服务的整个生命周期。
* **服务请求(Service Request)**：用户想要获得支持、递送、信息、建议或文档，它并不属于IT基础设施方面的故障。是根据服务级别协议(SLA,Service Level Agreement)提供的。其提供是根据既定程序进行，而这些既定程序是经过协商的且具有适当的检查和控制功能，它应该保存在维护记录的地方，如配置管理数据库(CMDB,Configuration Management Database)。
* 如果被请求的服务不是事先定义好的标准服务，并且它将改变IT基础设施的状态，那么我们将根据此提交一个**变更请求(RFC,Request for Change)**，变更请求RFC并不由事件管理流程处理，而是由变更管理流程负责对其进行正式的处理。
* **影响度(impact)**: 就锁影响的用户或业务数量而言，时间偏离正常服务级别的程度。
* **紧急度(urgency)**: 解决故障时，对用户或者业务来说可以接受的耽搁时间。
* **优先级(priority)**: 基于影响度和紧急度来决定。
* **升级**：如果某一事件不能在规定时间内由一线支持小组解决，那么更多有经验的人员和有更高权限的人员将不得不参与进来，这就是升级。升级又分职能性升级和结构性升级。**职能性升级(Functional escalation,水平升级，技术性升级)**：意味着需要具有更多时间、专业技能或访问权限的人员来参与事件的解决。**结构性升级(Hierarchical escalation，垂直升级，管理升级)**：意味着当经授权的当前级别的机构不足以保证事件能及时、满意地得到解决时，需要更高级别的机构参与进来。
* **1线支持**：通常由服务台来提供。**2线支持**：通常由管理部门提供。**3线支持**：通常由软件开发人员和系统结构人员提供。**4线支持**：通常由供应商提供。
* 事件管理活动和其他活动关系：

<figure>
     <a href="{{ site.url }}/images/blog2015/ITIL_Incident_management_and_other_process.png"><img src="{{ site.url }}/images/blog2015/ITIL_Incident_management_and_other_process.png"></a>
     <figcaption>事件管理活动和其他活动关系</figcaption>
</figure>
* 事件管理流程：

<figure>
     <a href="{{ site.url }}/images/blog2015/ITIL_Incident_management_process.png"><img src="{{ site.url }}/images/blog2015/ITIL_Incident_management_process.png"></a>
     <figcaption>事件管理流程</figcaption>
</figure>