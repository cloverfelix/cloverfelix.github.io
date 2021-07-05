# Environment


# Linux编程环境和软件安装

## 1、安装JDK

去官网地址下载JDK：https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html

第一步，先卸载掉服务器自带的openJDK，通过命令查看已安装的openJDK

~~~bash
rpm -qa | grep java
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705170027.png)

然后将`java`开头的删除完毕即可

~~~bash
yum -y remove java-1.7.0-openjdk-1.7.0.141-2.6.10.5.el7.x86_64
......
也可以执行，一次删除，删完后可以再次查看一次
yum -y remove javapackages-tools.noarch
~~~


然后将/root目录下的jdk解压到`/usr/local/java`路径下，java文件夹是你自己创建的

~~~bash
cd /etc/local
mkdir java
tar -zxvf /root/jdk-8u161-linux-x64.tar.gz -C ./
~~~

再去`/etc/profile`文件中配置环境变量

~~~bash
JAVA_HOME=/usr/local/java/jdk1.8.0_291
CLASSPATH=$JAVA_HOME/lib
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME CLASSPATH
~~~

然后执行命令使配置文件生效

~~~bash
source /etc/profile
~~~

输入下面命令检查结果

~~~bash
java -version 

javac
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705170915.png)

## 2、安装Node

先去官网下载node：https://nodejs.org/en/

历史版本链接：https://nodejs.org/dist/

先在`/usr/local`下创建一个node文件，然后将root目录下的node解压到当前node目录下

~~~bash
cd /usr/local/ 
mkdir node
cd node
tar -zxvf /root/node-v12.16.0-linux-x64.tar.gz -C ./
~~~

解压完成后，去`/etc/profile`文件中配置环境变量

~~~bash
export PATH=/usr/local/node/node-v12.16.0-linux-x64/bin:$PATH
~~~

然后刷新文件

~~~bash
source /etc/profile
~~~

使用下面命令检查安装结果

~~~bash
node -v 
npm -v
npx -v
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705171942.png)

## 3、安装Maven

去官网下载Maven：[http://maven.apache.org/](http://maven.apache.org/)  
maven 下载地址：[http://maven.apache.org/download.cgi](http://maven.apache.org/download.cgi)  
maven 历史版本：[https://archive.apache.org/dist/maven/maven-3/](https://archive.apache.org/dist/maven/maven-3/)

先去`/opt`目录下创建一个maven文件，然后将roo目录下的maven其解压到该文件中

~~~bash
cd /opt
mkdir maven
tar -zxvf /root/apache-maven-3.6.3-bin.tar.gz -C ./
~~~

然后配置阿里的镜像

编辑修改` /opt/maven/apache-maven-3.6.3/conf/settings.xml`文件

在`<mirrors></mirrors>`标签对⾥添加如下内容即可

~~~bash
<mirror>
	 <id>alimaven</id>
	 <name>aliyun maven</name>
	 <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
	 <mirrorOf>central</mirrorOf>
</mirror>
~~~

镜像配置完成后，去`/etc/profile`文件中配置环境变量

~~~bash
export MAVEN_HOME=/opt/maven/apache-maven-3.6.3
export PATH=$MAVEN_HOME/bin:$PATH
~~~

配置完环境变量，执行命令使配置文件生效

~~~bash
source /etc/profile
~~~

使用下面命令检查结果

~~~bash
mvn -v
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705173246.png)

## 4、安装Redis

去官网下载Redis：https://redis.io/download

历史版本：http://download.redis.io/releases/

先去`/usr/local`下创建一个redis文件，然后将root目录下的redis解压到该文件中

~~~bash
cd /usr/local/
mkdir redis
cd redis
tar -zxvf /root/redis-6.2.4.tar.gz -C ./
~~~

解压完毕后，进入redis-6.2.4文件中执行下面命令

~~~bash
make  && make install
~~~

>将 REDIS 安装为系统服务并后台启动，进⼊ utils ⽬录，并执⾏如下脚本即可

~~~bash
cd utils/ 
./install_server.sh
~~~

使用高版本的Redis的时候执行`./install_server.sh`，会报错

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705174729.png)

**解决方法：** 进入./install_server.sh文件中，对其编辑，将其中一段代码注释即可

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705174913.png)

再次执行`./install_server.sh`，即可成功，此处所有配置使用默认的

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705175028.png)

> 查看REDIS服务启动情况

~~~bash
systemctl status redis_6379.service
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705175225.png)

然后启动客户端进行测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705175849.png)

但是此时只能在本地访问，⽆法远程连接，因此还需要做部分设

>设置允许远程连接

编辑`redis`配置文件

~~~bash
vim /etc/redis/6379.conf
~~~

将 bind 127.0.0.1 修改为 0.0.0.0

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705180306.png)

然后重启 Redis 服务即可：

~~~bash
systemctl restart redis_6379.service
~~~

>设置密码

编辑redis配置文件,并且找到如下内容，去掉注释，将`foobared`修改为⾃⼰想要的密码，保存即可，最后重启redis

~~~bash
vim /etc/redis/6379.conf

#requirepass foobared

systemctl restart redis_6379.service
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705191011.png)

这样后续的访问需要先输⼊密码认证通过⽅可：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705191526.png)

## 5、安装Nginx

先去官网下载Nginx：http://nginx.org/en/download.html

先去`/usr/local`下创建一个nginx文件，然后将root目录下的nginx解压到该文件中

~~~bash
cd /usr/local/ 
mkdir nginx 
cd nginx

tar -zxvf /root/nginx-1.18.0.tar.gz -C ./
~~~

> 预先安装额外的依赖

~~~bash
yum -y install pcre-devel
yum -y install openssl openssl-devel
~~~

安装完成后，Nginx的可执⾏⽂件位置位于

~~~bash
/usr/local/nginx/sbin/nginx
~~~

>编译安装Nginx

~~~bash
cd nginx-1.17.10 
./configure 
make && make install
~~~

>启动Nginx

直接执行如下命令就可以了

~~~bash
/usr/local/nginx/sbin/nginx
~~~

如果想停⽌Nginx服务，可执⾏：

~~~bash
/usr/local/nginx/sbin/nginx -s stop
~~~

如果修改了配置⽂件后想重新加载Nginx，可执⾏：

~~~bash
/usr/local/nginx/sbin/nginx -s reload 
~~~

注意其配置⽂件位于： 

~~~bash
/usr/local/nginx/conf/nginx.conf
~~~

>浏览器验证启动情况 

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705195459.png)

出现访问不了的问题有两个原因

1. 是自己搭配的服务器，那就是看防火墙开了80端口没有

2.如果使用的是阿里或者腾讯云的要去网络安全组把80端口打开

执行下面命令打开防火墙的80端口

~~~bash
# 查看防火墙规则
firewall-cmd --list-all

# 查询端口是否开放
firewall-cmd --query-port=8080/tcp

# 开放80端口
firewall-cmd --permanent --add-port=80/tcp

# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp

# 重启防火墙（修改配置后要重启防火墙）
firewall-cmd --reload

# 参数解释：
firewall-cmd：是Linux提供的操作firewall的一个工具
--permanent：表示设置为持久
--add-port：标识添加的端口
~~~

开放端口号访问的结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705200439.png)

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~


~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~

~~~bash
~~~
