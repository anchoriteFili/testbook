---
layout: post
#标题
title:  XMPP安装_openFire安装配置
#时间配置
date:   2015-06-29 10:24:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


### XMPP安装
---


* <a href="http://www.baidu.com/link?url=l7dcMAv9ADJX7K6AAd4M_PvFEj6961q8tJVV-uJ9E_mQ3NKjvE9q3T-JJEJ8Cl4KDi-u8XgbLx43rIJDJm79NK&wd=&eqid=aef236590000a9c1000000035590b91a" target="_blank">XMPP_百度百科</a><br>
* <a href="http://www.baidu.com/link?url=hy33rGxFWpjQcgDHwtShmGJgJXNgRWUkrV3t7YFkGboTOjaYa1nwJHaQHci8NSxvzHi4P-moHijH9iYFNQqcHK&wd=&eqid=ac916c2e00009a5a000000035591367f" target="_blank">通过XMPP协议实现即时通讯介绍 - 崔同亮的个人空间 - 开源中国社区</a><br>
* <a href="http://blog.csdn.net/winer888/article/details/49886281" target="_blank">Mac OS10.10 openfire服务器无法启动</a><br>
* <a href="http://www.baike369.com/content/?id=5415" target="_blank">开启phpMyAdmin高级功能的设置方法</a><br>
* <a href="http://www.cnblogs.com/lmyhao/p/4120616.html" target="_blank">IOS基于XMPP协议开发－－XMPPFramewok框架（一）：基础知识</a><br>


**Mac OS10.10 openfire服务器无法启动**

**打开终端，按顺序输入以下命令：(注意细小的标点符号，建议逐一复制命令到终端运行)**

> ①：sudo chmod -R 777 /usr/local/openfire/bin
> ②：sudo su
> ③：cd /usr/local/openfire/bin
> ④：export JAVA_HOME=`/usr/libexec/java_home`
> ⑤：echo $JAVA_HOME /Library/Java/JavaVirtualMachines/jdk1.8.0_51.jdk/Contents/Home
> ⑥:   cd /usr/local/openfire/bin
> ⑦:  ./openfire.sh


### openFire安装配置
---


<a href="http://www.cnblogs.com/maxinliang/p/3582924.html" target="_blank">XMPP聊天之Openfire 的安装和配置---Mac OS</a><br>

#### 一、下载并安装openfire

##### 1、下载最新的openfire安装文件

> 官方下载站点：<a href="http://www.igniterealtime.org/downloads/index.jsp#openfire" target="_blank">http://www.igniterealtime.org/downloads/index.jsp#openfire</a><br>

![051710009095973.png]({{ site.img_url }}232EA9FE0AC4411ECFAD1553B0956588.png)

> openfire是服务器，下面还有一个spark，这个是一个XMPP协议通信聊天的CS的IM软件，它可以通过openfire进行聊天对话。


##### 2、 点击安装，并执行默认操作

![05111108-7008338c269f418bbdeb24738b6fe14f.png]({{ site.img_url }}870CF9AC441B9B5DF51A7AD755094060.png)

##### 3、 启动openfire服务

**在系统偏好设置的其他里，点击openfire偏好**

![05105724-eccaadfb8ae644a0bcc15a57fd7ea1a1-2.png]({{ site.img_url }}2C7CE3B9DAC32B4F69A5DE54F96EE3D4.png)

> 启动后，点击Open Admin Console按钮，自动在浏览器中打开本地web配置页面http://localhost:9090/setup/index.jsp

#### 二、配置openfire服务器

##### 1.设置语言，选中文

![05112100-bf50028050b04bc29723b869732b15d7.png]({{ site.img_url }}D15CF1E0D81106F66A3BDEA5BA3548BD.png)

##### 2.主机设置

**设置主机的访问ip地址**

![05112442-e66c30fe8aae45cab131a376352589b7.png]({{ site.img_url }}C48B0F1ED288D8F344E20EB3B696AEB2.png)

**注意：域不能是机器名，否则会如下错误：**

> HTTP ERROR: 500 INTERNAL_SERVER_ERROR

> 本地的域，要设置为127.0.0.1

##### 3.数据库设置

**如果要设置外部数据库（推荐，比如：MySQL），选择标准数据库连接**

![05113046-9d6ae81014e84de396da3b75c908f54e.png]({{ site.img_url }}6861816D234B1F9636D361579F677A35.png)

##### 4.设置数据库连接

![05113427-85763ec2b939454da5147fe75802eb20.png]({{ site.img_url }}25D4BB30085208CAE56085A4D3381AD3.png)

* （1）数据库驱动选择：MySQL，前提是已安装MySQL（具体的安装方法可以参考上一篇：mac上安装MySQL）

* （2）JDBC驱动，默认不变
  * `com.mysql.jdbc.Driver`

* 数据库URL：
  * **形式如下：**
`jdbc:mysql://你的主机名:端口号/数据库名称`
  * 这里设置为
  `jdbc:mysql://localhost:3306/openfire`
  其中主机名[host-name]改为localhost，
  其中数据库名称[database-name]改为openfire---》sql已创建

**注意：前提是已存在一个名为openfire的数据库，否则会报如下错误，连接配置不成功**

> The Openfire database schema does not appear to be installed. Follow the installation guide to fix this error. 

* （4）用户名和密码

> 这里的用户名密码，是访问MySQL数据库时使用的帐号：root，和安装MySQL设置的root密码

##### 5.特性设置

**如果不打算使用LDAP，则保持默认设置**

![05134329-46acad61845c487e84bcd2d401cfc4ec.png]({{ site.img_url }}D90BD614D53C20F84DC92B2B1B3F46B5.png)

##### 6.设置openfire服务器管理员的帐号和密码

![05150558-85e7fc4b52d348129162a0527049c5c9.png]({{ site.img_url }}82679E868A4BD36F1339B673F1876C55.png)

**可以随便填写一个管理员邮箱，输入要设置的密码**

**完成注册**

![05150730-d9571894a99c49f0ac988658399f15eb.png]({{ site.img_url }}8C267EE8666522805660678292348BFC.png)

 

##### 7.登陆管理控制台

 

**默认的管理员帐号是“admin”，默认管理员密码“admin”，如果上面设置了新密码，则管理员密码是新密码**

![05150139-16fa991f77c34a3f95b65625a82555fa.png]({{ site.img_url }}2ABD2381A4B90DE84899E67C684E0DA3.png)


**如果想去掉默认的admin帐号，并自定义，需要如下操作**

 

* （1）在终端中，登陆具体的数据库（openfire）
  * `mysql -u root -p openfire`
  * 然后输入数据库的root密码

* （2）删除表“ofUser”中的admin帐户
  * delete from ofUser where username='admin';
 
* （3）创建自定义管理员（用户名：xiaodao，密码：123）

```shell
INSERT INTO ofUser (username, plainPassword, encryptedPassword, name, email, creationDate, modificationDate) VALUES ('xiaodao','123','123','Administrator','xiaodao@sunyard.com','0','0');
```
  **注意：如果重设了用户名，必须重启openfire服务器** 
  
![05135545-2dba07de9424415bb966deb7e72b9e15.png]({{ site.img_url }}63622C8B404DA0D3315570EF114D4DB7.png)
  
##### 8.后台控制界面

![05154035-b35318ee6f3c4fbaad51115305df14e9.png]({{ site.img_url }}90BA616FD2063B6C2218A0DEDF3CAFCE.png)

#### 三、卸载openfire

##### 1.停止服务

**在系统偏好设置的其他里，打开openfire偏好设置**

![05105724-eccaadfb8ae644a0bcc15a57fd7ea1a1.png]({{ site.img_url }}2C7CE3B9DAC32B4F69A5DE54F96EE3D4.png)

**点击Stop Openfire按钮，停止服务**

![05105634-60e3f33c673940aaa169f36df81a8918.png]({{ site.img_url }}A2637F30B300B9FA95B313EC58E52C79.png)

##### 2.删除文件

**打开终端，输入以下命令**

```ruby
sudo rm -rf /Library/PreferencePanes/Openfire.prefPane
sudo rm -rf /usr/local/openfire
sudo rm -rf /Library/LaunchDaemons/org.jivesoftware.openfire.plist
```

**其中第一条命令之后，需要输入本机管理员密码**
 