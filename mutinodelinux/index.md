# MutiNodeLinux




# 部署项目到服务器

## 使用的是ruoyi进行试验

## 第一步

首先将项目后端的数据库以及一些配置文件配置好

1. **数据库配置：**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702193507.png)

- 对于这里的数据库必须使用远程的数据库，本地数据库是不起作用的

- 其次就是需要在服务器上创建对应的数据库，填写相关的信息

- 如果你有多台服务器的话你也可以架设到不同的服务器上（但我受限只有一台服务器，就在一台服务器上实验了）

2. **修改redis配置**

由于我们的项目全部是部署在服务器上的，所以我们所依赖的环境全部是存在于服务器上

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702194801.png)

由于我们服务器上的redis是没有设置密码的，所以默认为空


3. **修改pom.xml文件中字符集的设置**

	`注意：由于我采用的是ruoyi的项目，不同的项目对应不同的要求，ruoyi这个项目需要你改动一下pom.xml的配置文件`
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702194011.png)
	
	将**encoder**对应的地方的字符集编码改为`UTF-8`
	
4.**修改pom.xml文件中的日志文件logs**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702194245.png)
   
如果是第一次运行的话是不存在改路径的，就需要改为自己日志文件存放的路径了
   
如果是以及运行过的，可以忽略不看


将一些环境配置修改好后就可以运行起来了，当你运行成功后，就可以先将前端打包传送到服务器上

使用terminus工具进行上传

	注意：不要使用文件夹拖拽的形式，有可能存在一些隐藏的文件夹没有传递过去
   
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702195046.png)

传到服务器上的home目录下自己创建的文件夹中，然后在进行文件的解压

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702195205.png)

上传成功后，就对该文件夹进行解压**（前端）**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702195339.png)

这就是解压后的文件

解压成功后，也把后端运行好的项目压缩上传到服务器

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702195446.png)

## 然后开始进行前端的打包操作

1. 安装依赖

由于我们使用的是root权限所以我们在前面加上`--unsafe-perm`

~~~bash
npm install --unsafe-perm --registry=https://registry.npm.taobao.org
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702203203.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702203220.png)

	注意：如果你没有安装node的话是执行不了的，那你就必须得先去安装node后才可以执行，Linux上安
	装node的教程可以参考codesheep给的pdf上安装node的教程，只是，他最后配置node环境变量时，那个profile文件夹是存在于/etc下的profile文件夹中进行配置，而不是他pdf上所给的指令，使用pdf上的指令会提示我们没有权限
	
安装完依赖后就可以使用`npm run build:prod`打生产环境前端的包就可以了

但是在运行的过程中出现了错误信息提示

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702205825.png)

于是，我将node_modules模块删除掉了，再次运行npm install时，又有新的错误信息出现了

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702205942.png)

然后我找到一个博客找寻到了解决方法：https://blog.csdn.net/Jserm/article/details/105997504

原因是因为：`For security reasons, if you run npm install as root, npm will set the user to “nobody” when running postinstall scripts, which does not have access to the project directory, thus the error message.`

	出于安全原因，如果您以 root 身份运行 npm install，npm 将在运行 postinstall 脚本时将用户设置为“nobody”，该脚本无权访问项目目录，因此会出现错误消息。
	
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702215214.png)
	
**总结：出现这么多错误的原因是：因为之前我上传到服务器上的代码都是我之前已经运行好了，并且配置好了环境变量，已经依赖的，所以导致我上传至服务器后重复安装依赖出现错误。
所以刚才我是重新下载的一个源码直接上传到服务器上去后就直接打jar包成功了**

**但是，这也可能只是我的片面看法，有可能是其它的原因，是我目前所没有想到的，我暂且就将这个错误认为是我之前已经运行好的代码中有依赖环境存在，是我反复的运行没有成功，这个错误，我会在后期中再次进行验证**



## 现在对后端进行打jar包

前提：是服务器上装有maven环境

先去下载一个maven安装包，寻找一个3.6.x版本的maven，找到一个历史地址：[https://archive.apache.org/dist/maven/maven-3/](https://archive.apache.org/dist/maven/maven-3/)

安装完成以后按照codesheep给的安装教程去配置环境变量，注意自己文件夹的命名与codesheep是不一样的

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702221943.png)

**注意：为什么需要打`jar包`与`war包`，因为使用jar包是因为SpringBoot内置了Tomcat服务器，就可以不适用服务器上的Tomcat，而使用内置的Tomcat就行；而打war包就是不使用SpringBoot内置的Tomcat，而是使用服务器自己搭配的Tomcat服务器**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210702223145.png)

>将前后端打包好后，就可以将前端部署到Nginx

1. 修改nginx配置文件

	先将`user`改为root，以防止出现不必要的权限错误（**这里指的是如果你的项目放在root目录下**）
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210703110320.png)
	
	然后修改根目录下的代理，改为`dist`文件夹所在位置
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210703110645.png)

2. 修改配置文件夹后，切记一定要重新加载Nginx，让其重新加载配置文件，否则你就会一直报错

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210703113858.png)

	注意：修改完配置文件后，一定要记得将配置文件中的`pid`注释去掉，否则重新加载配置文件的时候就会报错，找不到pid文件

3. 如果出现了报错就去配置文件中去除该注释，然后再次重新运行Nginx即可

	如果不放心，可以使用命令行去查看进程号，出现这个就代表Nginx启动成功了

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210703114403.png)


> 前端部署起来后就可以开始部署后端了

使用命令`nohup java -jar ruoyi.jar &`  直接运行jar包使用后台运行的方式

但是现在前后端都运行起来了，但是没有对接起来，出现如图所示情况

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210703151212.png)

所以，我们就得去nginx中的配置文件进行配置请求路径，将它的请求路径转到后端上，与之对应

~~~bash
location /prod-api/ {
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header REMOTE-HOST $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://121.89.242.18:8081/;
}
~~~

由于我部署的服务器部署到最后的时候前端连接后端一直超时，没有找到解决问题的原因，所以我暂时放置在这一步了


后续想法：搭载两台虚拟机进行再次搭建



