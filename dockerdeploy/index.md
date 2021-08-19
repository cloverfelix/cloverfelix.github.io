# DockerDeploy



# 使用Dockers部署若依项目

## 1、先进行docker安装

## 2、安装mysql

~~~bash
docker run -p 3307:3306 --name mysql01 -v /home/westbrook/mysql/log:/var/log/mysql  \
-v /home/westbrook/mysql/conf/my.cnf:/etc/mysql/my.cnf  \
-v /home/westbrook/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=xn123456 -d mysql:5.7.33
~~~

执行成功后

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818112038.png)

	注意：如果第一次直接执行该命令，它会自动的把`mysql/conf`下的my.cnf配置文件创建成为一个目录，会产生报错，需要你手动删除该目录后重建一个配置文件
	
开启远程连接时需要进行以下操作：

1、先查看用户信息

~~~bash
select host,user,plugin,authentication_string from mysql.user;
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818113227.png)

	备注：host为 % 表示不限制ip ，localhost表示本机使用 plugin非mysql_native_password 则需要修改密码

2、如果是8.0版本以上还需要修改加密规则，而我采用的是5.7，所以不需要修改，直接执行以下命令即可

~~~bash
flush privileges;
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818113428.png)

## 3、安装Toomcat

~~~bash
docker run --name tomcat01 -p 8081:8080 -d tomcat
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818114222.png)

执行命令成功以后，去本机访问时，发现资源未找到，这就是我们常说的一个问题，可以进容器查看

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818114110.png)

进入容器可以看到有一个`webapps.dist`文件夹，我们tomcat所需要的文件全部存放在其中，而不是存放在webapps目录下面

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818114538.png)

先将两个文件`server.xml`和`tomcat-users.xml`复制过去为后面做映射做准备

~~~bash
docker cp bb50d6e13752:/usr/local/tomcat/conf/server.xml /home/westbrook/tomcat/conf
docker cp bb50d6e13752:/usr/local/tomcat/conf/tomcat-users.xml /home/westbrook/tomcat/conf
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818115221.png)

然后清除之前运行起来的tomcat01

~~~bash
# 先停止运行
docker stop tomcat01

# 再移除tomcat01
docker rm tomcat01
~~~

拉取tomcat镜像并运行

~~~bash
docker run --name tomcat01 -p 8081:8080 -v /home/westbrook/tomcat/webapps:/usr/local/tomcat/webapps \
-v /home/westbrook/tomcat/conf/server.xml:/usr/local/tomcat/conf/server.xml \
-v /home/westbrook/tomcat/conf/tomcat-users.xml:/usr/local/tomcat/conf/tomcat-users.xml \
-v /home/westbrook/tomcat/logs:/usr/local/tomcat/logs -d tomcat
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818115643.png)

现在再次访问还是访问不到

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818120042.png)


我们进入容器中，将webapps.dist文件中的资源拷贝到webapps中去

~~~bash
# 进入容器
docker exec -it tomcat01 /bin/bash

# 进入webapps.dist目录中，将文件中的内容复制到webapps目录下
cp -r * ../webapps
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818120250.png)

因为这里进行了数据卷的挂载，实现了容器内与宿主机的数据绑定，所以再容器中将文件复制到了webapps中后，宿主机中的webapps文件也自动同步对应容器文件中的数据，所以可以访问成功！

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818120557.png)

## 4、安装redis

~~~bash
docker run -p 6380:6379 --name redis01 \
-v /home/westbrook/redis/data:/data \
-v /home/westbrook/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf
# -d以后台的方式启动；redis是要运行什么镜像；redis-server以后面这个配置文件路径的方式启动
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818152416.png)

## 5、安装nginx

~~~bash
docker run --name nginx01 -p 81:80  \
-v /home/westbrook/nginx/conf/nginx.conf:/etc/nginx/nginx.conf  \
-v /home/westbrook/nginx/html:/usr/share/nginx/html \
-v /home/westbrook/nginx/logs:/var/log/nginx \
-d nginx
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818153427.png)

	注意：如果再拉取镜像并运行的时候进行数据卷的挂载，虽然会成功，但是nginx会直接挂掉；原因是因为你在宿主机建立一个空的配置文件来进行映射是行不同的；
	
**解决方法：先运行一个nginx镜像不进行数据卷的挂载，获取到它的配置文件后再次进行数据卷挂载**

## 6、前端部署

先进入若依前端目录，进入目录后再文件夹目录地方输入cmd

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818160951.png)

然后输入`cnpm install`安装依赖

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818161047.png)

在输入`npm build:prod`对项目进行打包，如果再命令行出现这种报错

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818161159.png)

那就去vs code中的`package.json`文件中找到`build:prod`命令，鼠标放在上面出现运行脚本后点击运行，让其开始打包

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818161336.png)

打包完成后生成了一个`dist`文件夹，将其拷贝到服务器上`/home/westbrook/nginx/html`目录下

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818161507.png)

同时进入服务器该`/home/westbrook/nginx/conf`目录下，编辑nginx配置文件

~~~bash
server {
	listen  80;
	server_name 192.168.167.48;
	#charset koi8-r;

	#access_log logs/host.access.log main;

	location / {
		root /usr/share/nginx/html/dist;
		index index.html index.htm;
	}

}
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818161834.png)


最后重启nginx容器即可！`docker restart nginx01`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818161944.png)

## 7、部署后端

打开若依项目，先修改`resources`目录下的`logback.xml`文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818163003.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818163043.png)

然后再修改`application-druid.yml`文件中数据库配置

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818163143.png)

再修改`application.yml`文件中关于redis配置以及路径配置

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818163255.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818163318.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818163445.png)

**如果想要打jar包，克隆下来后直接运行就可以了**

**如果想打war包就得进行一些修改**

首先修改`pom.xml`文件，引入tomcat依赖将`tomcat`内置的依赖去掉，因为springboot自动集成了tomcat，打war包就得去掉。

然后继续修改`pom.xml`文件，将`packaging`修改成你所要打包对应的类型

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210818163950.png)

后端还未部署起来，还在继续测试！！！


