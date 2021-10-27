# Javaweb



## 1、基本概念

### 1.1、静态web


![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601193856.png)

静态web存在的缺点

- web页面无法更新

- 它无法和数据库交互（数据无法持久化，用户无法交互）

### 1.2、动态web

页面会动态展示：“web的页面展示的效果因人而异”

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601194340.png)

动态web存在的缺点：

- 假如服务器的动态web资源出现了错误，我们需要重新编写我们的**后台程序**，重新发布；

动态web的优点：

- Web页面可以动态更新，所有用户看到的都不是同一个页面

- 它可以与数据库交互

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601194653.png)

## 2、web服务器

### 2.1、技术讲解

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

### 2.2、web服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息

**IIS**

微软的：ASP.......，Windows中自带的

**Tomcat**

下载tomcat：

1. 解压 or 安装

2. 了解配置文件以及目录结构

3. 这个东西的作用

## 3、Tomcat
### 3.1、安装Tomcat

[Tomcat官网](https://tomcat.apache.org/)

### 3.2、Tomcat启动和配置

文件夹的作用

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601201908.png)

访问测试：[Tomcat测试](https://localhost:8080)

可能遇到的问题：
1. Java环境变量没有配置

2.  闪退问题：需要配置兼容性

3.  乱码问题：配置文件中设置

 
### 3.3、配置

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
	

### 3.4、发布一个web网站

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

## 4、Http

### 4.1、什么是Http

HTTP（超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上

- 文本：html，字符串

- 超文本： 图片，音乐，视频，定位，地图。。。。。

- 默认端口：80

HTTPS：安全的

- 默认端口：443

### 4.2、两个时代

- http1.0

	- HTTP/1.0：客户端可以与web服务器连接后，只能获得一个web资源，就断开连接。

- http2.0

	- HTTP/2.0：客户端可以与web服务器连接后，可以获得多个web资源。


### 4.3、HTTP请求

- 客户端---发请求--服务器

~~~Java
1.  Request URL: https://www.baidu.com/				请求地址
    
2.  Request Method: GET										   get方法/post方法
    
3.  Status Code: 200 OK											状态码：200
    
4.  Remote Address: 14.215.177.39:443				远程地址
~~~


#### 1、请求行

- 请求行中的请求方式：GET

- 请求方式：Get，Post，HEAD，DELETE，PUT，TRACT

	-	get：一次请求能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效。

	-	post：一次请求能够携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效。

#### 2、消息头

~~~Java
Accept:  告诉浏览器，它所支持的数据类型
    
Accept-Encoding:  支持那种编码格式GBK  UTF-8 GB2312  ISO8859-1
    
Accept-Language:  告诉浏览器，它的语言环境
    
Cache-Control：缓存控制

Connection： 告诉浏览器，请求完成时断开还是保持连接
~~~



### 4.4、HTTP响应

- 服务器---响应---客户端

百度：

~~~Java
Cache-Control:  private					  缓存控制
    
Connection: keep-alive					 连接	
    
Content-Encoding: gzip					编码
    
Content-Type: ext/html					类型
~~~

#### 1、响应体

~~~Java
Accept:  告诉浏览器，它所支持的数据类型
    
Accept-Encoding:  支持那种编码格式GBK  UTF-8 GB2312  ISO8859-1
    
Accept-Language:  告诉浏览器，它的语言环境
    
Cache-Control：缓存控制

Connection： 告诉浏览器，请求完成时断开还是保持连接
    
Refrush：告诉客户端多久刷新一次

Location： 让网页重新定位
~~~

#### 2、相应状态码

200：代表请求响应成功

3**：请求重定向

- 重定向：你重新到我给你的新位置去

4xx：找不到资源

- 资源不存在

5xx：服务器代码错误    502：网关错误

**常见面试题：**

	当你的浏览器中地址栏输入地址并回车的一瞬间到页面能够展示回来，经历了什么！
	
	
## 5、Maven

### 5.1、Maven项目架构管理工具

我们目前用来就是方便导入Jar包的！

Maven的核心思想：**约定大于配置**

-	有约束就不要去违反

Maven会规定好你该如何去编写我们的Java代码，必须按照这个规定来 

### 5.2、配置环境变量

在系统环境变量中配置如下配置：

-  M2_HOME maven 目录下的bin目录

- MAVEN_HOME　maven的目录

- 在系统的path中配置  %MAVEN_HOME%\bin

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603111722.png)

测试maven是否安装成功，必须配置完毕

### 5.3、阿里云镜像

~~~xml
<mirror>
	<id>alimaven</id>
	<name>aliyun maven</name>
	<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
	<mirrorOf>central</mirrorOf>       
</mirror>
~~~

### 5.4、本地仓库

有本地仓库，就有远程仓库！

**建立一个本地仓库**：localRepository

~~~xml
<localRepository>E:\maven repository\.m2\repository</localRepository>
~~~

### 5.5、IDEA使用Maven

Maven官网：https://mvnrepository.com/

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603112623.png)

IDEAMaven自动配置问题

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603114305.png)

在IDEA中标记文件夹功能

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603113845.png)

### 5.6、配置Tomcat的问题

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

## 6、Servlet

### 6.1、HelloSevlet

Servlet接口Sun公司有两个默认的实现类：HttpServlet，GenericServlet

 1. 构建一个普通的Maven项目，删掉里面的src目录，以后我们的学习就在这个项目里面建议Moudel；
 2. 这个空的工程就是Maven主工程
 
 3. 关于Maven父子工程的理解：

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

      为什么需要映射：我们写的是Java程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所
	  
	  有我们需要在web服务器中注册我们写的Servlet，还需要给他一个浏览器能够访问的路径；
	  
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


### 6.2、Servlet原理

Servlet是由Web服务器调用，web服务器在收到浏览器请求之后，会有如下操作：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210603210339.png)

### 6.3、Mapping

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
	
### 6.4、ServletContext

web容器在启动的时候，它会为每个web应用程序都创建一个对应的ServletContext对象，它代

表了当前的web应用；

#### 1、共享数据

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

#### 2、获取初始化参数

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

#### 3、请求转发

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

#### 4、读取资源文件

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

### 6.5、HttpServletResponse

web服务器接受到客户端的http请求，针对这个请求，分别创建一个代表请求的
HttpServletRequest对

象，代表一个响应的HttpServletResponse；

- 如果要获取客户端请求过来的参数：找HttpServletRequest

- 如果要给客户端响应一些信息：找HttpServletResponse

#### 1、简单分类

负责向浏览器发送数据的方法

```Java
ServletOutputStream getOutputStream() throws IOException;  
  
PrintWriter getWriter() throws IOException;
```

负责向浏览器发送响应头的方法

~~~Java
void setCharacterEncoding(String var1);  
  
void setContentLength(int var1);  
  
void setContentLengthLong(long var1);  
  
void setContentType(String var1);

void addDateHeader(String var1, long var2);

void setHeader(String var1, String var2);

void addHeader(String var1, String var2);

void setIntHeader(String var1, int var2);

void addIntHeader(String var1, int var2);
~~~

#### 2、常见应用

1. 向浏览器输出消息

2. 下载文件
	1. 要获取下载文件的路径

	2. 下载的文件名时什么？

	3. 想办法让浏览器能够支持下载我们需要的东西

	4. 获取下载文件的输入流

	5. 创建缓冲区

	6. 获取OutputStream对象

	7. 将FileOutputStream流写入到buffer缓冲区

	8. 使用OutputStream将缓冲区中的数据输出到客户端！

~~~Java
public class FileServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1. 要获取下载文件的路径
        String realPath = "G:\\IDEADocument\\javaweb\\javaweb-03-servlet\\response\\target\\classes\\韋.png";
        System.out.println("下载文件的路径："+realPath);
        //2. 下载的文件名时什么？如果有/则需要进行转义
        String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);
        //3. 想办法让浏览器能够支持下载我们需要的东西,下载我们需要的东西，中文文件名，则使用URLEncoder.encode编码，否则有可能乱码
        resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode(fileName,"UTF-8"));
        //4. 获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);
        //5. 创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024];
        //6. 获取OutputStream对象
        ServletOutputStream out = resp.getOutputStream();
        //7. 将FileOutputStream流写入到buffer缓冲区 使用OutputStream将缓冲区中的数据输出到客户端！
        while((len = in.read(buffer))>0){
            out.write(buffer,0,len);
        }

        in.close();
        out.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210606161628.png)

	注意：如果图片采用中文名字命名，则需要将其转化，使用URLEncoder.encoder编码，有可能会乱码
	
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210606162027.png)

#### 3、验证码功能

验证怎么来的？

- 前端实现

- 后端实现，需要用到Java的图片类，生成一个图片

~~~Java
public class ImageServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //如何让一个浏览器3秒自动刷新一次
        resp.setHeader("refresh","3");

        //在内存中创建一个图片
        BufferedImage image = new BufferedImage(80, 20, BufferedImage.TYPE_INT_RGB);

        //得到图片
        Graphics2D g = (Graphics2D) image.getGraphics();//笔

        //设置图片的背景颜色
        g.setColor(Color.white);
        g.fillRect(0,0,80,20);

        //给图片写数据
        g.setColor(Color.red);
        g.setFont(new Font(null,Font.BOLD,20));
        g.drawString(makeNum(),0,20);

        //告诉浏览器，这个请求用图片的方式打开
        resp.setContentType("image/jpeg");

        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");

        //把图片写给浏览器
        ImageIO.write(image,"jpg",resp.getOutputStream());
    }

    //生成随机数
    private String makeNum(){
        Random random = new Random();
        String num = random.nextInt(999999) + "";
        StringBuffer sb = new StringBuffer();
        //如果num产生的长度为4，这里就让其空下来的地方填充0，保证其数字长度为6
        for (int i = 0; i <6 - num.length() ; i++) {
            sb.append("0");
        }
        num = sb.toString() + num;
        return num;
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
~~~

~~~xml
<servlet>
	<servlet-name>image</servlet-name>
	<servlet-class>com.clover.servlet.ImageServlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>image</servlet-name>
	<url-pattern>/img</url-pattern>
</servlet-mapping>
~~~

关于`expries`一些简单的简介：https://www.jianshu.com/p/f331d5f0b979

#### 4、实现重定向

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210616213234.png)

B一个web资源收到客户端A请求后，B它会通知A客户端去访问另外一个web资源C，这个过程叫重定向

常见场景：

- 用户登录

~~~Java
void sendRedirect(String var1) throws IOException;
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210607201917.png)

~~~java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
	//重定向
	 /*
	 resp.setHeader("Location","/r/img");
	 resp.setStatus(302);
	 * */
	resp.sendRedirect("/r/img");
}
~~~

~~~xml<html>
<body>
<h2>Hello World!</h2>
<%--这里提交的路径，需要寻找到项目的路径--%>
<%--${pageContext.request.contextPath()}代表当前项目的路径--%>
<form action="${pageContext.request.contextPath}/login" method="get">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit">
</form>


</body>
</html>
~~~

面试题：请你聊聊重定向和转发的区别？.

- 相同点

	- 页面都会实现跳转

- 不同点

	- 请求转发的时候，url不会产生变化

	- 重定向的时候，url地址栏会发生变化


### 6.6、HttpServletRequest

HttpServletRequest代表客户端的请求，用户通过Http协议访问服务器，HTTP请求中的所有信息会被封装

到HttpServletRequest，通过这个HttpServletRequest的方法，获得客户端的所有信息

#### 1、获取参数,请求转发

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210607212542.png)

~~~Java
//通过请求转发
//这里的 / 代表当前的web应用
req.getRequestDispatcher("/r/success.jsp").forward(req,resp);
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210607212915.png)

	注意：如果这里使用`/`它会默认的找不到路径资源，因为这里的`/`就代表着当前的web应用
	
			  重定向的时候需要写全路径
	
		      而转发的时候时不需要写全路径的
	
~~~xml
<h1>登录</h1>

<div>
<%--    这里表单表示的依视：以post方式提交表单，提交到我们的login请求--%>
    <form action="${pageContext.request.contextPath}/login" method="post">
        用户名：<input type="text" name="username"><br>
        密码：<input type="password" name="password"><br>
        爱好：
        <input type="checkbox" name="hobbys" value="女孩">女孩
        <input type="checkbox" name="hobbys" value="代码">代码
        <input type="checkbox" name="hobbys" value="唱歌">唱歌
        <input type="checkbox" name="hobbys" value="电影">电影

        <br>
        <input type="submit">
    </form>
</div>

~~~

~~~java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");

        String username = req.getParameter("username");
        String password = req.getParameter("password");

        String[] hobbys = req.getParameterValues("hobbys");

        System.out.println("====================");
        //后台接受中文乱码问题
        System.out.println(username);
        System.out.println(password);
        System.out.println(Arrays.toString(hobbys));
        System.out.println("====================");

        System.out.println(req.getContextPath());
        //通过请求转发
        //这里的 / 代表当前的web应用
        req.getRequestDispatcher("/r/success.jsp").forward(req,resp);


    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
~~~





## 7、Cookie、Session

### 7.1、会话

**会话**：用户打开一个浏览器，点击了很多超链接，访问web资源，关闭浏览器，这个过程可以称之为会话

**有状态会话：** 一个同学来过教室，下次再来教室，我们会指定这个同学，曾经来过，称之为有状态会话

**一个网站怎么证明你来过？**

客户端（自己）                   服务端（网站）

1. 服务端给客户端一个信件，客户端下次访问服务端带上信件就可以了；cookie

2. 服务器登记你来过了，下次你来的时候我来匹配你；session


### 7.2、保存会话的两种技术

**cookie**

- 客户端技术（响应，请求）



**session**

- 服务器技术，利用这个技术，可以保存用户的会话信息？我们可以把信息或者数据存放在session中！

常见例子：网站登录之后，你下次就不用在登录了，可以直接进去

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210608123319.png)

### 7.3、Cookie

	注意：使用谷歌浏览器解决浏览器显示中文乱码，要多设置一个resp.setContentType("text/html;charset=utf-8");

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210608195726.png)

1. 从请求中拿到cookie信息

2. 服务器响应给客户端cookie

~~~Java
Cookie[] cookies = req.getCookies();//获得cookie
cookie.getName();//获得cookie中的key
cookie.getValue();//获得cookie中的value
new Cookie("lastLoginTime", System.currentTimeMillis() + "");//新建一个cookie
loginTime.setMaxAge(24*60*60);//设置cookie的有效期
resp.addCookie(loginTime);//响应给客户端一个cookie
~~~

数据之间的转化

~~~Java
//获取cookie中的值![[Pasted image 20210608115423.png]]
cookie.getValue();
//将cookie中的值转化为Long型
long lastLoginTime = Long.parseLong(cookie.getValue());
//以时间格式显示
Date date = new Date(lastLoginTime);
//以字符串的形式输出
out.write(date.toLocaleString());
~~~

**cookie：一般会保存在本地的用户目录下的AppData**

一个网站的cookie是否存在上限！

- 一个cookie只能保存一个信息

- 一个web站点可以给浏览器发送多个cookie，最多存放20个cookie

- cookie大小有限制4kb

- 300个cookie浏览器上限

**删除Cookie**

- 不设置有效期，关闭浏览器，自动失效

- 设置有效期为0

~~~Java
public class CookieDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //创建一个Cookie，名字必须要和删除的名字一致
        Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis() + "");

        //设置Cookie的有效期为0，立马过期
        cookie.setMaxAge(0);

        //给客户端响应一个cookie
        resp.addCookie(cookie);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210608173329.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210608173351.png)

传递中文数据的时候，可能会出现乱码

所以可以采用**编码、解码**

~~~Java
URLEncoder.encode("韋","utf-8")
URLDecoder.decode(cookie.getValue(),"utf-8")
~~~

### 7.4、Session（重点）

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210608195902.png)

什么是Session：

- 服务器会给每一个用户（浏览器）创建一个Session对象；

- 一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就存在；

- 用户登录之后，整个网站它都可以访问！－－＞保存用户的信息；保存购物车的信息．．．．．．

Session和Cookie的区别：

- Cookie是把用户的数据写给用户的浏览器，浏览器保存（可以保存多个）

- Session把用户的数据写到用户独占Session中，服务器端保存（保存重要的信息，减少服务器资源的浪费

- Session对象由服务器创建

使用场景：

- 保存一个登录用户的信息

- 购物车信息

- 在整个网站中经常会使用的数据，我们将它保存在Session中

~~~Java
		//Session创建的时候做了什么事情
        /*
        1.把获取到的sessionId存放到Cookie中去
        2.在响应给服务器
        * */
        Cookie cookie = new Cookie("JSESSIONID",sessionId);
        resp.addCookie(cookie);
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210608201509.png)

	Session是在网页一打开就会创建并且一直存在，就算你关闭了网页他也会存在，除非你手动删除

使用Session：

~~~Java
package com.clover.servlet;

import com.clover.pojo.Person;

import javax.servlet.ServletException;
import javax.servlet.http.*;
import java.io.IOException;

public class SessionDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决中文乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session = req.getSession();

        //给Session中存放东西
        session.setAttribute("name",new Person("韋",1));

        //获取Session的ID
        String sessionId = session.getId();

        //判断Session是不是新创建的
        if(session.isNew()){
            resp.getWriter().write("session创建成功，ID："+sessionId);
        }else {
            resp.getWriter().write("session已经在服务器中存在了，ID："+sessionId);
        }

        //Session创建的时候做了什么事情
        /*
        1.把获取到的sessionId存放到Cookie中去
        2.在响应给服务器
        * */
//        Cookie cookie = new Cookie("JSESSIONID",sessionId);
//        resp.addCookie(cookie);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

---------------------------------------------------------
获取对象中的值
public class SessionDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //解决中文乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session = req.getSession();

        Person name = (Person) session.getAttribute("name");

        System.out.println(name);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

-----------------------------------------------------------
手动注销session
public class SessionDemo03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        //取消刚刚获取的name
        session.removeAttribute("name");
        //并且注销掉该session，注销后立马生成新的sessionId，手动注销Session
        session.invalidate();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}


~~~


**会话自动过期：web.xml中配置**
~~~xml
<!--设置session默认失效时间-->
    <session-config>
        <!--1分钟后session自动失效，以分钟为单位-->
        <session-timeout>1</session-timeout>
    </session-config>
~~~

## 8、JSP

### 8.1、什么是JSP

Java Server Pages ：Java服务器端页面，也和Servlet一样，用于动态Web技术

最大的特点：

- 写JSP就像在写HTML

- 区别：
	- HTML只给用户提供静态的数据

	- JSP页面中可以嵌入JAVA代码，为用户提供动态数据


### 8.2、JSP原理

思路：JSP到底是怎么执行的？

- 代码层面没有任何问题

- 服务器内部工作

	tomcat有一个work目录
	
	**IDEA中使用Tomcat会在IDEA的tomcat中产生一个work目录**
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210609200836.png)
	
	我电脑的地址：

~~~xml
C:\Users\REN\.IntelliJIdea2019.3\system\tomcat\Tomcat_9_0_411_javaweb-session-cookie\work\Catalina\localhost\ROOT\org\apache\jsp
~~~

发现页面转变成了Java程序！


![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210610101610.png)

**浏览器向服务器发送请求，不管访问什么资源，其实都是在访问Servlet**

JSP最终也会被转换成为一个Java类！

**JSP本质上就是一个Servlet**

~~~Java
//初始化
public void _jspInit() {
  }
//销毁
  public void _jspDestroy() {
  }
//JSPServlet
  public void _jspService(HttpServletRequest request, HttpServletResponse response)
~~~

1. 判断请求
2. 内置一些对象

~~~Java
final javax.servlet.jsp.PageContext pageContext;											//页面上下文
javax.servlet.http.HttpSession session = null;													//session
final javax.servlet.ServletContext application;													//applicationContext
final javax.servlet.ServletConfig config;														   //配置
javax.servlet.jsp.JspWriter out = null;															    //输出对象
final java.lang.Object page = this;																	 //当前页
HttpServletRequest request									         								//请求
HttpServletResponse response							  										//响应
~~~
3. 输出页面前增加的代码
~~~Java
response.setContentType("text/html");				//设置响应的页面类型
      pageContext = _jspxFactory.getPageContext(this, request, response,
      			null, true, 8192, true);
      _jspx_page_context = pageContext;
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      _jspx_out = out;

~~~

4. 以上的这些个对象，我们可以在JSP页面中直接使用

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210610103853.png)

在JSP页面中；只要是JAVA代码就会原封不动的输出；如果是HTML代码就会被转化为：

~~~xml
out.write("<html>\n");
~~~

这样的格式，输出到前端！

### 8.3、JSP基础语法

**JSP表达式**

~~~jsp
<%--  JSP表达式
作用：用来将程序的输出，输出到客户端
<%= 变量或者表达式%>
--%>
<%= new java.util.Date()%>
~~~

**JSP脚本片段**

~~~jsp
<%--  jsp脚本片段--%>
  <%
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
      sum+=i;
    }
    out.println("<h1>Sum="+sum+"</h1>");
  %>
~~~

**脚本片段的再实现**

~~~jsp
<%
    int x = 10;
    out.println(x);
  %>
  <p>这是一个JSP文档</p>
  <%
    int y = 20;
    out.println(x);
    out.println(y);
  %>

<%--  在代码中嵌入HTML元素--%>
  <%
    for (int i = 0; i < 5; i++) {
  %>
    <h1>hello,world<%= i%></h1>
  <%
    }
  %>
~~~

**JSP声明**

~~~JSP
<%!
    static {
      System.out.println("Loading servlet");
    }

    private int globalvar = 0;

    public void clover(){
      System.out.println("进入了clover方法");
    }
  %>
~~~

	注意：这里使用的<>里面有一个感叹号存在

JSP声明：会被编译到JSP生成的Java类中！其他的，就会被生成到`_jspService`方法中！

JSP的注释，不会在客户端显示，HTML就会！

### 8.4、JSP指令

~~~jsp
<%@page args......%>
<%@include file=""%>

<%--@include会将两个页面合二为一--%>
    <%@include file="common/header.jsp"%>
    <h1>网页主体</h1>
    <%@include file="common/footer.jsp"%>

    <hr>
    
    <%--jsp标签
    jsp:include：拼接页面，本质上还是三个
    --%>
    <jsp:include page="/common/header.jsp"/>
    <h1>网页主体</h1>
    <jsp:include page="/common/footer.jsp"/>
~~~

	注意：使用 jsp:include，其中的page代表当web页面，所以需要在common前面加一个/
			   而两者也是有区别，使用@include，如果同时定义一个名字相同的变量，则会报错
			   使用 jsp:include则不会报错

### 8.5、九大内置对象

- PageContext	存东西

- Request	存东西

- Response

- Session		存东西

- Application  【ServletContext】存东西

- config 【ServletConfig】

- out

- page

- exception

1. 在pageContextDemo01中测试

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%--内置对象--%>
<%
    pageContext.setAttribute("name1","clover一号");//保存的数据只在一个页面中有效
    request.setAttribute("name2","clover二号");//保存的数据只在一次请求中有效，请求转发会携带这个数据
    session.setAttribute("name3","clover三号");//保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
    application.setAttribute("name4","clover四号");//保存的数据只在服务器中有效，从打开服务器到关闭服务器
%>

<%--脚本片段中的代码，会被原封不动生成到.JSP.java
要求：这里面的代码，必须保证Java语法的正确性
--%>
<%
    //从pageContext取出，我们通过寻找的方式来
    //从底层到高层（作用域）:page-->request-->session-->application
    String name1 = (String) pageContext.findAttribute("name1");
    String name2 = (String) pageContext.findAttribute("name2");
    String name3 = (String) pageContext.findAttribute("name3");
    String name4 = (String) pageContext.findAttribute("name4");
    String name5 = (String) pageContext.findAttribute("name5");
%>

<%--使用EL表达式输出   ${} --%>
<h1>取出的值为：</h1>
<h3>${name1}</h3>
<h3>${name2}</h3>
<h3>${name3}</h3>
<h3>${name4}</h3>
<h3><%=name5%></h3>
</body>
</html>

~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210610204732.png)

2.在pageContextDemo02中测试
~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    //从pageContext取出，我们通过寻找的方式来
    //从底层到高层（作用域）:
    String name1 = (String) pageContext.findAttribute("name1");
    String name2 = (String) pageContext.findAttribute("name2");
    String name3 = (String) pageContext.findAttribute("name3");
    String name4 = (String) pageContext.findAttribute("name4");
    String name5 = (String) pageContext.findAttribute("name5");
%>

<%--使用EL表达式输出   ${} --%>
<h1>取出的值为：</h1>
<h3>${name1}</h3>
<h3>${name2}</h3>
<h3>${name3}</h3>
<h3>${name4}</h3>
<h3><%=name5%></h3>
</body>
</html>

~~~

	这里由于时在另外一个页面访问，也没有请求转发，所以一号与二号就消亡了
	
测试结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210610204845.png)

3.在pageContextDemo03中测试

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%
    pageContext.forward("/index.jsp");
    //就相当于执行了request.getRequestDispatcher("/index.jsp").forward(request,response);
%>

</body>
</html>

~~~

测试结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210610205230.png)

request：客户端向服务器发送请求，产生的数据，用户看完就没用了，比如：新闻，用户看完没用了的！

session：客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车

application：客户端向服务器发送请求，产生的数据，一个用户用完了，其它用户还可能使用，比如：聊天数据

### 8.6、JSP标签、JSTL标签，EL表达式

所需依赖

~~~xml
<dependency>
	<groupId>javax.servlet.jsp.jstl</groupId>
	<artifactId>jstl-api</artifactId>
	<version>1.2</version>
</dependency>
<dependency>
	<groupId>taglibs</groupId>
	<artifactId>standard</artifactId>
	<version>1.1.2</version>
</dependency>
~~~

EL表达式：${}

- **获取数据**

- **执行运算**

- **获取web开发常用对象**

**JSP标签**
~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%--jsp:include--%>
<%--
http://localhost:8080/jsptag.jsp?name=clover$age=3
--%>

<jsp:forward page="jsptag2.jsp">
    <jsp:param name="name" value="clover"/>
    <jsp:param name="age" value="3"/>
</jsp:forward>
</body>
</html>
~~~

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--取出参数--%>
名字：<%=request.getParameter("name")%>
年龄：<%=request.getParameter("age")%>

</body>
</html>

~~~

测试结果：
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210610215709.png)

**JSTL表达式**

JSTL标签库的使用就是为了弥补HTML标签的不足，它自定义许多标签，可以供我们使用，标签的功能和Java代码一样

**核心标签**（掌握部分）

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210610210652.png)

**JSTL标签使用步骤**

- 引入对应的taglib

- 使用其中的方法

		注意：在Tomcat也需要引入jstl的包，否则会报错，JSTL解析错误
		
		
~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<h4>if测试</h4>

<hr>

<form action="coreif.jsp" method="get">
<%--    EL表达式获取表单中的数据
        ${param.参数名}
--%>
    <input type="text" name="username" value="${param.username}">
    <input type="submit" value="登录">
</form>

<%--判断如果提交的用户名是管理员，则登录成功--%>
<c:if test="${param.username=='admin'}" var="isAdmin">
    <c:out value="管理员欢迎您！"/>
</c:if>

<%--自闭和标签--%>
<c:out value="${isAdmin}"/>

</body>
</html>
~~~

登录账户不为admin时的结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210610220039.png)

登录账户为admin时的结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210610220022.png)

**corechoose 与 corewhen**

~~~jsp
 <%@ page contentType="text/html;charset=UTF-8" language="java" %>
 <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%--定义一个变量score，值为85--%>
<c:set var="score" value="90"/>

<c:choose>
   <c:when test="${score>=90}">
       你的成绩为优秀
   </c:when>
    <c:when test="${score>=80}">
        你的成绩为良好
    </c:when>
    <c:when test="${score>=70}">
        你的成绩为一般
    </c:when>
    <c:when test="${score<=60}">
        你的成绩为不及格
    </c:when>
</c:choose>

</body>
</html>

~~~

	注意：如果有一个值满足多个条件，它会按照顺序去执行

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210611173033.png)

**coreforeach**
	
~~~jsp
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    ArrayList<String> people = new ArrayList<>();
    people.add(0,"张三");
    people.add(1,"李四");
    people.add(2,"王五");
    people.add(3,"张六");
    people.add(4,"田七");
    request.setAttribute("list",people);
%>
<%--
var,每一次遍历出来的变量
item，要遍历的对象
begin，从哪开始
end，到哪结束
step，每一次执行的步长
--%>
<c:forEach var="people" items="${list}">
    <c:out value="${people}"/><br>
</c:forEach>

<br>

<c:forEach var="people" items="${list}" begin="1" end="3" step="2">
    <c:out value="${people}" />
</c:forEach>
</body>
</html>
~~~

使用了begin end 变量后的情况

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210611173415.png)

## 9、JavaBean

JavaBean有特定的写法：

- 必须要有一个无参构造

- 属性必须私有化

- 必须有对应的get/set方法

一般用来和数据库的字段做映射

ORM：对象关系映射

## 10、MVC三层架构

### 10.1、原来的操作模式

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210616194918.png)

用户直接访问控制层，控制层就可以直接操作数据库

~~~Java
servlet---CRUD---数据库
弊端：程序十分臃肿，不利于维护（在servlet中加入了处理jdbc的代码）
Servlet的代码中：处理请求、响应、视图跳转、处理JDBC、处理业务代码、处理逻辑代码

架构思想：没有什么是加一层解决不了的！
~~~

### 10.2、MVC三层架构
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210616195412.png)

Model

- 业务处理：业务逻辑（Service）

- 数据持久层：CRUD（Dao）

View

- 展示数据

- 提供连接发起Servlet请求

Controller

- 接受用户的请求：（req：请求参数、session信息）

- 交给业务层处理对应的代码

- 控制视图的跳转

~~~Java
登录--->接受用户的登录请求--->处理用户的请求（获取用户登录的参数，username，password）--->交给业务层处理登录业务（判断用户名密码是否正确：事务）--->Dao层查询用户名和密码是否正确--->数据库
~~~

## 11、Filter（重点）

Filter：过滤器，用来过滤网站的数据

- 处理中文乱码

- 登录验证

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210616200630.png)

Filter开发步骤：

 1. 导包
 2. 编写过滤器
	 1. 导包不能导入错误
	 
	 	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210616201236.png)
		
	 2. 编写过滤器，实现Filter接口，重写对应的方法
~~~Java
package com.clover.filter;

import javax.servlet.*;
import java.io.IOException;

public class CharacterEncodingFilter implements Filter {

    //初始化：web服务器启动，就已经初始化了，随时等待过滤对象出现！
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("CharacterEncodingFilter初始化");
    }

    //Chain：链
    /*
    * 1.过滤器中的所有代码，再过滤特定请求的时候都会执行
    * 2.必须要让过滤器继续执行（因为可能存在多个过滤器，所以要继续执行下去）
    * */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {

        request.setCharacterEncoding("UTF-8");
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=utf-8");

        System.out.println("CharacterEncodingFilter执行前.........");
        chain.doFilter(request,response);//让我们的请求继续走，如果不屑，程序执行到这里就会被拦截停止！
        System.out.println("CharacterEncodingFilter执行后.........");
    }

    //销毁
    @Override
    public void destroy() {
        System.out.println("CharacterEncodingFilter销毁");
    }
}

~~~
     
	 
   3.编写web.xml配置Filter
   
~~~xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>com.clover.filter.CharacterEncodingFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
<!--    只要是 /servlet下的任何请求，都会经过这个过滤器-->
    <url-pattern>/servlet/*</url-pattern>
  </filter-mapping>
~~~

## 12、监听器

1. 编写一个监听器

	实现一个监听器的接口
	
~~~Java
package com.clover.listener;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class OnlineCountListener implements HttpSessionListener {
    //创建session监听：查看你的一举一动
    //一旦创建Session就会出发一次这个事件！
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext context = se.getSession().getServletContext();
        Integer onlineCount = (Integer) context.getAttribute("OnlineCount");
        if(onlineCount == null){
            onlineCount = new Integer(1);
        }else {
            int count = onlineCount.intValue();
            onlineCount = new Integer(count+1);
        }
        context.setAttribute("OnlineCount",onlineCount);
    }

    //销毁Session监听
    //一旦销毁Session就会出发一次这个事件！
    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext context = se.getSession().getServletContext();
        Integer onlineCount = (Integer) context.getAttribute("OnlineCount");
        if(onlineCount == null){
            onlineCount = new Integer(1);
        }else {
            int count = onlineCount.intValue();
            onlineCount = new Integer(count-1);
        }
        context.setAttribute("OnlineCount",onlineCount);
    }
	
	/*
	* 销毁session有两种方法
	*1.自动销毁  getSession().invalidate();
	*2.手动销毁  在web.xml中设置session存活时间
	*/
}

~~~

2. 在web.xml中注册监听器

~~~xml
<!--注册监听器-->
  <listener>
    <listener-class>com.clover.listener.OnlineCountListener</listener-class>
  </listener>
~~~

## 13、过滤器、监听器常见应用

