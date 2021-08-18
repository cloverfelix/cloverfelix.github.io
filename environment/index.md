# Environment


# Linux编程环境和软件安装

## 1、安装JDK

去官网地址下载JDK：[https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

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
cd /usr/local
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

先去官网下载node：[https://nodejs.org/en/](https://nodejs.org/en/)

历史版本链接：[https://nodejs.org/dist/](https://nodejs.org/dist/)

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

去官网下载Redis：[https://redis.io/download](https://redis.io/download)

历史版本：[http://download.redis.io/releases/](http://download.redis.io/releases/)

先去`/usr/local`下创建一个redis文件，然后将root目录下的redis解压到该文件中

~~~bash
cd /usr/local/
mkdir redis
cd redis
tar -zxvf /root/redis-6.2.4.tar.gz -C ./
~~~

解压完毕后，进入redis-6.2.4文件中执行下面命令

**注意：如果你所装虚拟机为最小化介质安装，则需要先安装gcc，执行yum -install gcc后，才能执行下面命令，否则会报错**

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

先去官网下载Nginx：[http://nginx.org/en/download.html](http://nginx.org/en/download.html)

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

>编译安装Nginx

~~~bash
cd nginx-1.17.10 
./configure 
make && make install
~~~

安装完成后，Nginx的可执⾏⽂件位置位于

~~~bash
/usr/local/nginx/sbin/nginx
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

## 6、安装Mysql

先去官网下载Mysql：[https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)

如果系统之前⾃带 Mariadb ，可以先卸载之。 

⾸先查询已安装的 Mariadb 安装包:

~~~bash
rpm -qa|grep mariadb
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705215633.png)

将其均卸载之

~~~bash
# 使用该四条命令可删除全部
yum -y remove mariadb-server-5.5.56-2.el7.x86_64 
yum -y remove mariadb-5.5.56-2.el7.x86_64 
yum -y remove mariadb-devel-5.5.56-2.el7.x86_64 
yum -y remove mariadb-libs-5.5.56-2.el7.x86_64

# 但是用该条命令可以直接删除完毕
yum -y remove mariadb-libs.x86_64
~~~

先去`/usr/local`下创建一个mysql文件，然后将root目录下的mysql解压到该文件中

~~~bash
cd /usr/local
mkdir mysql
cd mysql

tar -zxvf /root/mysql-5.7.34-linux-glibc2.12-x86_64.tar.gz -C ./
~~~

>创建MYSQL⽤户和⽤户组

~~~bash
groupadd mysql 
useradd -g mysql mysql
~~~

同时新建`/usr/local/mysql/data`⽬录，后续备⽤

>修改MYSQL⽬录的归属⽤户

~~~bash
chown -R mysql:mysql ./
~~~

>准备MYSQL的配置⽂件

在`/etc`⽬录下新建`my.cnf`⽂件

写入如下简化配置

~~~bash
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
socket=/var/lib/mysql/mysql.sock
[mysqld]
skip-name-resolve
#设置3306端⼝
port = 3306
socket=/var/lib/mysql/mysql.sock
# 设置mysql的安装⽬录
basedir=/usr/local/mysql
# 设置mysql数据库的数据的存放⽬录
datadir=/usr/local/mysql/data
# 允许最⼤连接数
max_connections=200
# 服务端使⽤的字符集默认为8⽐特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使⽤的默认存储引擎
default-storage-engine=INNODB
lower_case_table_names=1
max_allowed_packet=16M
~~~

同时使⽤如下命令创建`/var/lib/mysql`⽬录，并修改权限：

~~~bash
mkdir /var/lib/mysql 
chmod 777 /var/lib/mysql
~~~

>正式开始安装MYSQL

~~~bash
cd /usr/local/mysql 
./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql -- datadir=/usr/local/mysql/data
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705231038.png)

**注意：记住上⾯打印出来的`root`的密码，后⾯⾸次登陆需要使用**

>复制启动脚本到资源⽬录

~~~bash
cp ./support-files/mysql.server /etc/init.d/mysqld
~~~

并修改`/etc/init.d/mysqld` ，修改其 `basedir` 和 `datadir` 为实际对应⽬录：

~~~bash
basedir=/usr/local/mysql 
datadir=/usr/local/mysql/data
~~~

>设置MYSQL系统服务并开启⾃启

首先增加`mysqld`服务控制脚本执⾏权限：

~~~bash
chmod +x /etc/init.d/mysqld
~~~

同时将`mysqld`服务加⼊到系统服务：

~~~bash
chkconfig --add mysqld
~~~

最后检查`mysqld`服务是否已经⽣效即可：

~~~bash
chkconfig --list mysqld
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705222254.png)

这样就表明`mysqld`服务已经⽣效了，在2、3、4、5运⾏级别随系统启动⽽⾃动启动，以后可以直接使 ⽤`service`命令控制`mysql`的启停.

直接执行

~~~bash
service mysqld start
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705231424.png)

>将Mysql的BIN目录加入到PATH环境变量

这样方便以后任意目录上都可以使用mysql提供的命令

编辑`/etc/profile`文件，在文件末尾处追加如下信息

~~~bash
export PATH=$PATH:/usr/local/mysql/bin
~~~

最后执行如下命令使环境变量生效

~~~bash
source /etc/profile
~~~

>首次登录Mysql

以root账户登录Mysql，使用上文安装完成提示的密码进行登入

~~~bash
mysql -u root -p
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210705231852.png)

>接下来修改ROOT账户密码

在mysql的命令执行如下命令即可，密码可以换成你想设置的密码即可

~~~bash
alter user user() identified by "xn123456";

flush privileges;
~~~

>设置远程主机登录

~~~bash
use mysql;

update user set user.Host='%' where user.User='root';

flush privileges;
~~~

## 6、安装Tomcat

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
