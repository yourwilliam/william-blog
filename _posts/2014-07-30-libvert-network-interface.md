---
layout: post
title: libvert 虚拟化网络配置详解
description: "libvert 虚拟化网络配置详解，分析libvert所支持的几种联网方式"
modified: 2014-07-30
category: articles
tags: [libvert,interface,network,虚拟化,网络]
comments: true
share: true
---

##libvert 虚拟化网络配置详解

###基本虚拟libvert虚拟化网络

<figure>
     <a href="{{ site.url }}/images/blog2014/libvert_network_architecture.png"><img src="{{ site.url }}/images/blog2014/libvert_network_architecture.png"></a>
     <figcaption>libvert network 网络架构图</figcaption>
</figure>

####bridge 和 tap/tun
桥是一个接口，桥由多个接口共同构成，在以太层把每一个接口收到的数据复制到其他接口。向桥写入数据时，桥内所有的接口都会收到

tap/tun简单的说时一块虚拟网卡。tap是以太层设备，tun是IP层设备。用户空间的程序向tap/tun写入数据，这些数据会传送到内核的网络模块；内核的网络模块写入数据，这些数据又可以被用户空间的程序读到。实现了宿主和Guest之间的共享接口。当guest机器的IP和宿主IP在同一个子网中时，连上这块虚拟网卡的虚拟机就可以和宿主通信。

#### bridge模式
Linux每启动一个VM，就会为这个VM创建一个tap设备，名字叫vnetx，然后将这块虚拟网卡加入桥设备brx，桥设备具体和那一块网卡绑定由用户自己决定。

	...
	  <devices>
	    <interface type='bridge'>
	      <source bridge='br0'/>
	      <mac address='00:14:41:12:ac'/>
	    </interface>
	...

其中整个操作如下：
1. 安装uml-utilities和bridge-utils这两个工具分别含有tunctl和brctl命令。
2. 生成一个新的TAP接口
	`tunctl -t tap1 -u`
3. 生成一个叫做br0的bridge
	`brctl addbr br0`
4. 把真实网卡加到bridge br0的一端
	`brctl addif br0 eth0`
5. 把上面生成的TAP接口加到bridge br0的另一端
	`brctl addif br0 tap1`
6. 激活TAP
	`fconfig tap1 up`
7. 设置/dev/net/tun的读写权限
	`chmod 0666 /dev/net/tun`


#### NAT模式
NAT： 网络地址转换
NAT分为： DNAT把数据包的目的地改称指定地址， SNAT把数据包的原地址改为指定地址。

SNAT的作用就是在宿主机内部构造一个独立的虚拟子网。Guest就是子网中的一台机器，当子网中的数据要发送出来的时候，这些数据的源地址会改为宿主机的实际网络地址，这样就实现了IP伪装。
Libvert创建了virbr0网桥，作为default虚拟网络的网关，并打开了STP。Libvert每启动一个VM的时候，同样为这个VM创建一个TAP设备。该tap设备连接到网桥之后，就可以访问这个虚拟网络的其他虚拟机。网桥可以通过NAT转发到外网，因为这个NAT使用Linux的iptables实现的，所以可以使用iptables规则。

	...
	  <devices>
	    <interface type='network'>
	      <source network='default'/>
	      <mac address='00:14:41:12:ac'/>
	    </interface>
	...
该配置表示连接到default这个虚拟网络，default虚拟网络的配置文件位于/var/lib/libvert/network/default.xml
具体详细配置可以参考


### libvert 虚拟化网络详细配置

libvert xml详细使用可以参考官方的这篇文档[http://libvirt.org/formatdomain.html#elementsNICS](http://libvirt.org/formatdomain.html#elementsNICS)
这里专注于libvert的网络看一下当前libvert支持哪些网络，这也同样可以看出openstack当前支持哪些网络。

* Virtual network
* Bridge to LAN
* Userspace SLIRP stack
* Generic ethernet connection
* Direct attachment to physical interface
* PCI Passthrough
* Multicast tunnel
* TCP tunnel
* Setting the NIC model
* Setting NIC driver-specific options
* Overriding the target element
* Specifying boot order
* Interface ROM BIOS configuration
* Quality of service
* Setting VLAN tag (on supported network types only)
* Modifying virtual link state
* vhost-user interface




####Virtual network

> This is the recommended config for general guest connectivity on hosts with dynamic / wireless networking configs (or multi-host environments where the host hardware details are described separately in a <network> definition Since 0.9.4).

virtual network定义了在HOST上的一种动态的／无线的连接配置。(就是NAT模式)
在网络下可以配置一个或多个portgroup，每个portgroup为不同的网络连接配置了不同的配置信息。
virtualport定义了该虚拟端口的下一跳，比如 vepa (802.1Qbg) 或者 802.1Qbh compliant switch 或者 Open vSwitch virtual switch。


	...
	  <devices>
	    <interface type='network'>
	      <source network='default'/>
	    </interface>
	...
	    <interface type='network'>
	      <source network='default' portgroup='engineering'/>
	      <target dev='vnet7'/>
	      <mac address="00:11:22:33:44:55"/>
	      <virtualport>
	        <parameters instanceid='09b11c53-8b5c-4eeb-8f00-d84eaa0aaa4f'/>
	      </virtualport>

	    </interface>
	  </devices>
	...

#### Open vSwitch virtual switch

>This is the recommended config for general guest connectivity on hosts with static wired networking configs.

OVS switch配置静态的有线网络。 （bridge模式）
bridge提供虚拟机直接连接到LAN的功能。为虚拟机提供类似物理机的网络访问功能。
在Linux系统，bridge设备一般使用标准Linux host bridge 。 在支持OpenVSwitrh的HOST上同样可以将虚拟机对接到相应的OpenVSwitch端口上。



	...
	  <devices>
	    ...
	    <interface type='bridge'>
	      <source bridge='br0'/>
	    </interface>
	    <interface type='bridge'>
	      <source bridge='br1'/>
	      <target dev='vnet7'/>
	      <mac address="00:11:22:33:44:55"/>
	    </interface>
	    <interface type='bridge'>
	      <source bridge='ovsbr'/>
	      <virtualport type='openvswitch'>
	        <parameters profileid='menial' interfaceid='09b11c53-8b5c-4eeb-8f00-d84eaa0aaa4f'/>
	      </virtualport>
	    </interface>
	    ...
	  </devices>
	...

#### Userspace SLIRP stack 

>Provides a virtual LAN with NAT to the outside world.This networking is the only option for unprivileged users who need their VMs to have outgoing access.

使用NAT的方式将虚拟机网络直接对接到外部。仅用于需要访问外部的无特权用户。

	...
	 <devices>
	   <interface type='user'/>
	   ...
	   <interface type='user'>
	     <mac address="00:11:22:33:44:55"/>
	   </interface>
	 </devices>
	...

#### Generic ethernet connection

>Provides a means for the administrator to execute an arbitrary script to connect the guest's network to the LAN. 

	...
	  <devices>
	    <interface type='ethernet'/>
	    ...
	    <interface type='ethernet'>
	      <target dev='vnet7'/>
	      <script path='/etc/qemu-ifup-mynet'/>
	    </interface>
	  </devices>
	...


#### Direct attachment to physical interface

>Provides direct attachment of the virtual machine's NIC to the given physical interface of the host. Since 0.7.7 (QEMU and KVM only)
>This setup requires the Linux macvtap driver to be available. 

可以支持3种形式的直连选择，'vepa','bridge','private'。 其中vepa为默认模式。

* vepa
    All VMs' packets are sent to the external bridge. Packets whose destination is a VM on the same host
* bridge
    Packets whose destination is on the same host as where they originate from are directly delivered to the target macvtap device. Both origin and destination devices need to be in bridge mode for direct delivery. If either one of them is in vepa mode, a VEPA capable bridge is required.
* private
    All packets are sent to the external bridge and will only be delivered to a target VM on the same host if they are sent through an external router or gateway and that device sends them back to the host. This procedure is followed if either the source or destination device is in private mode.
* passthrough
	This feature attaches a virtual function of a SRIOV capable NIC directly to a VM without losing the migration capability. All packets are sent to the VF/IF of the configured network device. Depending on the capabilities of the device additional prerequisites or limitations may apply; for example, on Linux this requires kernel 2.6.38 or newer. Since 0.9.2
	    
例子如下：

	...
	  <devices>
	    ...
	    <interface type='direct'>
	      <source dev='eth0' mode='vepa'/>
	    </interface>
	  </devices>
	...

	...
	<devices>
	  ...
	  <interface type='direct'>
	    <source dev='eth0.2' mode='vepa'/>
	    <virtualport type="802.1Qbg">
	      <parameters managerid="11" typeid="1193047" typeidversion="2" instanceid="09b11c53-8b5c-4eeb-8f00-d84eaa0aaa4f"/>
	    </virtualport>
	  </interface>
	</devices>
	...

	...
	  <devices>
	    ...
	    <interface type='direct'>
	      <source dev='eth0' mode='private'/>
	      <virtualport type='802.1Qbh'>
	        <parameters profileid='finance'/>
	      </virtualport>
	    </interface>
	  </devices>
	...

#### PCI Passthrough
>A PCI network device (specified by the <source> element) is directly assigned to the guest using generic device passthrough, after first optionally setting the device's MAC address to the configured value, and associating the device with an 802.1Qbh capable switch using an optionally specified <virtualport> element (see the examples of virtualport given above for type='direct' network devices).

用于将客户虚拟机直接绑定到PCI物理网卡，实现PCI的 passthrough.









