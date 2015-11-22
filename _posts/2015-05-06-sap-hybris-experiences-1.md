---
layout: post
title: Hybris 经验总结——1
description: "Hybris经验总结-1"
modified: 2015-04-26
category: articles
tags: [Hybris]
comments: true
share: true
---

#### Hybris 数据库、端口配置文件

配置文件地址： hybris-commerce-suite-5.2.0.1/hybris/bin/platform/project.properties
可提供配置： Hybris 端口、Hybris容器配置、数据库

如果使用MySQL的话，需要自己创建数据库 

MySQL配置：

db.url=jdbc:mysql://localhost/hybris?useConfigs=maxPerformance&characterEncoding=utf8
db.driver=com.mysql.jdbc.Driver
db.username=root
db.password=stella
db.tableprefix=
db.customsessionsql=SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
mysql.optional.tabledefs=CHARSET=utf8 COLLATE=utf8_bin
mysql.tabletype=InnoDB
mysql.allow.fractional.seconds=true



####Hybris 组合包的问题

Hybris分为 Module(accelerator)、extension、page、组件等。
其中最大的为accelerator，这个可以理解为每个大的加速器模板。模板下面为extension，每一个extension为一个类似插件的形式进行扩展，一个accelerator可以包含多个extension。

每个extension都含有 project.properties和extensioninfo.xml 两个配置文件。
* project.properties配置每个extension的spring文件位置，url相关地址等extension相关规格。
* extension.info配置模块间的引用关系

local.properties 全局配置文件，用于配置全局参数，这个文件会覆盖每个extension里面的配置，所以一般把数据库连接，tomcat配置等配置在这里。

localextensions.xml 

#### 系统的Initialization 和 Update

Initialization发生时：系统将会重建、所有的数据库信息将会重建、数据模型将会重建、建立新的数据库、之前的数据模型将会删除。
Update发生时：数据库将会更新到新的模型，数据不会丢失，将在数据库中添加新的定义类型，

####Extension概念
1. 用自己的数据模型和商务逻辑开发一个新的工程，不用于添加代码，提供更加方便的代码重用和整合到最新的版本
2. Extension可以提供下面集中类型的扩展：business logic、Data model definition、web application。
3. 新创建的extension默认不被其他的extension引用，除了platform以外
4. extension的优势：方便整合和更新。
5. extension之间可以互相依赖，但是不能循环依赖。
6. 所有的extension默认依赖于平台
7. 每一个extension默认是一个Eclipse工程
8. 通过extensioninfo.xml来配置extension之间的依赖关系
9. 对于bean的定义有三种定义：global-{ext-name}.xml 、 {ext-name}-spring.xml 、web-application-config.xml。其中配置在global中的bean会被所有的extension共享，配置在ext-name-spring中的bean同样也会被所有的extension共享同时对不同的租户会创建不同的实例；配置在web-application-config中的bean只对所选中的extension和tenant使用。

####Mac下启动Hybris

导入环境变量 

PLATFORM_HOME=`pwd`
export -p PLATFORM_HOME
export -p ANT_OPTS="-Xmx200m -XX:MaxPermSize=128M"
export -p ANT_HOME=$PLATFORM_HOME/apache-ant-1.9.1
chmod +x "$ANT_HOME/bin/ant"
export -p PATH=$ANT_HOME/bin:$PATH

启动Hybris服务器

		sh hybriserver.sh


####各大Portal

* page - http://whantrade1.com:9001/yb2bacceleratorstorefront/
* hac - http://127.0.0.1:9001/  
* hmc - http://127.0.0.1:9001/hmc
* cms - http://whantrade1.com:9001/cmscockpit/
* mcc - http://whantrade1.com:9001/mcc/

使用当前域名可以进入到当前网站下的配置页面，使用127.0.0.1配置的为全局页面。


####环境整理

把Ant换成 1.9 版本。

页面 —— structure*Template   页面模板 


####项目创建

1. 新建一个module，module可以视为很多个accelerator的集合。创建Module(Accelerator)使用 ant modulegen，创建Extention的时候使用 ant genext.
2. 创建完成之后可以修改相应的模块依赖，将Hybris目录下面的sampleconfigurations 中得配置文件拷贝到config目录下面的localextensions.xml文件中。然后将自己创建的extension添加到相应的extension中。
3. 这里一定要注意的是一定要删除重复的accelerator extesion。否则进行ant clean all的时候会报错。
4. 使用ant clean all进行重新发布。
5. 发布之后可以修改item文件，修改item文件的目的是创建一个数据库持久化对象，并能对这个对象自动生成很多方法
6. item中得itemtype分为两种，一种是自动生成新表的，可以加上`<deployment table="OrganizationOrderStats" typecode="6214"/>`，另外一种是可以生成在系统本身有的表里面的，不带这个字段。
7. 添加完成itemtype之后Update一下即可。

item代码如下
		
	<itemtype code="WilliamForTestItemProduct" extends="VariantProduct"
	        autocreate="true" generate="true"
	        jaloclass="org.training.core.jalo.WilliamForTestItemProduct">
			 <description>William Test Powertools size variant type that contains additional attribute describing variant size.
			</description>
			<attributes>
				<attribute qualifier="size" type="localized:java.lang.String"
				           metatype="VariantAttributeDescriptor">
					<description>Size of the product.</description>
					<modifiers/>
					<persistence type="property"/>
				</attribute>
			</attributes>
		</itemtype>

8. 修改core下面的hmc.xml
	
修改代码如下：

	<explorertree mode="append">
		<group name="user" expand="false" description="group.user.description">
         <typeref type="WilliamForTestItemProduct" description="type.WilliamForTestItemProduct.name"/>
      </group>
	</explorertree>


9.hmc提供两种修改方式，一种是通过自己generator一个extension来创建一个工程，然后在里面修改。另外一种方式是直接在Core的hmc里面直接修改。 hmc当前只支持单表的查询，如果需要支持多表查询则必须进行扩展。   hmc页面的总hmc-configuration选项是集合了所有的hmc文件的内容，实际上hmc的机制就是这样。




####Product 

ProductModel   baseProduct 
VariantProductModel  封装了baseProduct在里面

多语言支持在数据库的响应字段后面增加一个后缀为ip的数据库表 参考 productsip
ecache 缓存机制，看看是不是写到内存里面去了，不然的话肯定会慢。

itemtype里面可以增加多语言属性 增加一个type=localized:java.lang.string


itemtype 里面， 产品层级  BaseProduct —— WFJStyleVarientProducts  —— WSJCounterVariantProduct 
可以在varient 里面定义多级从而来定义产品的层级。


####Catagory   把产品化类别
Catalog 绑定用户权限的东西， 作为最顶级的对象。 网站的用户级别。
Catagory 绑定下面的产品结构，网站的类目级别。


####Classification  属性系统
classification  系统， 所有的商品属性组都是按照classification来做，如果把Product和Classification切断之后，Product就没有任何属性了。

在Classification中定义属性，这些属性还可以有继承关系。 


#### 产品价格  Price Rows    price setting中
定义一个价格的有效期、范围。
产品  吊牌价、真实价格。
一个产品可以关联很多个price row ， 真正起作用的根据关联属性，根据真是价格关联到Price Row。

一般都是一个产品对应一个或多个PriceRow，但是基本不会出现多个产品对应一个PriceRow的，根据业务需求来。
价格可以设置时间段来判断相应的价格。


权限的功能，在用户的后面添加一个select语句就可以进行相关权限限制。

#### HMC 开发 

webchart 的开发元素




https://wiki.hybris.com/display/release5/How+To+Write+an+hMC+Editor+WebChip

https://wiki.hybris.com/display/release5/How+To+Create+a+Wizard



在hmc上面做一个excel导入的方法，让客户使用excel的方式导入产品。
联表查询
定制化显示字段
鼠标右键动态开发


ImpExImportJob  这个可以用于用代码导入Impex文件。


https://wiki.hybris.com/display/accdoc/B2C-Specific+Accelerator+Extensions

https://wiki.hybris.com/display/release5/Classification+Guide

https://wiki.hybris.com/display/release5/Modeling+Product+Variants

https://wiki.hybris.com/display/release5/Restrictions

https://wiki.hybris.com/display/release4/How+To+Create+Restrictions+Using+hybris+Management+Console+-+Tutorial

https://wiki.hybris.com/display/forum/User+Rights+Import


https://wiki.hybris.com/display/release5/How+To+Create+a+Wizard

https://wiki.hybris.com/display/accdoc/B2C-Specific+Accelerator+Extensions


#### hybris日志 
platform 实际上是一个tomcat

#### 查数据
一般是使用flaxibleSearch  来用代码查询数据
查询基本上全部都是使用这种方式来实现的。

提供一个单例的转换模式，使用DAO 查询DTO之后，使用一个单例的转换模式，然后转换成自己建的bean，用于页面展现。


用户密码可以自己做一套加密的算法和老系统保持一致，在添加用户的时候可以看到

PasswordEncode 类用于密码加解密。如果需要在HMC里面显示的话，需要做一次配置，在spring.xml里面配置，配置一个bean，用于专门的加密，持久化到数据库。


####页面  
cms2 Data model overview

cms2 Extension Tutorial


base store ---- website

cmscockpit 进入修改页面，使用自己的域名进去

首页位置
/Users/valentine/workspace/hybris-commerce-suite-5.5.0.0/hybris/bin/custom/training/trainingstorefront/web/webroot/_ui/desktop

页面模板的位置
/Users/valentine/workspace/hybris-commerce-suite-5.5.0.0/hybris/bin/custom/training/trainingstorefront/web/webroot/WEB-INF/views


页面怎么做
https://wiki.hybris.com/display/release5/cms2+Extension+Tutorial

页面框架图
https://wiki.hybris.com/display/release5/cms2+-+Data+Model+Overview

如何增加一个新的CMS组件
https://wiki.hybris.com/display/release5/How+to+Add+a+New+CMS+Component

WCMS组件信息
https://wiki.hybris.com/display/accdoc/WCMS+Components


WCMS是一个编辑环境， 所有的展示东西都是模板里面，在WCMS改得时候


主页生效要在WCMS里面把生效点一下。


wiki ： WCMS components 可以看到所有的组件接口

页面组件数据都是可以在jsp里面修改的，具体的进去搜索 navigatebar*.jsp



导入数据入口类
/Users/valentine/workspace/hybris-commerce-suite-5.5.0.0/hybris/bin/custom/training/traininginitialdata/src/org/training/initialdata/setup/InitialDataSystemSetup.java

/Users/valentine/workspace/hybris-commerce-suite-5.5.0.0/hybris/bin/custom/training/trainingcore/src/org/training/core/setup/CoreSystemSetup.java

自己的做法：
自己项目下面的，新建一个工程，自己写初始化类（像上面定义的）归结上面要倒的数据。然后加进去

wiki : customize initialization process 


页面目录的内容管理：
/Users/valentine/workspace/hybris-commerce-suite-5.5.0.0/hybris/bin/ext-template/yacceleratorinitialdata/resources/yacceleratorinitialdata/import/coredata/contentCatalogs/catalogName/cms-content_zh.impex



#### wrapper 
wrapper  把tomcat打一下，在tomcat奔溃的时候会自动重启

####B2B 

admincockpit     提供一个B2B admin管理方式


addonsupport
secureportaladdon
加上这两个扩展。

ant addoninstall -Daddonnames="..."
使用这个ant命令可以添加addon，实际上添加addon和普通的添加扩展是不一样的。

邮件需要配置一个邮件服务器。
注册完成之后，登陆hmc之后，在Enployment里面添加 审批  approval ，然后在admincockpit里面就可以看到审核消息。

客户核心以单位进行管理，用户必须要设置一个用户单位。

其他的cockpit使用zk的js架构去做扩展

project.properties 写入程序变量


####查询 
一般后台使用flexsearch查询

https://wiki.hybris.com/display/release5/FlexibleSearch

Update 和 Delete 效率比较低，插入较好。 设计的时候尽量的去做插入，少做Update和Delete。  
做更新的时候，很多情况可以做两张表，一边用来做insert，然后不停的做查询。



wiki ：MediaConversion

wiki  拦截器 ：interceptors 

https://wiki.hybris.com/display/accdoc/B2B

https://wiki.hybris.com/display/release5/Interceptors


动态多表查询  Query  Hmc里面有

动态属性概念，用于多表查询的时候，把里面的某个字段（引用字段） 展示为多表之间的关系。

solr 全文检索系统，下面配置了lucene。

