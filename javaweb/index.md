# Javaweb



# 1、基本概念

## 1.1、静态web


![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601193856.png)

静态web存在的缺点

- web页面无法更新

- 它无法和数据库交互（数据无法持久化，用户无法交互）

## 1.2、动态web

页面会动态展示：“web的页面展示的效果因人而异”

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601194340.png)

动态web存在的缺点：

- 假如服务器的动态web资源出现了错误，我们需要重新编写我们的**后台程序**，重新发布；

动态web的优点：

- Web页面可以动态更新，所有用户看到的都不是同一个页面

- 它可以与数据库交互

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601194653.png)

# 2、web服务器

## 2.1、技术讲解

PHP：

- PHP开发速度很快，功能很强大，跨平台，代码很简单

- 无法承载大量访问的情况

JSP/Servlet：

	B/S：浏览器和服务器

	C/S：客户端和服务器
- sun公司主推的B/S架构

- 基于Java语言（所有的大公司，或者一些开源的组件，都是用Java写的）

- 可以承载三高问题带来的影响
	- 高可用

	- 高并发

	- 高性能

- 语法像ASP

## 2.2、web服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息

**IIS**

微软的：ASP.......，Windows中自带的

**Tomcat**

下载tomcat：

1. 解压 or 安装

2. 了解配置文件以及目录结构

3. 这个东西的作用

# 3、Tomcat
## 3.1、安装Tomcat

[Tomcat官网](https://tomcat.apache.org/)

## 3.2、Tomcat启动和配置

文件夹的作用

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601201908.png)

访问测试：[Tomcat测试](https://localhost:8080)

可能遇到的问题：
1. Java环境变量没有配置

2.  闪退问题：需要配置兼容性

3.  乱码问题：配置文件中设置

 
## 3.3、配置

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601202139.png)

可以配置启动的端口号

- tomcat的默认端口是：8080

~~~xml
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
~~~

可以配置主机的名称

- 默认主机名为：localhost--->127.0.0.1

- 默认网站应用存放的位置：webapps


~~~xml
<Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
~~~

**高难度面试题：**

	请你谈谈网站是如何进行访问的！
	1. 输入一个域名；回车
	2. 检查本机的C:\Windows\System32\drivers\etc\hosts配置文件下有没有这个域名映射；
		1. 有：直接返回对应的IP地址，这个地址中，有我们需要访问的web程序，可以直接访问
		2. 没有：去DNS服务器找，找到的话就返回，找不到就返回找不到
	
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601210836.png)
	

## 3.4、发布一个web网站

[查看Tomcat自带的一些样例](http://localhost:8080/examples/servlets/)

- 将自己写的网站，放到服务器（Tomcat）中指定的web应用的文件夹（webapps）下，就可以访问了

- 网站应有的结构

~~~xml
--webapps ：Tomcat服务器的web目录
		--ROOT
		--clover ： 网站的目录名
			- WEB-INF
				- classes ： Java程序
				- lib ：web应用所有依赖的jar包
				- web.xml ：网站配置文件
			- index.html  默认的首页
			- static
				-css
					-style.css
				-js
				-img
~~~

# 4、Http

## 4.1、什么是Http

HTTP（超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上

- 文本：html，字符串

- 超文本： 图片，音乐，视频，定位，地图。。。。。

- 默认端口：80

HTTPS：安全的

- 默认端口：443

## 4.2、两个时代

- http1.0

	- HTTP/1.0：客户端可以与web服务器连接后，只能获得一个web资源，就断开连接。

- http2.0

	- HTTP/2.0：客户端可以与web服务器连接后，可以获得多个web资源。


## 4.3、HTTP请求

- 客户端---发请求--服务器

~~~Java
1.  Request URL: https://www.baidu.com/				请求地址
    
2.  Request Method: GET										   get方法/post方法
    
3.  Status Code: 200 OK											状态码：200
    
4.  Remote Address: 14.215.177.39:443				远程地址
~~~


### 1、请求行

- 请求行中的请求方式：GET

- 请求方式：Get，Post，HEAD，DELETE，PUT，TRACT

	-	get：一次请求能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效。

	-	post：一次请求能够携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效。

### 2、消息头

~~~Java
Accept:  告诉浏览器，它所支持的数据类型
    
Accept-Encoding:  支持那种编码格式GBK  UTF-8 GB2312  ISO8859-1
    
Accept-Language:  告诉浏览器，它的语言环境
    
Cache-Control：缓存控制

Connection： 告诉浏览器，请求完成时断开还是保持连接
~~~



## 4.4、HTTP响应

- 服务器---响应---客户端

百度：

~~~Java
Cache-Control:  private					  缓存控制
    
Connection: keep-alive					 连接	
    
Content-Encoding: gzip					编码
    
Content-Type: ext/html					类型
~~~

### 1、响应体

~~~Java
Accept:  告诉浏览器，它所支持的数据类型
    
Accept-Encoding:  支持那种编码格式GBK  UTF-8 GB2312  ISO8859-1
    
Accept-Language:  告诉浏览器，它的语言环境
    
Cache-Control：缓存控制

Connection： 告诉浏览器，请求完成时断开还是保持连接
    
Refrush：告诉客户端多久刷新一次

Location： 让网页重新定位
~~~

### 2、相应状态码

200：代表请求响应成功

3**：请求重定向

- 重定向：你重新到我给你的新位置去

4xx：找不到资源

- 资源不存在

5xx：服务器代码错误    502：网关错误

**常见面试题：**

	当你的浏览器中地址栏输入地址并回车的一瞬间到页面能够展示回来，经历了什么！
	
	
# 5、Maven

## 5.1、Maven项目架构管理工具

我们目前用来就是方便导入Jar包的！

Maven的核心思想：**约定大于配置**

-	有约束就不要去违反

Maven会规定好你该如何去编写我们的Java代码，必须按照这个规定来 

## 5.2、配置环境变量

在系统环境变量中配置如下配置：

-  M2_HOME maven 目录下的bin目录

- MAVEN_HOME　maven的目录

- 在系统的path中配置  %MAVEN_HOME%\bin

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603111722.png)

测试maven是否安装成功，必须配置完毕

## 5.3、阿里云镜像

~~~xml
<mirror>
	<id>alimaven</id>
	<name>aliyun maven</name>
	<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
	<mirrorOf>central</mirrorOf>       
</mirror>
~~~

## 5.4、本地仓库

有本地仓库，就有远程仓库！

**建立一个本地仓库**：localRepository

~~~xml
<localRepository>E:\maven repository\.m2\repository</localRepository>
~~~

## 5.5、IDEA使用Maven

Maven官网：https://mvnrepository.com/

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603112623.png)

IDEAMaven自动配置问题

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603114305.png)

在IDEA中标记文件夹功能

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603113845.png)

## 5.6、配置Tomcat的问题

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603192041.png)

解决警告问题

必须要的配置：**为什么会有这个问题：我们访问一个网站，需要指定一个文件夹的名字**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603192456.png)


	maven由于它的约定大于配置，我们之后可能遇到我们写的配置文件，无法被导出或者生效的问题。

解决方案：

~~~xml
<!--在build中配置resources，来防止我们资源导出失败的问题-->  
<build>  
	 <resources> 
		 <resource> 
			 <directory>src/main/resources</directory>  
			 <includes> 
					<include>**/*.properties</include>  
					<include>**/*.xml</include>  
			</includes>
		 </resource> 
		 <resource> 
			 <directory>src/main/java</directory>  
				 <includes> 
					 <include>**/*.properties</include>  
					 <include>**/*.xml</include>  
				 </includes>
			 <filtering>true</filtering>  
		 </resource>
	</resources>
</build>
~~~

# 6、Servlet

## 6.1、HelloSevlet

Servlet接口Sun公司有两个默认的实现类：HttpServlet，GenericServlet

 1. 构建一个普通的Maven项目，删掉里面的src目录，以后我们的学习就在这个项目里面建议Moudel；这个空的工程就是Maven主工程
 2. 关于Maven父子工程的理解：

 	父项目中会有
	
~~~xml
<modules>  
	<module>servlet-01</module>  
</modules>
~~~

	子项目会有
	
~~~xml
<parent>  
	 <artifactId>javaweb-03-servlet</artifactId>  
	 <groupId>com.clover</groupId>  
	 <version>1.0-SNAPSHOT</version>  
</parent>
~~~
	
	父项目中的Jar包子项目可以直接使用，而子项目中的jar包父项目不可以使用

 3. Maven环境优化

	1. 修改web.xml为最新版本（可以去tomcat中的webapps里面找到）
	2. 将Maven的结构搭建完整
	
 4. 编写一个Servlet程序

	1. 编写一个普通类
	2. 实现Servlet接口，这里我们直接继承HttpServlet

~~~java
	public class HelloServlet extends HttpServlet {  
		@Override  
		 protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
				PrintWriter writer = resp.getWriter();  
				writer.print("Hello,Servlet");  
		 }  

		@Override  
		 protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
				doGet(req, resp);  
		 }  
	}	
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603203836.png)

 5.  编写Servlet映射

      为什么需要映射：我们写的是Java程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所有我们需要在web服务器中注册我们写的Servlet，还需要给他一个浏览器能够访问的路径；
	  
~~~xml
<!--注册Servlet-->  
<servlet>  
	 <servlet-name>hello</servlet-name>  
	 <servlet-class>com.clover.servlet.HelloServlet</servlet-class>  
</servlet>  
<!--    Servlet的请求路径-->  
<servlet-mapping>  
	 <servlet-name>hello</servlet-name>  
	 <url-pattern>/hello</url-pattern>  
</servlet-mapping>
~~~
	  
 6.  配置tomcat

       注意：配置项目的发布路径就可以了

 7.  启动测试	   


## 6.2、Servlet原理

Servlet是由Web服务器调用，web服务器在收到浏览器请求之后，会有如下操作：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603210339.png)

## 6.3、Mapping

1. 一个Servlet请求可以指定一个映射路径
~~~xml
<servlet-mapping>  
	<servlet-name>hello</servlet-name>  
	<url-pattern>/hello</url-pattern>  
</servlet-mapping>
~~~
2. 一个Servlet请求可以指定多个映射路径
~~~xml
<servlet-mapping>  
	<servlet-name>hello</servlet-name>  
	<url-pattern>/hello</url-pattern>  
</servlet-mapping>  
<servlet-mapping>  
	<servlet-name>hello</servlet-name>  
	<url-pattern>/hello2</url-pattern>  
</servlet-mapping>  
<servlet-mapping>  
	<servlet-name>hello</servlet-name>  
	<url-pattern>/hello3</url-pattern>  
</servlet-mapping>  
<servlet-mapping>  
	<servlet-name>hello</servlet-name>  
	<url-pattern>/hello4</url-pattern>  
</servlet-mapping>  
<servlet-mapping>  
	<servlet-name>hello</servlet-name>  
	<url-pattern>/hello5</url-pattern>  
</servlet-mapping>
~~~
3. 一个Servlet请求可以指定通用映射路径
~~~xml
<servlet-mapping>  
	<servlet-name>hello</servlet-name>  
	<url-pattern>/hello\*</url-pattern>  
</servlet-mapping>
~~~
4. 默认请求路径
~~~xml
<servlet-mapping>  
	 <servlet-name>hello</servlet-name>  
	 <url-pattern>/*</url-pattern>  
</servlet-mapping>
~~~
5. 指定一些后缀或者前缀等等
~~~xml
<!--可以自定义后缀实现请求映射  
	   注意点：*前面不能加项目映射的路径  
-->  
<servlet-mapping>  
	<servlet-name>hello</servlet-name>  
	<url-pattern>*.clover</url-pattern>  
</servlet-mapping>
~~~
6. 优先级问题

		指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求了
		
~~~xml
<!--404-->  
<servlet>  
	<servlet-name>error</servlet-name>  
	<servlet-class>com.clover.servlet.ErrorServlet</servlet-class>  
</servlet>  
<servlet-mapping>  
	<servlet-name>error</servlet-name>  
	<url-pattern>/*</url-pattern>  
</servlet-mapping>
~~~
		注意：在使用IDEA创建多个webapps工程时，在第二个运行可能会出现404找不到资源问题
				   这里是因为新创建的webapps文件上没有带蓝色点点,需要自己手动去加上，然后运行才会成功。
				   如若创建新的工程的pom.xml中没有parent那一行，重新创建即可
				   另外在创建新文件时应当先创建java跟resources两个文件夹，并且要标记，否则无法创建package
	
## 6.4、ServletContext

web容器在启动的时候，它会为每个web应用程序都创建一个对应的ServletContext对象，它代

表了当前的web应用；

### 1、共享数据

我在这个Servlet中保存的数据，可以在另外一个servlet中拿到

~~~java
public class HelloServlet extends HttpServlet {  
    @Override  
	 protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  

	//        this.getInitParameter() 初始化参数     很少用  
	//        this.getServletConfig()   Servlet参数    很少用  
	//        this.getServletContext()  Servlet上下文  

	 ServletContext context = this.getServletContext();  

	 String username = "韋";  
	 context.setAttribute("username",username);//将一个数据保存在了ServletContext中，名字为：username，值为：username  

	 System.out.println("hello");  
	 }  
  
}
~~~

~~~java
public class GetServlet extends HttpServlet {  
  
    @Override  
	 protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
			 ServletContext context = this.getServletContext();  
			 String username = (String) context.getAttribute("username");  

			 //设置显示页面不乱码问题  
			 resp.setContentType("text/html");  
			 resp.setCharacterEncoding("utf-8");  
			 resp.getWriter().print("名字为："+username);  
	 }  

		@Override  
	 protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
			doGet(req, resp);  
	 }  
}
~~~

~~~xml
<servlet>  
	 <servlet-name>hello</servlet-name>  
	 <servlet-class>com.clover.servlet.HelloServlet</servlet-class>  
</servlet>  
<servlet-mapping>  
	 <servlet-name>hello</servlet-name>  
	 <url-pattern>/hello2</url-pattern>  
</servlet-mapping>  
  
<servlet>  
	 <servlet-name>getc</servlet-name>  
	 <servlet-class>com.clover.servlet.GetServlet</servlet-class>  
</servlet>  
<servlet-mapping>  
	 <servlet-name>getc</servlet-name>  
	 <url-pattern>/getc</url-pattern>  
</servlet-mapping>
~~~
	
测试结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210604203557.png)

### 2、获取初始化参数

~~~xml
<context-param>  
	 <param-name>url</param-name>  
	 <param-value>jdbc:mysql//localhost:3306/mybatis</param-value>  
</context-param>

<servlet>  
	 <servlet-name>sd3</servlet-name>  
	 <servlet-class>com.clover.servlet.ServletDemo03</servlet-class>  
</servlet>  
<servlet-mapping>  
	 <servlet-name>sd3</servlet-name>  
	 <url-pattern>/sd3</url-pattern>  
</servlet-mapping>
~~~

~~~java
public class ServletDemo03 extends HttpServlet {  
    @Override  
	 protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
			 ServletContext context = this.getServletContext();
			 //此处获取上下文对象，获取初始化参数对应web.xml中的context-param中的参数，当你访问这个路径的时候，页面进行跳转的时候，就会获取到该路径对应的值
			 String url = context.getInitParameter("url");  
			 resp.getWriter().print(url);  
	 }  

		@Override  
	 protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
			doGet(req, resp);  
	 }  
}
~~~

### 3、请求转发

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210604214223.png)

~~~java
public class ServletDemo04 extends HttpServlet {  
		@Override  
		 protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
				 ServletContext context = this.getServletContext();  
				 System.out.println("进入demo4");  
//        		RequestDispatcher requestDispatcher = context.getRequestDispatcher("/sd3");//转发的请求路径 
//        		requestDispatcher.forward(req,resp);//调用forward实现转发请求
				 context.getRequestDispatcher("/sd3").forward(req,resp);  
		 }  

		@Override  
		 protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
				doGet(req, resp);  
		 }  
}
~~~

~~~xml
<servlet>  
	 <servlet-name>sd4</servlet-name>  
	 <servlet-class>com.clover.servlet.ServletDemo04</servlet-class>  
</servlet>  
<servlet-mapping>  
	 <servlet-name>sd4</servlet-name>  
	 <url-pattern>/sd4</url-pattern>  
</servlet-mapping>
~~~

### 4、读取资源文件

Properties

- 在Java目录下新建一个properties

- 在resources目录下新建一个properties

发现：都打包到了同一个路径下：`classes`；我们俗称这个路径为`classpath`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210604213420.png)

思路：需要一个文件流

~~~xml
username=root  
password=123456
~~~

~~~java
public class ServletDemo05 extends HttpServlet {  
    @Override  
	 protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
			 InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");  

			 Properties properties = new Properties();  
			 properties.load(is);  
			 String user = properties.getProperty("username");  
			 String pwd = properties.getProperty("password");  

			 resp.getWriter().print(user+":"+pwd);  
	 }  

		@Override  
		 protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
				doGet(req, resp);  
		 }  
}
~~~

访问测试结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210604220055.png)

## 6.5、Request

明日任务

## 6.6、Response

明日任务



