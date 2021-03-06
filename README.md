# Tomcat Install
Tomcat Install

安装tomcatTomcat的安装分为两个步骤：安装JDK和安装Tomcat.
JDK(Java Development Kit)是Sun Microsystems针对Java开发员的产品。自从Java推出以来，JDK已经成为使用最广泛的Java SDK. JDK是整个Java的核心，包括了Java运行环境，Java工具和Java基础的类库。所以要想运行jsp的程序必须要有JDK的支持，理所当然安装Tomcat的前提是安装好JDK.
安装JDK
下载jdk-7u79-linux-x64.tar.gz
```
cd /usr/local/src/
wget wget http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz?AuthParam=1478670268_cfcc4cd435916f22c348a5197cc00899
```
你也可以从官方网站(http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)下载其他版本。
```
tar zsvf jdk-7u79-linux-x64.tar.gz?AuthParam=1478670268_cfcc4cd435916f22c348a5197cc00899
```
它会自动把文件解压出来，到最后会提示 “Press Enter to continue.....”， 只需要按一下回车就可以了。
```
mv  jdk1.7.0_79  /usr/local/
```
设置环境变量

```
vim /etc/profile

```
在末尾输入以下内容:
```
JAVA_HOME=/usr/local/jdk1.7.0_79/
JAVA_BIN=/usr/local/jdk1.7.0_79/bin
JRE_HOME=/usr/local/jdk1.7.0_79/jre
PATH=$PATH:/usr/local/jdk1.7.0_79/bin:/usr/local/jdk1.7.0_79/jre/bin
CLASSPATH=/usr/local/jdk1.7.0_79/jre/lib:/usr/local/jdk1.7.0_79/lib:/usr/local/jdk1.7.0_79/jre/lib/charsets.jar
export  JAVA_HOME  JAVA_BIN JRE_HOME  PATH  CLASSPATH
```
保存文件后，使其生效:
```
source /etc/profile
```
检测是否设置正确:

```
java -version
```
如果显示如下内容，则配置正确:
```
java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b05)
Java HotSpot(TM) Client VM (build 19.0-b09, mixed mode, sharing)
```
# 安装Tomcat
上面介绍了那么多内容，仅仅是在为安装tomcat做准备工作而已，现在才是安装tomcat.

```
cd /usr/local/src/
wget http://www.aminglinux.com/bbs/data/attachment/forum/apache-tomcat-7.0.14.tar.gz
```
如果觉得这个版本不适合，可以到官方网站(http://tomcat.apache.org/)下载。
```
tar zxvf apache-tomcat-7.0.14.tar.gz
mv apache-tomcat-7.0.14 /usr/local/tomcat
cp -p /usr/local/tomcat/bin/catalina.sh /etc/init.d/tomcat
vim /etc/init.d/tomcat
```
在第二行加入以下内容:
```
# chkconfig: 112 63 37
# description: tomcat server init script
# Source Function Library
. /etc/init.d/functions

JAVA_HOME=/usr/local/jdk1.7.0_79/
CATALINA_HOME=/usr/local/tomcat
```
保存文件后，执行以下操作:
```
chmod 755 /etc/init.d/tomcat
chkconfig --add tomcat
chkconfig tomcat on
```
启动tomcat:
```
service tomcat start
```
查看是否启动成功:
```
ps aux |grep tomcat
```
如果有进程的话，请在浏览器中输入http://IP:8080/ 你会看到tomcat的主界面。
配置tomcat1. 配置tomcat服务的访问端口
tomcat默认启动的是8080，如果你想修改为80，则需要修改server.xml文件:
vim /usr/local/tomcat/conf/server.xml
找到:
<Connector port="8080" protocol="HTTP/1.1"

修改为:
<Connector port="80" protocol="HTTP/1.1"

2. 配置新的虚拟主机
cd /usr/local/tomcat/conf/
vim server.xml

找到</Host>下一行插入新的<Host>内容如下:
<Host name="www.123.cn" appBase="/data/tomcatweb"
    unpackWARs="false" autoDeploy="true"
    xmlValidation="false" xmlNamespaceAware="false">
    <Context path="" docBase="./" debug="0" reloadable="true" crossContext="true"/>
</Host>

保存后，重启tomcat:
```
service tomcat stop
service tomcat start
```
测试tomcat先创建tomcat的测试文件:
vim /data/tomcatweb/111.jsp

加入如下内容:
<html><body><center>
    Now time is: <%=new java.util.Date()%>
</center></body></html>

保存后，使用curl测试:
[root@localhost ~]# curl -xlocalhost:80 www.123.cn/111.jsp

看看运行结果是否是:
<html><body><center>
    Now time is: Thu Jun 13 15:26:03 CST 2013
</center></body></html>

如果是这样的结果，说明tomcat搭建成功。另外，你也可以在你的真机上，绑定hosts, 用IE来测试它。

