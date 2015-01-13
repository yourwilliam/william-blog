---
layout: post
title: maven本地仓库nexus
description: "maven项目建立越来越受到各个团队的欢迎，但烦恼于和远程仓库连接的缓慢速度，建立maven本地仓库来进行开发是每个团队必须进行的过程"
modified: 2013-02-13
category: articles
tags: [maven,nexus]
comments: true
share: true
---

#### nexus的安装

##### 下载nexus
	
在nexus官网可以[下载最新的nexus私服](http://www.sonatype.org/nexus/go/)，现在通过官网可以直接下载bundle方式，他提供一个整合的包，直接运行就能启动nexus环境。
##### 安装
	
下载后解压到本地`tar -zxvf`即可。 解压后设置配置文件`nexus-2.2-01/conf/nexus.properties`，可以修改监控ip和相应的端口。修改完成后进入`bin/`目录，通过`./nexus start`即可以启动nexus。 使用`./nexus stop`可以停止nexus服务。
##### 配置nexus私服

**登陆nexus** 
	
用本地链接加上相应端口： http://localhost:8081/nexus/， 默认用户名密码为admin/admin123。

**修改可以下载Repair index**

第一步修改仓库的默认下载索引为true， 点击Views/Repositories —> Repositories，找到右边的Apache Snapshots，Codehaus Snapshots和Central，把标签下面的Configuration下把Download Remote Indexes修改为true。然后将这三个工程右键选择Repair index。

**建立自己的私有库**

Repositories –> Add –> Hosted Repository，在页面的下半部分输入框中填入Repository ID和Repository Name即可，比如分别填入myrepo和 my repository，另外把Deployment Policy设置为Allow Redeploy，点击save就创建完成了

**修改nexus仓库组**

Nexus中仓库组的概念是Maven没有的，在Maven看来，不管你是hosted也好，proxy也好，或者group也好，对我都是一样的，我只管根据groupId，artifactId，version等信息向你要构件。为了方便Maven的配置，Nexus能够将多个仓库，hosted或者proxy合并成一个group，这样，Maven只需要依赖于一个group，便能使用所有该group包含的仓库的内容。

neuxs-2.2中默认自带了一个名为“Public Repositories”组，点击该组可以对他保护的仓库进行调整，把刚才建立的公司内部仓库加入其中，这样就不需要再在maven中明确指定内部仓库的地址了。同时创建一个Group ID为public-snapshots、Group Name为Public Snapshots Repositories的组，把Apache Snapshots、Codehaus Snapshots和Snapshots加入其中。


##### 修改maven的setting.xml配置文件

maven安装好默认是没有配置仓库信息，此时mavne都默认去远程的中心仓库下载所依赖的构件。既然使用了私服，就需要明确告诉maven去哪里下载构件，可以在三个地方进行配置，分别是是mavne的安装目录下的conf下的settings.xml文件，这个全局配置，再一个是在userprofile/.m2目录下的settings.xml,如果不存在该文件，可以复制conf下settings.xml，这个是针对当前用户的，还有一个是在pom.xml中指定，其只对该项目有效，三个文件的优先级是pom.xml大于.m2,.m2大于conf下。

settings.xml的配置如下：

在profiles段增加一个profile

	<profile>
      <id>nexus</id>
      <repositories>
        <repository>
            <id>nexus</id>
            <name>local private nexus</name>
            <url>http://192.168.1.68:8081/nexus/content/groups/public</url>
            <releases><enabled>true</enabled></releases>
            <snapshots><enabled>false</enabled></snapshots>
        </repository>
        <repository>
            <id>nexus-snapshots</id>
            <name>local private nexus</name>
            <url>http://192.168.1.68:8081/nexus/content/groups/public-snapshots</url>
            <releases><enabled>false</enabled></releases>
            <snapshots><enabled>true</enabled></snapshots>
        </repository>
      </repositories>
      <pluginRepositories>
        <pluginRepository>
            <id>nexus</id>
            <name>local private nexus</name>
            <url>http://192.168.1.68:8081/nexus/content/groups/public</url>
            <releases><enabled>true</enabled></releases>
            <snapshots><enabled>false</enabled></snapshots>
        </pluginRepository>
        <pluginRepository>
            <id>nexus-snapshots</id>
            <name>local private nexus</name>
            <url>http://192.168.1.68:8081/nexus/content/groups/public-snapshots</url>
            <releases><enabled>false</enabled></releases>
            <snapshots><enabled>true</enabled></snapshots>
        </pluginRepository>
       </pluginRepositories>
    </profile>


另外，仓库是两种主要构件的家。第一种构件被用作其它构件的依赖。这是中央仓库中存储的大部分构件类型。另外一种构件类型是插件。如果不配置pluginRepositories，那么执行maven动作时，还是会看到去远程中心库下载需要的插件构件，所以这里一定要加上这个。

在settings.xml中指使配置了profile还不行，需要激活它，在activeProfiles段，增加如下配置:

	<activeProfile>nexus</activeProfile>


##### 添加pom从本地仓库下载

	<repositories>
        <repository>
            <id>nexus-public</id>
            <name>nexus publiu</name>
            <url>http://localhost:8081/nexus/content/groups/public</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
        <repository>
            <id>nexus-snapshots</id>
            <name>nexus snapshots</name>
            <url>http://localhost:8081/nexus/content/groups/public-snapshots</url>
            <releases>
                <enabled>false</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>nexus-pulign</id>
            <name>nexus pluign</name>
            <url>http://localhost:8081/nexus/content/groups/public</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </pluginRepository>
        <pluginRepository>
            <id>nexus-snapshots</id>
            <name>neuxs snapshots</name>
            <url>http://localhost:8081/nexus/content/groups/public-snapshots</url>
            <releases>
                <enabled>false</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </pluginRepository>
    </pluginRepositories>


##### 添加第三方jar包依赖

有些自己导入的类需要导入到本地仓库的，可以在nexus中将pom文件和jar包导入第三方库即可。

具体方法：View/Repositories -> Repositories -> 3rd party -> artifact upload -> GAV Definition 选择 From POM (此时既是通过pom导入相关类) -> Select Artifact(s) for Upload -> 导入相应的jar 包和pom文件即可。


##### 添加第三方其他库文件

有些工程需要依赖第三方库文件。 可以在nexus中导入proxy工程的方式，就可以增加新的依赖库。

#####使用命令行建立Maven工程

使用命令行建立Maven工程的方法： 

        mvn archetype:generate -DgroupId=com.pengjunjie -DartifactId=idTest -DpackageName=nameTest -Dversion=1.0

archetype:create  create是个被废弃或不被推荐使用的插件，在以后创建项目中请尽量使用archetype:generate

创建一个项目，如下：

        mvn archetype:generate -DgroupId=com.demo.mvn -DartifactId=mvnDemo -DpackageName=mvnDemo -Dversion=1.0

进入要创建的目录

创建web工程：
        
        mvn archetype:create -DgroupId=com.demo.mvn -DartifactId=mvnDemo -DarchetypeArtifactId=maven-archetype-webapp

创建java工程：
        
        mvn archetype:create -DgroupId=com.demo.mvn -DartifactId=app

建好后，进入工程目录，
eclipse下编译：

        mvn eclipse:clean eclipse:eclipse
myeclipse下编译：

        mvn eclipse:myeclipse -Dwtpversion=2.0
