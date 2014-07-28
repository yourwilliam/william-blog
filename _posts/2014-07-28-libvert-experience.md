---
layout: post
title: libvert基本功能及相关接口分析
description: "libvert基本功能及相关接口分析"
modified: 2014-07-28
category: articles
tags: [libvert]
comments: true
share: true
---

## KVM

KVM（Kernel-based Virtual Machine）是基于内核的虚拟化解决方案，目前Intel和AMD的CPU都对其提供了好很的支持。

平时KVM也会被说成是管理KVM虚拟化的一个工具，类似于qemu（quick emulator）。在网上看的文档，有的说KVM只能在物理机上做虚拟化，而qemu还适合在虚拟机上面进一步做虚拟化。目前大家常用的KVM虚拟化工具都是qemu。

## KVM基本操作

#### 虚拟机创建
创建img镜像文件（快照）

	# cd /var/instances
	# qemu-img create ubuntu.img 5G
	# cd /var/instances
	# kvm -hda ubuntu.img -cdrom ubuntu-12.04-server-amd64.iso -boot d -m 512
创建镜像并创建虚拟机。

	# cd /var/instances
	#virt-install --name=ubuntu --ram=512 --vcpu=2 \
    --disk path=/var/instances/ubuntu.img,size=5 \
    --cdrom=/var/images/ubuntu-12.04-server-amd64.iso \
    --graphics=vnc,listen=0.0.0.0

另一种创建虚拟机的方式。
virt-install命令在virtinst包里，在CentOS下该包名为python-virtinst，其实最终调用的命令还是qemu-img和qemu-kvm。

#### 操作系统安装

vnc登入安装操作系统，完成之后会在相应目录下生成镜像文件及xml配置文件（/etc/libvirt/qemu/ubuntu.xml）。

更多相关内容: [http://en.wikibooks.org/wiki/QEMU/Images#Mounting_an_image_on_the_host](http://en.wikibooks.org/wiki/QEMU/Images#Mounting_an_image_on_the_host)

## libvirt

libvirt是一套免费、开源的支持Linux下主流虚拟化工具的C函数库，这个函数库是一种实现Linux虚拟化功能的API，它支持虚拟机监控程序，比如Xen, KVM, Qemu等。

### 虚拟机基本信息

#### libvert虚拟机管理

安装完成之后，会生成相应的XML文件，libvert对于虚拟机的管理就针对这个XML文件，管理的命令如下：

	# virsh list
	# virsh list --all
	# virsh create /etc/libvirt/qemu/ubuntu.xml
	# virsh define /etc/libvirt/qemu/ubuntu.xml
	# virsh undefine /etc/libvirt/qemu/ubuntu.xml
	# virsh autostart domain_id|instance_name
	# virsh destory domain_id|instance_name
	# virsh start/shutdown/reboot/... domain_id|instance_name

#### libvirt XML文件格式

#####常规信息区域

	...
	<domain type='xen' id='3'>
	    <name>instance-name</name>
	    <uuid>d9ef885b-634a-4437-adb6-e7abe1f792a5</uuid>
	    <title>A short description - title - of the domain</title>
	    <description>Some human readable description</description>
	    <metadata>
	        <app1:foo xmlns:app1="http://app1.org/app1/">..</app1:foo>
	        <app2:bar xmlns:app2="http://app1.org/app2/">..</app2:bar>
	    </metadata>
	    ...
type是虚拟化类型，其值可以是kvm, xen, qemu, lxc, kqemu等。id是标识正在运行的虚拟机，可以省略。

*   name
	虚拟机的名字，可以由数字、字母、中横线和下划线组成。
*   uuid
	虚拟机的全局唯一标识，可以用uuidgen命令生成。如果在定义（define）或创建（create）虚拟机实例时省略，系统会自动分配一个随机值这个实例。
*   title, description
	这两个东西都可以省略，见名知义，如果有特殊需求可以加上。
*   metadata
	metadata可以被应用（applications）以xml格式来存放自定义的metadata，该项也可以省略。

##### 操作系统启动区域
    
    ...
    <os>
        <type arch='x86_64' machine='pc'>hvm</type>
        <boot dev='hd'/>
        <bootmenu enable='yes'/>
        <kernel>/var/instances/instance-hostname/kernel</kernel>
        <initrd>/var/instances/instance-hostname/ramdisk</initrd>
        <cmdline>root=/dev/vda console=ttyS0</cmdline>
    </os>
    ...

*   type
	虚拟机启动的操作系统类型，hvm表示操作系统是在裸设备上运行的，需要完全虚拟化。
*   boot
	boot属性的值可以是fd, hd, cdrom, network等，用来定义下一个启动方式（启动顺序）。该属性可以有多个。
*   bootmenu
	在虚拟机启动时是否弹出启动菜单，该属性缺省是弹出启动菜单。
*   kernel
	内核镜像文件的绝对路径。
*   initrd
	ramdisk镜像文件的绝对路径，该属性是可选的。
*   cmdline
	这个属性主要是在内核启动时传递一些参数给它。

##### 内存和CPU区域

	...
    <vcpu placement='static' cpuset="1-4,^3,6" current="1">2</vcpu>
    <memory unit='KiB'>2097152</memory>
    <currentMemory unit='KiB'>2000000</currentMemory>
	...
*   vcpu
	vcpu属性表示分配给虚拟机实例的最大CPU个数。其中cpuset表示该vcpu可以运行在哪个物理CPU上面，一般如果设置cpuset，那么placement就设置成static。current的意思是是否允许虚拟机使用较少的CPU个数（current can be used to specify whether fewer than the maximum number of virtual CPUs should be enabled）。vcpu下面的这几个属性貌似只在kvm与qemu中有。
*   memory
	memory表示分配给虚拟机实例的最大内存大小。unit是内存的计算单位，可以是KB, KiB, MB, MiB，默认为Kib。（1KB=10^3bytes，1KiB=2^10bytes）
*   currentMemory
	currentMemory表示实际分配给虚拟实例的内存，该值可以比memory的值小。

##### 磁盘区域

	...
	<devices>
	    <emulator>/usr/bin/kvm</emulator>	

	    <disk type='file' device='disk'>
	        <source file='/var/instances/instance-hostname/disk' />
	        <target dev='vda' bus='ide' />
	    </disk>	

	    <disk type='file' device='disk'>
	        <driver name='qemu' type='raw'/>
	        <source file='/var/instances/instance-hostname/disk.raw'/>
	        <target dev='vda' bus='virtio'/>
	        <alias name='virtio-disk0'/>
	        <address type='pci' domain='0x0000' bus='0x00' slot='0x04' function='0x0'/>
	    </disk>	

	    <disk type='block' device='disk'>
	        <driver name='qemu' type='raw'/>
	        <source dev='/dev/sdc'/>
	        <geometry cyls='16383' heads='16' secs='63' trans='lba'/>
	        <blockio logical_block_size='512' physical_block_size='4096'/>
	        <target dev='hda' bus='ide'/>
	    </disk>
	</devices>
	...
*   emulator
	模拟器的二进制文件全路径。
*   disk
	定义单块虚拟机实例上的磁盘。
*   type
	可以是block, file, dir, network。分别表示什么意思就不多说了。
*   device
	表示该磁盘以什么形式暴露给虚拟机实例，可以是disk, floppy, cdrom, lun，默认为disk。
*   driver
	可以定义对disk更为详细的使用结节。
*   source
	定义磁盘的源地址，由type来确定该值应该是文件、目录或设备地址。
*   target
	控制着磁盘的bus/device以什么方式暴露给虚拟机实例，可以是ide, scsi, virtio, sen, usb, sata等，如果未设置的系统会自动根据设备名字来确定。如： 设备名字为hda那么就是ide。
*   mirror
	这个mirror属性比较牛B，可以将虚拟机实例的某个盘做镜像，具体不细说了。

##### 网络接口区域

	...
	<devices>
	    <interface type='bridge'>
	        <source bridge='br0'/>
	        <mac address='00:16:3e:5d:c7:9e'/>
	        <model type='virtio'/>
	    </interface>
	</devices>
	...
*   type
	表示该接口的类型，可以是bridge等类型


##### 相关事件的配置

    ...
    <on_poweroff>destroy</on_poweroff>
    <on_reboot>restart</on_reboot>
    <on_crash>restart</on_crash>
    <on_lockfailure>poweroff</on_lockfailure>
    ...

on_poweroff, on_reboot, on_crash
其属性值为遇到这三项时进行的操作，可以是以下操作：

* destroy 虚拟机实例完全终止并且释放所占资源。
* restart 虚拟机实例终止并以相同配置重新启动。
* preserve 虚拟机实例终止，但其所占资源保留来做进一步的分析。
* rename-restart 虚拟机实例终止并且以一个新名字来重新启动。

on_crash 还支持以下操作：

* coredump-destroy crash的虚拟机实例会发生coredump，然后该实例完全终止并释放所占资源。
* coredump-restart crash的虚拟机实例会发生coredump，然后该实例会重新启动。

on_lockfailure
当锁管理器（lock manager）失去对资源的控制时（lose resource locks）所采取的操作：

* poweroff 虚拟机实例被强制停止。
* restart 虚拟机实例被停止后再启动来重新获取它的锁（locks）。
* pause 虚拟机实例会被暂停，并且当你解决了锁（lock）问题后可以将其手动恢复运行。
* ignore 让虚拟机实例继续运行，仿佛一切都没发生过。

##### 时间区域

	<clock offset='localtime'/>

offset支持utc, localtime, timezone, variable等四个值，表示虚拟机实例以什么方式与宿主机同步时间。（并不是所有虚拟化技术都支持这些模式）

##### 图形管理接口

	...
	<devices>
	    <graphics type='vnc' port='5904'>
	        <listen type='address' address='1.2.3.4'/>
	    </graphics>
	    <graphics type='vnc' port='-1' autoport='yes' keymap='en-us' listen='0.0.0.0'/>
	</devices>
	...
type为管理类型，可以是VNC,rdp等。其中port可以自动分配（从5900开始分配）。

##### 日志记录

	...
	<devices>
	    <console type='stdio'>
	        <target port='1'/>
	    </console>
	    <serial type="file">
	        <source path='/var/instances/instance-hostname/console.log'/>
	        <target port="1"/>
	    </serial>
	</devices>
	...
以上意思是禁止字符设备的输入，并将其输出定向到虚拟机的日志文件中（domain log）。将设备的日志写到一个文件里（Device log），比如：开机时的屏幕输出。


###虚拟网卡

#### Linux Tun/Tap虚拟网卡
##### 介绍：
Tun/Tap 驱动程序实现了虚拟网卡的功能，tun表示虚拟的是点对点设备，tap表示虚拟的是以太网设备，这两种设备针对网络包实施不同的封装。利用tun/tap 驱动，可以将tcp/ip协议栈处理好的网络分包传给任何一个使用tun/tap驱动的进程，由进程重新处理后再发到物理链路中。
开源项目[openvpn](http://openvpn.sourceforge.net)和[Vtun](http://vtun.sourceforge.net)都是利用tun/tap驱动实现的隧道封装。

##### 工作原理：
做为虚拟网卡驱动，Tun/Tap驱动程序的数据接收和发送并不直接和真实网卡打交道，他在Linux内核中添加了一个TUN/TAP虚拟网络设备的驱动程序和一个与之相关连的字符设备 /dev/net/tun，字符设备tun作为用户空间和内核空间交换数据的接口。当内核将数据包发送到虚拟网络设备时，数据包被保存在设备相关的一个队 列中，直到用户空间程序通过打开的字符设备tun的描述符读取时，它才会被拷贝到用户空间的缓冲区中，其效果就相当于，数据包直接发送到了用户空间。通过 系统调用write发送数据包时其原理与此类似。
在linux下，要实现内核空间和用户空间数据的交互，有多种方式：可以通用socket创建特殊套接字，利用套接字实现数据交互；通过proc文件系统创建文件来进行数据交互；还可以使用设备文件的方式，访问设备文件会调用设备驱动相应的例程，设备驱动本身就是内核空间和用户空间的一个接口，Tun/tap驱动就是利用设备文件实现用户空间和内核空间的数据交互。
从结构上来说，Tun/tap驱动并不单纯是实现网卡驱动，同时它还实现了字符设备驱动部分。以字符设备的方式连接用户空间和内核空间。
Tun/tap 驱动程序中包含两个部分，一部分是字符设备驱动，还有一部分是网卡驱动部分。利用网卡驱动部分接收来自TCP/IP协议栈的网络分包并发送或者反过来将接收到的网络分包传给协议栈处理，而字符驱动部分则将网络分包在用户空间和内核空间之间传送，模拟物理链路的数据接收和发送。Tun/tap驱动很好的实现了两种驱动的结合。

##### bridge虚拟网卡创建











