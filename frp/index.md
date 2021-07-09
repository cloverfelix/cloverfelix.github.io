# Frp


# 配置FRP实现内网穿透

## 1、安装frp

**frp的作用**

- 利用处于内网或防火墙后的机器，对外网环境提供 http 或 https 服务。


- 对于 http, https 服务支持基于域名的虚拟主机，支持自定义域名绑定，使多个域名可以共用一个80端口。


- 利用处于内网或防火墙后的机器，对外网环境提供 tcp 和 udp 服务（ps：例如在家里通过 ssh 访问处于公司内网环境内的主机）


**说明**

1. 实现功能

	① 外网通过ssh访问内网机器（ps：主要验证实现此功能）
	
	② 自定义绑定域名访问内网web服务

2. 配置准备

	① 公网服务器1台（演示用机为CentOS系统）
	
	② 内网服务器1台（演示用机为CentOS系统）
	
	③ 公网服务器绑定域名1个（实现**1-①**功能不需要公网服务器绑定域名，**1-②**功能必须需要公网服务器绑定域名）
	
	④ 内网服务器部署一个web服务，可以用tomcat模拟，没有测试。


先去官网下载frp：[https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases)

下载完传到Linux服务器上，并且在`/etc/local`目录下创建frp文件夹，并且将frp解压至该文件下

~~~bash
mkdir /usr/local/frp
cd frp
tar -zxvf /root/frp_0.37.0_linux_amd64.tar.gz -C ./
~~~

进入解压目录cd frp_0.13.0_linux_amd64，这里主要关注4个文件，分别是`frpc`、`frpc.ini`和`frps`、

`frps.ini`，**前者两个文件是`客户端`**所关注文件，**后者两个文件是`服务端`**所关注两个文件。

配置服务端（`公网服务器`），首先删掉`frpc、frpc.ini`两个文件，然后再进行配置，`vi ./frps.ini`

~~~bash
[common]
    
bind_port = 7000 #与客户端绑定的进行通信的端口
    
vhost_http_port = 7081 #访问客户端web服务自定义的端口号
~~~

保存然后启动服务，这是前台启动，

~~~bash
./frps -c ./frps.ini
~~~

后台启动命令为

~~~bash
nohup ./frps -c ./frps.ini &
~~~

启动成功后的样子：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210706211521.png)

配置客户端（`内网服务器`），首先删掉`frps、frps.ini`两个文件,然后再进行配置，`vi ./frpc.ini`

~~~bash
[common]
server_addr = 192.168.167.227   # 公网服务器IP
server_port = 7000              		# 与服务端bind_port一致

# 公网通过ssh访问内部服务器
[ssh]
type = tcp                     				  # 连接协议
local_ip = 192.168.167.117          # 内网服务器ip
local_port = 22                			  # ssh默认端口号
remote_port = 7088              	# 自定义的访问内部ssh端口号

# 公网访问内部web服务器以http方式
[web]
type = http                     						# 访问协议
local_prot = 8081               					# 内部web服务的端口号
custom_domains = repo.clover.com     # 所绑定的公网服务器域名，一级、二级域名都可以
~~~

保存然后执行启动，这是前台启动，

~~~bash
./frpc -c ./frpc.ini
~~~

后台启动命令为

~~~bash
nohup ./frpc -c ./frpc.ini &
~~~

这种方法，我没有做成功，我换到使用隧道代理的方法上去了，以后有了计网的基础后，再来实践

这是隧道代理的信息

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210706221516.png)
