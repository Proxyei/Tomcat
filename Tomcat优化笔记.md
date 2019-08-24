# 目标（基于Tomcat 7）

## 优化什么？

## 怎么优化？

- Tomcat启动优化
- Tomcat并发优化
- Tomcat内存优化（内存是JVM还是Tomcat自身）
- Apache的ab压力测试
- Tomcat的BIO/NIO/APR三大模式

# 1、术语

webserver：中间件，像Nginx，Apache。

appserver：容器，应用服务器，像Tomcat，weblogic。

# 2、Tomcat启动优化

在catalina.sh（Linux）进行配置。

## 相关参数（研究一下区分大小写不）

- -server：表示启用jdk的server版本，生产环境必须这样设置。
- -Xms/-Xmx：最小堆，最大堆内存，设置相等防止内存的浮动。
- -Xss：设置每个线程的栈大小，一般不超过1m。
- -XX:AggressiveOpts：必须配置，表示将最新版本的jdk的特性自动注入。
- -XX:UseBiasedLocking：表示启用了优化了的线程锁，对于高并发访问很重要，并发量太大的时候，会自动优化。
- -XX:PermSize=256m：JDK8之后换成了-XX:MetaSpace
- -XX:MaxPermSize=512m：同上
- -XX:MaxNewSize：
- -XX:NewSize：与上一个交给JDK完成。
- -XX:+DisableExplicitGC：表示程序里面不允许有显式使用System.gc()，避免内存的大起大落。
- -XX:MaxTeruringThreshold：默认15，表示年老代最大存活次数。数值越大，越能降低出现内存溢出的概率。
- -Djava.awt.headless=true：避免Java图形界面下跨平台出现的问题。

# 2、Tomcat并发优化

## 1、参数说明：



## 2、配置：

在server.xml中的<connector>节点中配置。

```xml
 71     <Connector port="8780" protocol="HTTP/1.1"
 72                connectionTimeout="20000"
 73                redirectPort="8443" />
```

HTTP/1.1：表示BIO，默认

优化配置：

```xml
<Connector port="8780" protocol="org.apache.coyote.http11.Http11NioProtocal" 
           maxThreads="600" minSpareTheads="100" maxSpareTheads="500" acceptCount="700"
           connectionTimeout="20000" redirectPort="8443"/>
```

# 3、Tomcat内存优化

Tomcat默认内存是128m，可以通过设置JAVA_OPTS参数来解决堆/方法区内存溢出的错误。



# 4、ab压力测试



# 5、BIO/NIO/APR模式

## 1、BIO



## 2、NIO



## 3、APR



