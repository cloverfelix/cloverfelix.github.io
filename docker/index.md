# Docker



## Docker

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601110908.png)

### 参考资料

官方文档：https://docs.docker.com/docker-for-windows/ 

【官方文档超级详细】

仓库地址：https://hub.docker.com/

【发布到仓库，git pull push】

b站教程：https://www.bilibili.com/video/BV1og4y1q7M4?

【这个教程非常简洁！且深入！基于企业应用场景！推荐！以下笔记都基于该课程】

### 前期基础

linux基本命令，类似cd，mkdir等



### Docker概述

#### Docker为什么会出现

一款产品，开发和上线两套环境，应用环境配置费时费力，而且容易出问题，尤其

对于机器学习和深度学习的库更是如此，很可能存在版本问题、底层依赖冲突问题

所以发布项目时，不只是一套代码过去，而是代码+环境整体打包过去，所谓开发

即运维，保证系统稳定性，提高部署效率

使用Docker后的流程：

- 开发：建立模型--环境--打包带上环境，即镜像--放到Docker仓库

- 部署：下载Docker中的镜像，直接运行即可



**Docker的思想来自于集装箱，集装箱，对环境进行隔离**

**Docker通过隔离机制，可以将服务器利用到极致。**

#### Docker的历史

2010年，几个搞IT的人，在美国成立一家公司`dotCloud`，做一些pass的云计服

务，他们将自己的容器化技术命名为Docker，Docker基于Go语言开发，Docker刚

刚诞生的时候，没有引起行业的注意，dotCloud活不下去，然后他们决定开源

2013年，创始人将Docker开源，不开则以，一开惊人，刚开源的时候，每个月都

会更新一个版本，2014年4月9日，Docker 1.0发布

#### 容器vs虚拟机

	在容器技术出来之前，用的是虚拟机技术

#### 虚拟机原理示意图

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601110955.png)

**缺点**：
1. 资源占用多
2. 冗余步骤多
3. 启动很慢

##### 容器化技术示意图

不是模拟的完整的操作系统

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111016.png)

##### 二者对比

比较虚拟机和Docker的不同

|          | 传统虚拟机               | Docker        |
| -------- | ------------------------ | ------------- |
| 虚拟内容 | 硬件+完整的操作系统+软件 | APP+LIB       |
| 大小     | 笨重，通常几个G          | 轻便几个M或KB |
| 启动速度 | 慢，分钟级               | 快，秒级      |

### Docker安装

#### Docker的基本组成

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111108.png)

明确几个概念：

1. **镜像(image)**：docker镜像好比一个模板，可以通过这个模板来创建容器(container)，一个镜像可以创建多个容器，类似Python中的Class

2. **容器(container)**：类似Python中通过Class创建的实例，Object；容器可以理解为一个简易的系统

3. **仓库(repository)**：存放镜像的地方，

   分为公有仓库和私有仓库

   - Docker Hub：国外的

   - 阿里云：配置镜像加速

#### 环境准备

我们要有一台服务器，并且可以操作它

1. Linux命令基础，购买linux阿里云的服务器
2. CentOS 7
3. 使用Xshell链接远程服务器

#### 安装xshell

下载CentOS7 https://www.jianshu.com/p/a63f47e096e8

下载VMware 360软件管家下载

VMware配置虚拟机 https://blog.csdn.net/babyxue/article/details/80970526

xshell链接服务器 https://blog.csdn.net/zzy1078689276/article/details/77280814

```
[root@192 ~]# cd /
[root@192 /]# pwd
/
[root@192 /]# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@192 /]# uname -r
3.10.0-1127.el7.x86_64
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111141.png)

用户名 

密码

查看ip：vmware里面输入，ip addIPr

https://www.bilibili.com/video/BV1og4y1q7M4?p=6

#### Centos安装

https://docs.docker.com/engine/install/centos/

#### 卸载旧的版本

```
# 卸载旧的版本
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111157.png)

#### 安装基本环境



```
# 安装基本的安装包
$ sudo yum install -y yum-utils
```


![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111309.png)

#### 设置镜像的仓库

	注意！！下载默认用国外的，太慢不要用！

	用国内镜像，百度搜索，docker的阿里云镜像地址

```
# 不要用官网默认这个！
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo # 默认是国外的

# 换成下面的

$ sudo yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo # 阿里云镜像
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111340.png)

直接复制粘贴就OK了

更新软件包索引

```
yum makecache fast
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111404.png)

没有问题的话就是可以用的

#### 安装docker引擎

```python
yum install docker-ce docker-ce-cli containerd.io # docker-ce 社区版 ee 企业版
```

	注意这里会有几个个y/n的判断

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111427.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111446.png)

要看到Complet再收手！



#### 启动Docker

```
systemctl start docker # 代表启动成功
```

```
docker version
```

```
Client: Docker Engine - Community
 Version:           19.03.11
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        42e35e61f3
 Built:             Mon Jun  1 09:13:48 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.11
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       42e35e61f3
  Built:            Mon Jun  1 09:12:26 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683

```

```
docker run hello-world
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111510.png)

中间一堆是签名信息

run的运行流程图

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111642.png)

查看下载的镜像

```
docker images
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111528.png)

#### 卸载Docker

```
# 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io

# 删除资源
rm -rf /var/lib/docker # docker 的默认工作路径

```

#### 阿里云镜像加速

支付宝扫码登录，短信验证，确认授权

我有两个问题

1. 阿里云镜像加速必须配置嘛？

   >哎呦我去: 加速快
   >
   >哎呦我去: 要是翻墙就无所谓了

2. 这个阿里云必须要买嘛，买哪个，我看阿里云好多产品

   >哎呦我去:不需要买
   >
   >哎呦我去:免费的
   >
   >哎呦我去:阿里云搜索容器服务
   >
   >哎呦我去:有一个镜像加速

   

3. 这个地址是哪来的呀

操作

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111707.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111722.png)

控制台搜索 容器镜像服务

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111736.png)



找到加速地址

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111750.png)

```
sudo mkdir -p /etc/docker # 创建一个目录
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://uyfgafsw.mirror.aliyuncs.com"]
}
EOF # 编写配置文件

sudo systemctl daemon-reload # 重启服务
sudo systemctl restart docker # 重启docker
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111810.png)



[在线画图软件ProcessOn](https://www.processon.com/)



[服务器及购买相关介绍](https://www.bilibili.com/video/BV177411K7bH)

### 底层原理

Docker是怎么工作的？

Docker是一个Client-Server结构的系统，Docker的守护进程在主机上。通过Socket从客户端访问！

DockerServer接受到Docker-Client的指令，

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111827.png)

Docker为什么比VM快？

	1. Docker有着比虚拟机更少的抽象层
	2. docker主要用的是宿主机的内核，vm需要Guest OS

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111840.png)

所以说新建一个容器的时候，docker不需要像虚拟机一样重新加载一个操作系统内核，避免引导



### Docker命令

#### 帮助命令

```
docker version # 显示docker的基本信息
docker info # 系统信息，镜像和容器的数量
docker 命令 --help # 全部信息
```

[官网文档](https://docs.docker.com/reference/)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111857.png)



#### 镜像命令

##### docker images

查看所有本地主机上的镜像

```bash
[root@192 ~]# docker images

```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111912.png)

```
# 解释
REPOSITORY  # 镜像仓库源
TAG                 # 镜像的标签
IMAGE ID           # 镜像的ID
CREATED           # 镜像的创建时间
SIZE # 镜像的大小
```

```bash
--all , -a		Show all images (default hides intermediate images) # 显示所有
--digests		Show digests
--filter , -f		Filter output based on conditions provided
--format		Pretty-print images using a Go template
--no-trunc		Don’t truncate output
--quiet , -q		Only show numeric IDs # 只显示id
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111929.png)

##### docker search

搜索仓库中的镜像，相当于网页搜索

[网页搜索](http://hub.docker.com)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111944.png)

mysql

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601111958.png)

```
docker search mysql
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112012.png)

```
docker search --help
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112030.png)

```bash
# 解释
Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output

```

```
docker search mysql --filter=STARS=3000 # 搜索出Stars大于3000的
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112053.png)


##### docker pull

下载镜像

```bash
docker pull mysql # 下载mysql镜像，default tag，默认最新版latest
```

```bash
[root@192 ~]# sudo systemctl daemon-reload
[root@192 ~]# sudo systemctl restart docker
[root@192 ~]# docker pull mysql
Using default tag: latest # 不写tag默认最新版
latest: Pulling from library/mysql
8559a31e96f4: Pull complete  # layer 分层下载，docker image的核心 联合文件系统
d51ce1c2e575: Pull complete 
c2344adc4858: Pull complete 
fcf3ceff18fc: Pull complete 
16da0c38dc5b: Pull complete 
b905d1797e97: Pull complete 
4b50d1c6b05c: Pull complete 
c75914a65ca2: Pull complete 
1ae8042bdd09: Pull complete 
453ac13c00a3: Pull complete 
9e680cd72f08: Pull complete 
a6b5dc864b6c: Pull complete 
Digest: sha256:8b7b328a7ff6de46ef96bcf83af048cb00a1c86282bfca0cb119c84568b4caf6#签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest # 真实地址

# 即
docker pull mysql
# 等价于
docker pull docker.io/library/mysql:latest

```

```bash
# 指定版本下载 
docker pull mysql:5.7
```

版本来自于官网，版本库https://hub.docker.com/_/mysql

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112136.png)



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112149.png)

```
docker images
```

此时查看镜像，可以看到新下载的两个

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112208.png)

##### docker rmi

remove images

```bash
# 删除一个 可以通过名称 也可以指定id -f表示删除所有
docker rmi -f 9cfcce23593a
# 删除多个 用空格分隔id
docker rmi -f id id id
# 删除所有 
docker rmi -f $(docker images -aq) # images -aq就是查所有镜像id，从而递归删除
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112237.png)



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112253.png)

#### 容器命令

说明：有了镜像才能创建容器，linux，下载一个centos镜像来测试学习

```bash
docker pull centos
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112310.png)

##### 新建容器并启动

```shell
docker run [可选参数] image

# 参数说明
--name=“Name” # 容器名字，用于区分容器
-d 后台方式运行
-it 使用交互方式运行，进入容器查看内容
-p 指定容器的端口 如-p 8080::8080
	-p ip:主机端口：容器端口
	-p 主机端口:容器端口
	-p 容器端口
	
-p 随机指定端口
```

##### 进入退出容器

```bash
# 进入
docker run -it centos /bin/bash 
# 查看目录
ls
# 退出
exit
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112327.png)

注意**这里面主机名，变成了centos的容器id**

这里面就是一个容器，套娃啊

##### 查看运行的容器

```
# 查看正在运行的容器
docker ps
# 查看曾经运行的容器
docker ps -a
# 显示最近创建的容器，设置显示个数
docker ps -a - n=? 
# 只显示容器的编号
docker ps -aq
```

```shell
[root@192 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@192 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
9939864fa2e6        centos              "bin/bash"          4 minutes ago       Exited (0) 4 minutes ago                       unruffled_knuth
5f42e9930435        centos              "/bin/bash"         8 minutes ago       Exited (0) 4 minutes ago                       lucid_cannon
a89ddb393d3d        bf756fb1ae65        "/hello"            19 hours ago        Exited (0) 19 hours ago                        gracious_bhabha
[root@192 ~]# docker ps -a -n=2
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
9939864fa2e6        centos              "bin/bash"          6 minutes ago       Exited (0) 6 minutes ago                       unruffled_knuth
5f42e9930435        centos              "/bin/bash"         10 minutes ago      Exited (0) 7 minutes ago 
[root@192 ~]# docker ps -aq
9939864fa2e6
5f42e9930435
a89ddb393d3d

```

##### 退出容器

```shell
# 容器停止退出
exit
# 容器不停止退出 注意必须在英文输入法下，中文输入法不行
Ctrl + P + Q
```

```shell
[root@192 ~]# docker run -it centos /bin/bash
[root@bfcea13c40cd /]# [root@192 ~]# docker ps ##注意这里会自动给个命令，删掉
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
bfcea13c40cd        centos              "/bin/bash"         About a minute ago   Up About a minute                       stoic_wilson
edbd9366d959        centos              "/bin/bash"         7 minutes ago        Up 7 minutes                            affectionate_bartik
[root@192 ~]# docker exec -it edbd9366d959 /bin/bash ## 再次进入
[root@edbd9366d959 /]# exit ##停止并推出
exit

```

##### 删除容器

```shell
# 删除指定容器 不能删除正在运行的容器，如果强制删除 rm -f
docker rm 容器id
# 删除所有容器
docker rm -f $(docker ps -aq)
# 删除所有容器
docker ps -a -q|xargs docker rm
```

```shell
[root@192 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
bfcea13c40cd        centos              "/bin/bash"         29 minutes ago      Up 29 minutes                                   stoic_wilson
edbd9366d959        centos              "/bin/bash"         35 minutes ago      Up 35 minutes                                   affectionate_bartik
9939864fa2e6        centos              "bin/bash"          48 minutes ago      Exited (0) 48 minutes ago                       unruffled_knuth
5f42e9930435        centos              "/bin/bash"         52 minutes ago      Exited (0) 49 minutes ago                       lucid_cannon
a89ddb393d3d        bf756fb1ae65        "/hello"            20 hours ago        Exited (0) 20 hours ago                         gracious_bhabha
[root@192 ~]# docker rm 5f42e9930435
5f42e9930435
[root@192 ~]# docker rm edbd9366d959      # 注意正在运行的容器不能删除
Error response from daemon: You cannot remove a running container edbd9366d9596c744dd449119269b04de2f2a494e7fc471f6396bcefd94c33fe. Stop the container before attempting removal or force remove

```

```shell
[root@192 ~]# docker ps -aq # 所有容器id
bfcea13c40cd
edbd9366d959
9939864fa2e6
a89ddb393d3d
[root@192 ~]# docker rm -f $(docker ps -aq) # 全部删除
bfcea13c40cd
edbd9366d959
9939864fa2e6
a89ddb393d3d

```

##### 启动和停止容器的操作

```shell
docker start
docker restart
docker stop
docker kill
```

```shell
[root@192 ~]# docker run -it centos /bin/bash
[root@7b1a7dd10ea4 /]# exit
exit
[root@192 ~]# docker ps #查看正在运行的
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@192 ~]# docker ps -a # 查看历史运行过的
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
7b1a7dd10ea4        centos              "/bin/bash"         54 seconds ago      Exited (0) 42 seconds ago                       fervent_mirzakhani
[root@192 ~]# docker start 7b1a7dd10ea4 # 启动当前这个容器 container id 粘过 来
7b1a7dd10ea4
[root@192 ~]# docker ps # 查看当前运行容器 发现启动成功
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
7b1a7dd10ea4        centos              "/bin/bash"         2 minutes ago       Up 28 seconds                           fervent_mirzakhani
[root@192 ~]# docker stop 7b1a7dd10ea4 # 停止运行
7b1a7dd10ea4
[root@192 ~]# docker ps # 再次查看 没有这个容器了
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES


```

#### 常用其他命令

##### 后台启动docker

```shell
docker run -d 镜像名
# 用docker ps 查看的时候 发现停止了

# 后台运行，docker发现前台没有，容器启动后，发现自己没有提供服务，会立刻停止
```

```shell
Last login: Wed Jun 17 19:47:35 2020
[root@192 ~]# systemctl start docker # 关机后重启了，需要启动docker
[root@192 ~]# docker run -d centos # 运行
8ce188e5fee31c2fac93c0a405ee1a95c38dbc50cb47c35b19c0039c27558ded
[root@192 ~]# docker ps -a # 查看正在运行的
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
8ce188e5fee3        centos              "/bin/bash"         19 seconds ago      Exited (0) 18 seconds ago                       tender_dirac
7b1a7dd10ea4        centos              "/bin/bash"         8 hours ago         Exited (0) 8 hours ago                          fervent_mirzakhani

```



##### 查看日志

```shell
docker logs

# -tf          #显示日志
#--tail number # 要显示日志条数
docker logs -f -t --tail n 【id】

```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112415.png)



```shell
[root@192 ~]# docker logs --help

Usage:	docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g.
                       2013-01-02T13:23:37) or relative
                       (e.g. 42m for 42 minutes)
      --tail string    Number of lines to show from the
                       end of the logs (default "all")
  -t, --timestamps     Show timestamps # 时间戳
      --until string   Show logs before a timestamp (e.g.
                       2013-01-02T13:23:37) or relative
                       (e.g. 42m for 42 minutes)

```

```shell
# 自己编写的一个脚本
whiel true;do echo shenzai;sleep 
```



```shell
# 运行一个
[root@192 ~]# docker run -it centos /bin/bash

[root@c2887d35c71d /]# [root@192 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c2887d35c71d        centos              "/bin/bash"         57 seconds ago      Up 56 seconds                           vigorous_kare

# 查看日志，由于没有运行脚本，所以啥也没显示
[root@192 ~]# docker logs -f -t --tail 10 c2887d35c71d
^C # ctrl+c退出

# 运行centos里面加个脚本
[root@192 ~]# docker run -d centos /bin/sh -c "while true;do echo shenzai;sleep 1;done"
cb6d7fbc3f27a064137d58282de97b97365dea2705211ebfbad642079cc1b388

[root@192 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
cb6d7fbc3f27        centos              "/bin/sh -c 'while t…"   7 seconds ago       Up 6 seconds                            dreamy_almeida
c2887d35c71d        centos              "/bin/bash"              3 minutes ago       Up 3 minutes                            vigorous_kare

# 查看日志 发现隔一秒打印一条
[root@192 ~]# docker logs -f -t --tail 10 cb6d7fbc3f27
2020-06-17T12:02:11.293765084Z shenzai
2020-06-17T12:02:12.297675608Z shenzai
2020-06-17T12:02:13.301845582Z shenzai
2020-06-17T12:02:14.304800996Z shenzai
2020-06-17T12:02:15.307130238Z shenzai
2020-06-17T12:02:16.310574235Z shenzai
2020-06-17T12:02:17.312946923Z shenzai
2020-06-17T12:02:18.314841295Z shenzai
2020-06-17T12:02:19.317021705Z shenzai
2020-06-17T12:02:20.319670013Z shenzai
2020-06-17T12:02:21.322651649Z shenzai
2020-06-17T12:02:22.325466918Z shenzai
2020-06-17T12:02:23.327984704Z shenzai
2020-06-17T12:02:24.329656919Z shenzai

```



##### 查看容器中进程信息

~~~shell
# 命令 docker top 容器id
# UID：当前用户id PID：父id   PPID：进程ID

UID                 PID                 PPID                C                   STIME     
root                1424                31183               0                   15:13              
root                31183               31163               0                   14:50     
~~~





##### 查看正在镜像的元数据

```shell
# 命令
docker inspect 容器id

[root@192 ~]# docker inspect cb6d7fbc3f27
[
    {
        # 容器的完整id
        "Id": "cb6d7fbc3f27a064137d58282de97b97365dea2705211ebfbad642079cc1b388",
        
        # 创建时间
        "Created": "2020-06-17T12:00:50.706906186Z",
        
        # 脚本位置
        "Path": "/bin/sh",
        
        # 运行的脚本
        "Args": [
            "-c",
            "while true;do echo shenzai;sleep 1;done"
        ],
        "State": {
            "Status": "running", # 状态，正在运行
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1909, # 父进程id
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-06-17T12:00:51.093617477Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        
        # 镜像来源 
        "Image": "sha256:831691599b88ad6cc2a4abbd0e89661a121aff14cfa289ad840fd3946f274f1f",
        "ResolvConfPath": "/var/lib/docker/containers/cb6d7fbc3f27a064137d58282de97b97365dea2705211ebfbad642079cc1b388/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/cb6d7fbc3f27a064137d58282de97b97365dea2705211ebfbad642079cc1b388/hostname",
        "HostsPath": "/var/lib/docker/containers/cb6d7fbc3f27a064137d58282de97b97365dea2705211ebfbad642079cc1b388/hosts",
        "LogPath": "/var/lib/docker/containers/cb6d7fbc3f27a064137d58282de97b97365dea2705211ebfbad642079cc1b388/cb6d7fbc3f27a064137d58282de97b97365dea2705211ebfbad642079cc1b388-json.log",
        
        
        "Name": "/dreamy_almeida",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        
        # 主机配置
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        
        # 其他配置
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/3675586ebbd79cd72d2562a90c9380627a331c563724c0dac091f92600af4907-init/diff:/var/lib/docker/overlay2/7f79322e0f58d651a84a555dadd83d92537788172525945d3f538dd95dce336c/diff",
                "MergedDir": "/var/lib/docker/overlay2/3675586ebbd79cd72d2562a90c9380627a331c563724c0dac091f92600af4907/merged",
                "UpperDir": "/var/lib/docker/overlay2/3675586ebbd79cd72d2562a90c9380627a331c563724c0dac091f92600af4907/diff",
                "WorkDir": "/var/lib/docker/overlay2/3675586ebbd79cd72d2562a90c9380627a331c563724c0dac091f92600af4907/work"
            },
            "Name": "overlay2"
        },
        
        "Mounts": [], # 挂载
        
        # 基本配置
        "Config": {
            "Hostname": "cb6d7fbc3f27",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ], # 基本环境变量，这里没有Java
            
            # 基本命令
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo shenzai;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200611",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        
        # 网卡，比如现在用的是桥接的网卡
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "4d701985d7e77aa153790b697b2f38a61e20555c224b7675e4bf650b82799882",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/4d701985d7e7",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "8a6c71e2bafb19ca7dfd85445ccc4bef6d17467360a243d624089e676a24a018",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.3",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:03",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "22b0fd2290ccbc4e066a75d3f01bd8bf32ee4352c5bbcfc9f911287219219571",
                    "EndpointID": "8a6c71e2bafb19ca7dfd85445ccc4bef6d17467360a243d624089e676a24a018",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:03",
                    "DriverOpts": null
                }
            }
        }
    }
]

```



```shell
# 停止正在疯狂输出的那个容器
[root@192 ~]# docker stop cb6d7fbc3f27
cb6d7fbc3f27
```

##### 进入当前正在运行的容器

```shell
# 我们通常容器都是使用后台方式运行的e

docker exec -it 容器id bashSHELL

# 测试
[root@192 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c2887d35c71d        centos              "/bin/bash"         35 minutes ago      Up 35 minutes                           vigorous_kare
[root@192 ~]# docker exec -it c2887d35c71d /bin/bash
[root@c2887d35c71d /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@c2887d35c71d /]# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 11:57 pts/0    00:00:00 /bin/bash
root         14      0  0 12:32 pts/1    00:00:00 /bin/bash
root         28     14  0 12:32 pts/1    00:00:00 ps -ef
[root@c2887d35c71d /]# c2887d35c71d
[root@c2887d35c71d /]# exit
exit

# 方式二
[root@192 ~]# docker attach c2887d35c71d
[root@c2887d35c71d /]# 

# 区别
# docker exec # 进入容器后开启一个新的终端，可以在里面操作(常用)
# docker attach 进入容器正在执行的终端，不会启动新的进程

```



##### 从容器内拷贝文件到主机上

```shell
docker cp 容器id:容器内路径  目的的主机路径

# 运行
[root@192 ~]# docker run -it centos
# ctrl P Q 不关闭退出，查看
[root@0569081aa89c /]# [root@192 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
0569081aa89c        centos              "/bin/bash"         19 seconds ago      Up 19 seconds    
hopeful_chebyshev

# 查看主机home下无文件
[root@192 ~]# cd /home
[root@192 home]# ls

# 进入正在运行的容器
[root@192 home]# docker attach 0569081aa89c

# 进入容器home目录
[root@0569081aa89c /]# cd /home

# 在容器目录中创建java文件
[root@0569081aa89c home]# touch test.java

# 退出并停止容器
[root@0569081aa89c home]# exit
exit

# 查看现在运行的容器
[root@192 home]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

# 容器虽然被停止，但是数据都会保留
[root@192 home]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                         PORTS               NAMES
0569081aa89c        centos              "/bin/bash"              3 minutes ago       Exited (0) 8 seconds ago                           hopeful_chebyshev
f589e5684a01        centos              "/bin/bash"              44 minutes ago      Exited (0) 44 minutes ago                          cranky_easley
cb6d7fbc3f27        centos              "/bin/sh -c 'while t…"   54 minutes ago      Exited (137) 42 minutes ago                        dreamy_almeida
c2887d35c71d        centos              "/bin/bash"              58 minutes ago      Exited (127) 16 minutes ago                        vigorous_kare
8ce188e5fee3        centos              "/bin/bash"              About an hour ago   Exited (0) About an hour ago                       tender_dirac
7b1a7dd10ea4        centos              "/bin/bash"              9 hours ago         Exited (0) 9 hours ago                             fervent_mirzakhani

# 将容器数据拷贝到主机上
[root@192 home]# docker cp 0569081aa89c:/home/test.java /home
[root@192 home]# ls
test.java

# 拷贝是一个手动过程，未来我们使用 -v 卷的技术，可以实现自动同步 /home /home
```

##### 查看内容占用

```shell
docker stats
```





##### 小结

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601112511.png)

```shell
[root@192 home]# docker --help

Usage:	docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config
                           files (default "/root/.docker")
  -c, --context string     Name of the context to use to
                           connect to the daemon
                           (overrides DOCKER_HOST env var
                           and default context set with
                           "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level
                           ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by
                           this CA (default
                           "/root/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file
                           (default "/root/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default
                           "/root/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  engine      Manage the docker engine
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

```



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113054.png)



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113124.png)



##### 作业练习

###### 部署Nginx

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113145.png)

```shell
# 官网搜索nginx，可以看到帮助文档

# 下载镜像
[root@192 home]# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
8559a31e96f4: Pull complete 
8d69e59170f7: Pull complete 
3f9f1ec1d262: Pull complete 
d1f5ff4f210d: Pull complete 
1e22bfa8652e: Pull complete 
Digest: sha256:21f32f6c08406306d822a0e6e8b7dc81f53f336570e852e25fbe1e3e3d0d0133
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest

# 查看镜像
[root@192 home]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              831691599b88        13 hours ago        215MB
nginx               latest              2622e6cca7eb        7 days ago          132MB

# 运行测试
# -d 后台运行，--name 命名，-p 暴露端口，3344服务器、宿主机的端口，容器内部端口
[root@192 home]# docker run -d --name nginx01 -p:3344:80 nginx
38dbf7bdcaef232d269b7184d91e44e06087181b5ee929494e177ad526810fa8
[root@192 home]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
38dbf7bdcaef        nginx               "/docker-entrypoint.…"   7 seconds ago       Up 6 seconds        0.0.0.0:3344->80/tcp   nginx01

# 使用3344可以访问成功
[root@192 home]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

端口暴露

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113202.png)

可以公网访问

找到服务器地址

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113216.png)

浏览器输入 192.168.147.132:3344/

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113233.png)

```shell
[root@192 home]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
38dbf7bdcaef        nginx               "/docker-entrypoint.…"   21 minutes ago      Up 21 minutes       0.0.0.0:3344->80/tcp   nginx01

# 进入容器
[root@192 home]# docker exec -it nginx01 /bin/bash

# 查一下nginx在哪
root@38dbf7bdcaef:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx

# 到这个目录
root@38dbf7bdcaef:/# cd /etc/nginx
root@38dbf7bdcaef:/etc/nginx# ls
conf.d		koi-utf  mime.types  nginx.conf   uwsgi_params
fastcgi_params	koi-win  modules     scgi_params  win-utf

# 退出
root@38dbf7bdcaef:/etc/nginx# exit
exit

# 停止
[root@192 home]# docker stop 38dbf7bdcaef
38dbf7bdcaef


```



再次刷新网页，服务关闭

思考问题：每次改动nginx配置文件，都需要进入容器内部，十分麻烦，要是可以在容器外部提供一个映射路径，达到在容器外修改文件名，容器内部就可以自动修改？-v 数据卷技术！

###### 部署tomcat

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113252.png)



在docker hub上查看版本号和使用方法

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113304.png)

官方文档一定要翻烂，超多版本，我的天呐~

###### 官方方法

```shell
docker run -it --rm tomcat:9.0

# docker run 可以不用pull，能自动下载
# -it 直接进去运行
# --rm 是什么意思？入门的意思？
# 我们之前的启动都是后台，停止了容器之后，容器还是可以查到
# 写了--rm，类似阅后即焚模式，用完即删除，这种通常用来测试

# 最后冒号查好的版本号
```



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113323.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113336.png)

ctrl+c退出

```shell
docker ps -a
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113351.png)

	可以看到并没有tomcat，印证阅后即焚模式，容器会删除，镜像不会删除

平时不建议这样搞

###### 正常方法

```shell
docker pull tomcat:9.0 # 之前下过了，应该不用下了，这里老师讲错了
```



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113408.png)

```shell
# 启动运行，应该加上版本号
docker run -d -p 3355:8080 --name tomcat01 tomcat
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113425.png)



```shell
# 进入容器
docker exec -it tomcat01 /bin/bash
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113438.png)

发现问题

1. linux命令少了
2. 没有webapps

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113452.png)

这是阿里云镜像的原因：默认使用最小镜像，所有不必要的都剔除了，保证最小可运行环境

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113506.png)

再次找到结构

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113523.png)

在浏览器中输入：http://192.168.147.132:3355/

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113537.png)

思考问题：

	我们以后部署项目，如果每次都要进入容器是不是身份麻烦？我要是可以在容器外部提供一个映射路径，
	webapps，我们在外部放置项目，就自动同步到内部就好了！


###### 部署es+kibana

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113548.png)



```shell
# es 暴露的端口很多
# es 十分耗内存
# es 的数据一般需要放置到安全目录！挂载

# 启动 elasticsearch
$ docker run -d --name elasticsearch01 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2

# 查看内存占用情况
docker stats
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113612.png)

```shell
# 先感觉stop一下
docker stop ba18713ca536
```



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113624.png)



```shell
# 通过 -e 限制内存
$ docker run -d --name elasticsearch02 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS=“-Xms64m -Xmx512m” elasticsearch:7.6.2
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113637.png)

没成功啊，谁能告诉我为啥！！

```shell
docker run -d --name elasticsearch02 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
```

>py0003null大佬:
>“ES_JAVA_OPTS=-Xms64m -Xmx512m” 
>
>py0003null大佬:
>引号提前试试

原因是引号！！你没觉得怪怪的嘛

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113657.png)

此时查看stats，发现内存占用在控制范围内

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113707.png)

ctrl + C退出，记得stop

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113724.png)

###### 思考：用kibana链接elasticsearch

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113737.png)



#### 可视化

- portainer(先用这个)
- Rancher(CI/CD时用)

##### portainer

Docker图像化界面管理工具，提供一个后台面板供我们操作！

```shell
docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker --privileged=true portainer/portainer
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113751.png)

访问外网8088

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113804.png)

用户名

密码 

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113836.png)

【中午吃饭重连了一下，失败，不知道发生了啥】

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113905.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113918.png)

【平时不会用这个，好吧，先往下】


### Docker镜像

#### 原理

UnionFS 联合文件系统

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113935.png)

bootfs：boot file system

rootfs：root file system

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601113950.png)





![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114009.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114023.png)



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114037.png)

Docker镜像都是只读的，当容器启动时，一个新的可写层被加到镜像的顶部，这一层就是我们通常说的容器层，容器层之下的都叫镜像层

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114057.png)



#### commit提交镜像

```shell
docker commit # 提交容器成为一个新的副本
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名：[TAG]
```

```shell
docker images
docker run -it -p 8080:8080 tomcat
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114113.png)

这是一个前台程序

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114126.png)

将webapps.dist里面所有的文件拷贝到webapps里面，其中-r必须有，表示目录递归拷贝

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114136.png)



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114147.png)

```shell
docker commit -a="paidaxing" -m="add webapps app" 当前容器的id tomcat02:1.0
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114204.png)

发现新的版本，比之前的大了一些，因为里面记录了我们的改动

> 如果想保存当前容器的状态，可以通过commit提交，获得一个镜像
>
> 好比我们以前学习VM的时候的快照
>
> 到这里算是入门了
>
> 接下来三个部分是docker的精髓



### 容器数据卷

#### 什么是容器卷

docker是要将应用和环境打包成一个镜像，这样，数据就不应该在容器中，否则容

器删除，数据就会丢失，这就是删库跑路，故容器之间要有一个数据共享技术

在Docker容器中产生的数据，同步到本地，这就是卷技术，本质上是一个目录挂

载，将容器内的目录挂载到虚拟机上

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114219.png)

目的：

- 容器的持久化和同步操作

- 容器间可以数据共享

#### 使用数据卷

方式一：直接使用命令来挂载

```shell
docker run -it -v -p
# -it 交互式进入
# -v volume卷技术
# -p 主机端口
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114233.png)

新开一个窗口

```shell
docker inspect 容器id
```



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114254.png)

找到挂载信息Mounts

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114353.png)

测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114408.png)

容器停止后，修改主机文件，再启动容器的时候，数据同样改变

双向同步

#### 实战安装mysql

MySQL的数据持久化命令

```shell
docker search mysql

# 拉取
docker pull mysql:5.7

# 挂载
docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=xn123456 --name mysql mysql:5.7

-d 后台运行
-p 端口映射
-v 卷挂载
-e 环境配置 安装启动mysql需要配置密码
--name 容器名字

```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114432.png)

链接测试：打开SQLyog

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114446.png)

点 测试链接

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114457.png)

点 链接

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114509.png)

#### 具名和匿名挂载

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114529.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114550.png)



####  使用DockerFile来构建docker镜像的文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114743.png)

> kas:
> 没有安装吧？
>
> kas:
> 安装一下就可以了
>
> py9001021曾叙坚:
> centos 默认使用vi写内容，vim需要下载
>
> py9001021曾叙坚:
> 是的，需要下载vim才能使用
>
> YF:
> 配置可以改下，字体就很炫酷了
>
> 都建民:
> 用vi  试试
>
> 都建民:
> 你没有安装vim
>
> 都建民:
> yum install vim
>
> 都建民:
> 试试这个命令

```shell
yum install vim # 编辑文件的，没有装一下
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114758.png)

安装完之后就可以运行这个命令了

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601114810.png)

```shell
# 镜像是一层一层的，脚本是一行一行的
# 指令都是大写的
# 这里的每个命令可以理解为镜像的一层

FROM centos

VOLUME ["volume01","volume02"] # 再创建镜像的时候就挂载出来

CMD echo "---end---"
CMD /bin/bash
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115003.png)

想保存并退出

>Wesley.:
>shift  加  冒号



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115016.png)

```shell
cat dockerfile1

docker build -f dockerfile1 -t padaxing/centos:1.0 .  # 最后的点很重要 镜像名前面不能有/
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115030.png)

```shell
docker images
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115045.png)

启动生成的镜像

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115101.png)

在容器内部创建一个文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115117.png)

查看Mounts，Source对应容器外目录，匿名挂载卷

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115131.png)

测试一下，在container volume01下生成文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115145.png)

在主机挂载路径下，也同样生成

#### 多个容器数据共享

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115200.png)

看一下有啥images

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115217.png)

启动docker01，用之前建的padaxing/centos 1.0  镜像

```shell
docker run -it --name docker01 padaxing/centos:1.0 # 1.0必须写
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115230.png)



当前这个ctrl+p+q不停止退出

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115241.png)

依次启动docker02、docker03

```shell
docker run -it --name docker02 --volumes-from docker01 padaxing/centos:1.0
```

docker02继承docker01的volumes

可以验证，在docker01下加一个数据，在docker02下也会出现

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115255.png)

创建docker03，同样继承docker01

```shell
docker run -it --name docker03 --volumes-from docker01 padaxing/centos:1.0
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115307.png)

在docker03的volume01下建立文件，在docker01的volume01下同样也有

**即通过--volumes-from 可以实现不同容器间的数据共享**

删除docker01，数据还在

```shell
docker rm -f 
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115321.png)

可以看到，删除docker01，进入docker02，数据依然在

结论：

	容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用位置

	但是如果持久化到了本地，即使所有容器删除了，本地数据是不会删除的

------

### DockerFile

是用来构建docker镜像的文件，可以理解为命令参数脚本

构建步骤：

1. 编写一个dockerfile文件
2. docker build 构建成为一个镜像
3. docker run运行镜像
4. docker push 发布镜像（DockerHub、阿里云镜像仓库 私有/共有）

这个写一个项目时一样的

#### 官方DockerFile示例

看一下官方的DockerFile

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115349.png)

可以看到官方镜像都是基础包，很多功能没有，我们通常会自己搭建自己的镜像

官方既然可以制作镜像，我们亦可以

-----

#### DockerFile基础知识

1. 每个指令都必须是大写字母
2. 按照从上到下顺序执行
3. **#** 表示注释
4. 每一个指令都会创建体检一个新的镜像层，并提交

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115407.png)

docker是面向开发的，我们以后要发布项目，做镜像，就要编写dockerfile文件，这个文件十分简单！

Docker镜像逐渐成为企业的交付标准，必须掌握！

---

#### DockerFile命令

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115428.png)

#### DockerFile

```shell
FROM          # 基础镜像 比如centos
MAINTAINER    # 镜像是谁写的 姓名+邮箱
RUN           # 镜像构建时需要运行的命令
ADD           # 添加，比如添加一个tomcat压缩包
WORKDIR       # 镜像的工作目录
VOLUME        # 挂载的目录
EXPOSE        # 指定暴露端口，跟-p一个道理
RUN           # 最终要运行的
CMD           # 指定这个容器启动的时候要运行的命令，只有最后一个会生效，而且可被替代
ENTRYPOINT    # 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD       # 当构建一个被继承Dockerfile 这个时候运行ONBUILD指定，触发指令
COPY          # 将文件拷贝到镜像中
ENV           # 构建的时候设置环境变量
```

#### 实战构建自己的centos

Docker Hub中99%的镜像都是从FROM scratch开始的

添加centos7的压缩包

```shell
# 创建一个自己的centos

# 进入home目录
cd /home

# 创建一个目录，之后的东西都保存到这里
mkdir dockerfile
# 进入这个目录
cd dockerfile/
# 创建一个dockerfile，名字叫mydockerfile
vim mydockerfile-centos


```

xshell新开一个界面

```shell
# 官方默认centos
docker run -it centos
pwd # 官方默认有pwd命令
vim # 官方默认没有vim命令
ifconfig # 官方默认没有ifconfig命令
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115458.png)

回到mydockerfile

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115509.png)

```shell
# 下面给官方centos加上自定义的内容
FROM centos
MAINTAINER padaxing<010301200@hai.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115524.png)

ESC, shif + : 输入wq保存并退出

如果写错了需要修改、

```shell
vim mydockerfile-centos
# 进入之后按i或者INSERT键即可修改
```



下面通过这个这个文件创建镜像

```shell
docker build -f dockerfile-centos -t mycentos:0.1 .
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115541.png)

依次执行命令

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115553.png)

最终返回Successfully表示成功

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115604.png)

```shell
docker run -it mycentos:0.1 # 版本号必须写，不然他会去找最新的
pwd
vim
ifconfig
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115616.png)

这时可以看到这些功能都有了

可以通过查看docker构建历史

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115628.png)

可以看到当前这个镜像是怎么一步一步构建起来的

我们平时拿到一个镜像也可以通过这个方法研究一下他是怎么做的

#### CMD与ENTRYPOINT

```shell
FROM centos
CMD ["ls","-a"] # 启动centos展示目录
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115643.png)

**测试ENTRYPOINT**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115655.png)

**run的时候可以直接加命令**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115706.png)

Docker中许多命令都十分相似，我们需要了解他们的区别，最好的方式就是这样对比测试

区别：

	CMD是直接产生了覆盖现象，想要在后面追加命令则需要输入被覆盖的命令加上要追加的命令；而entrypoint可以直接在后面追加命令不会产生覆盖现象

---

#### 实战Tomcat镜像

1. 准备镜像文件 tomcat压缩包，jdk压缩包

   [jdk压缩包](https://blog.csdn.net/doupeihua/article/details/51831947)

   [tomcat压缩包](https://tomcat.apache.org/download-90.cgi)

2. 编写dockerflie文件，官方命名**Dockerfile**，build会自动寻找这个文件，就不需要-f指定了

~~~shell
FROM centos

MAINTAINER wei<1186419584@qq.com>

COPY readme.txt /usr/local/readme.txt          #将文件复制到容器里面的路径
ADD jdk-8u11-linux-x64.tar.gz /usr/local/		#将压缩包添加到容器中（使用add自动会帮你解压）
ADD apache-tomcat-9.0.41.tar.gz /usr/local/

RUN yum -y install vim					#安装vim命令	
ENV MYPATH /usr/local					#进去后就进入这个路径
WORKDIR $MYPATH							#工作目录

ENV JAVA_HOME /usr/local/jdk1.8.0_11 	#配置Java环境变量（Java安装目录）

ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar #Java配置

ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.41 		#配置tomcat环境变量（tomcat默认的安装目录）

ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.41		#tomcat配置

ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin		#将这些目录放入总的path中，$CATALINA_HOME/lib：tomcat的依赖，$CATALINA_HOME/bin启动tomcat就不需要进入bin目录了

EXPOSE 8080  #暴露端口

CMD /usr/local/apache-tomcat-9.0.41/bin/startup.sh && tail -F /url/local/apache-tomcat-9.0.41/bin/logs/catalina.out #执行tomcat后面是做一些参数，展示日志信息 
~~~



3.运行镜像

~~~shell
docker build -t 镜像名 .
~~~

4.启动镜像

~~~shell
docker run -d -p 3345:8080 --name weitomcat02 -v /home/wei/build/tomcat/test:/usr/local/apache-tomcat-9.0.41/webapps/test -v /home/wei/build/tomcat/tomcatlogs/:/usr/local/apache-tomcat-9.0.41/logs divtomcat
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115726.png)



5.访问测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115737.png)



6.发布项目（由于做了卷挂载，我们直接在本地编写就可以发布了！）

**web.xml文件**

~~~shell
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4" 
    xmlns="http://java.sun.com/xml/ns/j2ee" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
        http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
</web-app>
~~~



**index.jsp文件**

~~~jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>hello,wei</title>
</head>
<body>
Hello World!<br/>
<%
System.out.println("-------my test web logs-----------");
%>
</body>
</html
~~~



#### 发布自己的镜像

1、地址https://hub.docker.com/ 注册自己的账号

2、确定这个账号可以登录

3、在我们的服务器上提交自己的镜像

~~~shell
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker login --help

Usage:  docker login [OPTIONS] [SERVER]

Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker login -u cloverfelix
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[root@iZ2zejeormv0s24sdvazvxZ ~]# 
~~~



4、登录完毕后就可以提交镜像了，就是一步 docker push

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115803.png)

~~~shell
# push到自己的镜像到服务器上
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker push divtomcat
Using default tag: latest
The push refers to repository [docker.io/library/divtomcat]
b23ff398b23f: Preparing 
e0403d0b82a9: Preparing 
d252d00ff518: Preparing 
537a66dda3f7: Preparing 
2653d992f4ef: Preparing 
denied: requested access to the resource is denied # push请求被拒绝了
[root@iZ2zejeormv0s24sdvazvxZ ~]# 
~~~

**push上去的时候一定记得带上版本号**



#### 小结：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115820.png)



### Docker网络

#### 理解docker0

1、清空所有环境

测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115832.png)

三个网络

~~~shell
# 问题： docker是如何处理容器访问网络的？
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115848.png)



~~~shell
# [root@iZ2zejeormv0s24sdvazvxZ ~]# docker run -d -P --name tomcat01 tomcat  运行一个tomcat



# 查看容器的内部网络地址 ip addr， 发现容器启动的时候会得到一个eth0@if91 IP地址，docker分配的！
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
90: eth0@if91: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

# 思考：Linux能不能ping通容器内部？
[root@iZ2zejeormv0s24sdvazvxZ ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.085 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.070 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.067 ms
64 bytes from 172.17.0.2: icmp_seq=4 ttl=64 time=0.070 ms
64 bytes from 172.17.0.2: icmp_seq=5 ttl=64 time=0.066 ms
64 bytes from 172.17.0.2: icmp_seq=6 ttl=64 time=0.091 ms
64 bytes from 172.17.0.2: icmp_seq=7 ttl=64 time=0.070 ms
^C
--- 172.17.0.2 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 5999ms
rtt min/avg/max/mdev = 0.066/0.074/0.091/0.010 ms

# Linux可以ping通docker容器内部
~~~



原理

1、我们每启动一个docker容器，docker就会给docker容器分配一个IP，我们只要安装了docker，就会有一个网卡docker0，桥接模式，使用的技术是evth-pair技术

再次测试IP addr

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115904.png)



2、再启动一个容器测试，发现又多了一对网卡

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115915.png)



~~~shell
# 我们发现这个容器带来的网卡，都是一对对的
# evth-pair就是一对的虚拟设备接口，他们都是成对的出现，一端连接着协议，一端批次相连
# 正因为有这个特性，evth-pair 充当一个桥梁，连接各种虚拟网络设备的
# OpenStac，Docker容器之间的连接，OVS的连接，都是使用evth-pair 技术
~~~

3、我们来测试一下tomcat01 和 tomcat02是否可以ping通！

~~~shell
# [root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat02 ping 172.17.0.2

# 结论： 容器和容器之间是可以互相ping通的！
~~~

绘制一个网络模型图：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115929.png)

结论：

	tomcat01和tomcat02是共用一个路由器的，也就是我们的docker0

	所有的容器在不指定网络的情况下，都是docker0路由的，docker会给我们的容器

	分配一个默认的可用IP



小结：

 Docker使用的是Linux的桥接，宿主机中是一个docker容器的网桥 docker0

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115941.png)

Docker中的所有的网络接口都是虚拟的，虚拟的转发效率高！

只要容器删除，对应的网桥一对就没了

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601115952.png)







思考一个场景，我们编写了一个微服务，database url=ip:，项目不重启，数据库ip换掉了，我们希望可以解决这个问题，可以使用名字来进行访问容器？  这里使用的是`--link` 技术

~~~shell
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat02 ping tomcat01
ping: tomcat01: Name or service not known

# 如何解决呢？
# 通过--link 既可以解决了网络连通问题
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker run -d  -P --name tomcat03 --link tomcat02 tomcat
581a112c15d5f4de2634cb1027ff1d011a1c495b4e83a27ccd98784c17fce05f
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.126 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.102 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=3 ttl=64 time=0.106 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=4 ttl=64 time=0.096 ms

# 反向可以ping通嘛？
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat02 ping tomcat03
ping: tomcat03: Name or service not known
~~~



探究inspect：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601120009.png)

其实这个tomcat03就是在本地配置了tomcat02的配置

~~~shell
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat03 cat /etc/hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
# 172.17.0.3      tomcat02 0303648ab9d4
172.17.0.4      581a112c15d5
~~~



本质探究：`--link`就是我们在hosts配置中增加了一个tomcat02的映射。172.17.0.3      tomcat02 0303648ab9d4 



我们现在玩Docker已经不建议使用`--link`了！

使用的都是自定义网络！不使用docker0！

docker0问题：它不支持容器名连接访问！



#### 自定义网络

查看所有的docker网络

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601120024.png)



**网络模式**

	bridge： 桥接 就是在docker上搭桥（默认使用，自己创建也使用bridge）

	none： 不配置网络

	host： 和宿主机共享网络

	container： 容器网络连通！（用得少，局限很多）

**测试**

~~~shell
# 我们直接启动的命令 默认是有--net bridge，而这个就是我们的docker0
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker run -d -P --name tomcat01 --net bridge tomcat

# docker0特点： 默认，域名不能访问，--link可以打通连接！

# 我们可以自定义一个网络！
# --driver bridge 连接模式
# --subnet 192.168.0.0/16 子网地址
# --gateway 192.168.0.1 网关
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
f833096b830f85a24a3fe4d1f4e9e0cc9f5327bc8a0059ec9577d600fb54b111

[root@iZ2zejeormv0s24sdvazvxZ ~]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
e3ed9ad5b74b   bridge    bridge    local
51f1bd18d34e   host      host      local
f833096b830f   mynet     bridge    local
bb8aa0300ba6   none      null      local
~~~

我们自己的网络就创建好了

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601120038.png)

~~~shell
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker run -d -P --net mynet --name tomcat-net-01 tomcat
7904d3327a0c8c7f32e43c0611aeecf97f5b1ae9878957c3a09aae53c8c262bb
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker run -d -P --net mynet --name tomcat-net-02 tomcat
11795f8616ff8b5df0bc172fc4f67c68d30ead31442dfd812f1d09959ab0600c
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "f833096b830f85a24a3fe4d1f4e9e0cc9f5327bc8a0059ec9577d600fb54b111",
        "Created": "2021-02-03T00:17:44.277900481+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "11795f8616ff8b5df0bc172fc4f67c68d30ead31442dfd812f1d09959ab0600c": {
                "Name": "tomcat-net-02",
                "EndpointID": "1b542176401a508f905acbb09ab2d07c45c3280e6f60e66428a565766470b46e",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            },
            "7904d3327a0c8c7f32e43c0611aeecf97f5b1ae9878957c3a09aae53c8c262bb": {
                "Name": "tomcat-net-01",
                "EndpointID": "872b36e9c2a5d9f59a2931e1edad34c3eded6d693c3fef830e29bcdae511b4d9",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

# 再次测试ping连接
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat-net-01 ping 192.168.0.3
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=0.121 ms
64 bytes from 192.168.0.3: icmp_seq=2 ttl=64 time=0.096 ms
64 bytes from 192.168.0.3: icmp_seq=3 ttl=64 time=0.100 ms

--- 192.168.0.3 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.096/0.105/0.121/0.016 ms

#现在不使用--link也可以ping名字了！
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat-net-01 ping tomcat-net-02
PING tomcat-net-02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.069 ms
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.099 ms
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=3 ttl=64 time=0.103 ms
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=4 ttl=64 time=0.093 ms
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=5 ttl=64 time=0.100 ms

--- tomcat-net-02 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.069/0.092/0.103/0.017 ms
~~~

**结论：我们自定义的网络docker都已经帮我们维护好了对应的关系，推荐我们平时这样使用网络**



好处：

	redis - 不同的集群使用不同的网络，保证集群是安全和健康的

	mysql - 不同的集群使用不同的网络，保证集群是安全和健康的



#### 网络连通

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601120052.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601120103.png)



~~~shell
# 测试打通 tomcat01 到 mynet

# 连通之后就是将我们的 tomcat01 加到 mynet网络下

# 一个容器两个ip地址
#阿里云服务器： 一个公网 一个私网地址
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601120113.png)



~~~shell
# tomcat01 可以与 tomcat-net-01
连通
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat-net-01 ping 192.168.0.4
PING 192.168.0.4 (192.168.0.4) 56(84) bytes of data.
64 bytes from 192.168.0.4: icmp_seq=1 ttl=64 time=0.134 ms
64 bytes from 192.168.0.4: icmp_seq=2 ttl=64 time=0.095 ms
64 bytes from 192.168.0.4: icmp_seq=3 ttl=64 time=0.097 ms
64 bytes from 192.168.0.4: icmp_seq=4 ttl=64 time=0.096 ms
^C
--- 192.168.0.4 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.095/0.105/0.134/0.019 ms

# tomcat02 与 tomcat-net-01依旧打不通
[root@iZ2zejeormv0s24sdvazvxZ ~]# docker exec -it tomcat02 ping tomcat-net-01
ping: tomcat-net-01: Name or service not known
~~~

**结论：假设要跨网络操作别人，就需要使用docker network connect 连通！**



#### 实战：部署Redis集群





### IDEA整合Docker

### Docker 整合Docker

### Docker Compose

### Docker Swarm

### 各种bug

#### Xshell链接失败

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601120127.png)

```bash
Connecting to 192.168.147.131:22...
Could not connect to '192.168.147.131' (port 22): Connection failed.

```


注：电脑休眠重启后，机子会断开，要重新在vmware里面输入ip addr 获得ip

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601120136.png)

修改xshell中的主机接口即可

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601120148.png)


