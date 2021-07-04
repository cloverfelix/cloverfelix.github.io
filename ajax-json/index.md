# Ajax-Json


# Json和Ajax

需要掌握的知识

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627173156.png)

早期网站：如登录功能，需要刷新页面，才能重新登录；不点击提交按钮，就不知道自己密码输错了；

现在大多数的网站，都是局部刷新，不刷新整个页面的情况下，实现更新；

注册的时候，发现手机已经注册过了，但是你只是输入了，没有提交，然后它就提示了

> 什么是Json

1、JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。

2、采用完全独立于变成语言的**文本格式**来存储和表示数据。

3、简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。

4、易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

- 对象表示为键值对

- 数据由逗号分隔

- 花括号保存对象

- 方括号保存属组

JSON键值对是用来保存JavaScript对象的一种方式，和 JavaScript 对象的写法也大同小异，键--值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

~~~json
{"name": "clover"}  
{"age": "3"}  
{"sex": "男"}
~~~

很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：

JSON是JavaScript对象的字符串表示法，它使用文本表示一个JS对象的信息，本质是一个字符串

~~~json
var obj = {a: 'Hello', b: 'World'}; 			//这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
~~~

**JSON和 JavaScript对象互转**

要实现从JSON字符串转换为JavaScript对象，使用**JSON.parse()方法**：
~~~JavaScript
var obj = JSON.parse('{"a": "Hello", "b": "World"}');
//结果是 {a: 'Hello', b: 'World'}
~~~

要实现从JavaScript 对象转换为JSON字符串，使用**JSON.stringify()** 方法：
~~~JavaScript
var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}'
~~~

代码测试：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>


<script type="text/javascript">
    //编写一个对象
    var user = {
        name: "韋",
        age: 3,
        sex: "男"
    };
    console.log(user);
    //将js对象转化为json字符串
    var str = JSON.stringify(user);
    console.log(str);
    //将json字符串转化为js兑现对象
    var users = JSON.parse(str);
    console.log(users);
</script>

</body>
</html>
~~~

结果图：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627180521.png)

>Controller控制层返回JSON数据

导入jackson所需要的包

~~~xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
   <groupId>com.fasterxml.jackson.core</groupId>
   <artifactId>jackson-databind</artifactId>
   <version>2.9.8</version>
</dependency>
~~~

配置SpringMVC需要的配置web.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--1.注册servlet-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!-- 启动顺序，数字越小，启动越早 -->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--所有请求都会被springmvc拦截 -->
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/</url-pattern>
    </filter-mapping>

</web-app>
~~~

springmvc-servlet.xml

~~~xml
<?xml version="1.0" encoding="UTF8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
    <context:component-scan base-package="com.clover.controller"/>
    <!-- 让Spring MVC不处理静态资源 -->
    <mvc:default-servlet-handler />
    <!--
    支持mvc注解驱动
        在spring中一般采用@Request Mapping注解来完成映射关系
        要想使@Request Mapping注解生效
        必须向上下文中注册Default Annotation Handler Mapping
        和一个Annotation Method Handler Adapter实例
        这两个实例分别在类级别和方法级别处理。
        而annotation-driven配置帮助我们自动完成上述两个实例的注入。
     -->
    <mvc:annotation-driven />

    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
          id="internalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
~~~

编写User实体类
~~~Java
package com.clover.pojo;

public class User {
    private String name;
    private int age;
    private String sex;

    public User() {
    }

    public User(String name, int age, String sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                '}';
    }
}
~~~

这里我们需要两个新东西，一个**是@ResponseBody**，一个是**ObjectMapper**对象，我们看下具体的用法

编写一个Controller；
~~~Java
package com.clover.controller;

import com.clover.pojo.User;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class UserController {


    //思考问题，我们正常返回他会走视图解析器，而json需要返回的是一个字符串
    //市面上有很多的第三方jar包可以实现这个功能，jackson，只需要一个简单的注解就可以实现了
    //
    //@ResponseBody,将服务器端返回的对象转换为json对象响应回去
    @RequestMapping("/json1")
    @ResponseBody
    public String json1() throws JsonProcessingException {
        //需要一个jackson对象的映射器，就是一个类，使用它就可以直接将对象转换为json字符串
        ObjectMapper mapper = new ObjectMapper();
        //创建一个对象
        User user = new User("韋",3,"男");
        System.out.println(user);

        //将Java对象转换为json字符串
        String str = mapper.writeValueAsString(user);
        System.out.println(str);

        return str;//由于使用了@ResponseBody注解，这里会将str以json格式的字符串返回
    }

    //发现了一个问题，乱码了，怎么解决？给@RequestMapping加上一个属性
    @RequestMapping(value = "/json2",produces = "application/json;charset=utf-8")
    @ResponseBody
    public String json2() throws JsonProcessingException {
        //需要一个jackson对象的映射器，就是一个类，使用它就可以直接将对象转换为json字符串
        ObjectMapper mapper = new ObjectMapper();
        //创建一个对象
        User user = new User("韋",3,"男");
        System.out.println(user);

        //将Java对象转换为json字符串
        String str = mapper.writeValueAsString(user);
        System.out.println(str);

        return str;//由于使用了@ResponseBody注解，这里会将str以json格式的字符串返回
    }
}

~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627191827.png)

发现出现了乱码问题，我们需要设置一下他的编码格式为utf-8，以及它返回的类型；

通过@RequestMaping的produces属性来实现，修改下代码

~~~Java
//produces:指定响应体返回类型和编码
@RequestMapping(value = "/json1",produces = "application/json;charset=utf-8")
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627191847.png)

> 代码优化

**乱码问题统一解决**

上一种方法比较麻烦，如果项目中有许多请求则每一个都要添加，可以通过Spring配置统一指定，这样就不用每次都去处理了！

我们可以在springmvc的配置文件上添加一段消息StringHttpMessageConverter转换配置！

**这里的消息转换器是一个重点！！需要多了解一下**

~~~xml
<mvc:annotation-driven>
        <!--SON格式化-->
        <mvc:message-converters register-defaults="true">
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="UTF-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
~~~

**返回json字符串统一解决**

在类上直接使用 **@RestController** ，这样子，里面所有的方法都只会返回 json 字符串了，不用再每一个都添加@ResponseBody ！我们在前后端分离开发中，一般都使用 @RestController ，十分便捷！

~~~Java
@Controller
@RestController
public class UserController {

	//produces:指定响应体返回类型和编码
    //@ResponseBody,将服务器端返回的对象转换为json对象响应回去
    @RequestMapping("/json1")
    public String json1() throws JsonProcessingException {
        //需要一个jackson对象的映射器，就是一个类，使用它就可以直接将对象转换为json字符串
        ObjectMapper mapper = new ObjectMapper();
        //创建一个对象
        User user = new User("韋",3,"男");
        System.out.println(user);

        //将Java对象转换为json字符串
        String str = mapper.writeValueAsString(user);
        System.out.println(str);

        return str;//由于使用了@ResponseBody注解，这里会将str以json格式的字符串返回
    }
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627195809.png)

> 测试集合输出

~~~Java
@RequestMapping("/json3")
    @ResponseBody
    public String json3() throws JsonProcessingException {

        List<User> list = new ArrayList<>();

        //创建一个对象
        User user1 = new User("韋1",3,"男");
        User user2 = new User("韋2",3,"男");
        User user3 = new User("韋3",3,"男");
        User user4 = new User("韋4",3,"男");

        list.add(user1);
        list.add(user2);
        list.add(user3);
        list.add(user4);


        return new ObjectMapper().writeValueAsString(list);
    }
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627192759.png)

>输出时间对象

增加一个新的方法

~~~Java
 @RequestMapping("/time1")
    @ResponseBody
    public String json4() throws JsonProcessingException {
        //创建一个日期对象
        Date date = new Date();

        //发现问题：时间默认返回的json字符串变成了时间戳的格式：1624793651760  Timestamp
        return new ObjectMapper().writeValueAsString(date);
    }
~~~

运行结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627194829.png)

- 默认日期格式会变成一个数字，是自1970年1月1日到当前日期的毫秒数！

- Jackson默认是会把时间转换成timestamps形式

解决方法：取消timestamps形式，自定义时间格式

~~~Java
@RequestMapping("/time2")
    @ResponseBody
    public String json5() throws JsonProcessingException {

        ObjectMapper mapper = new ObjectMapper();
        //1.如何让它不返回时间戳，所以我们要关闭它的时间戳功能
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
        //2.时间格式话问题，自定义日期格式对象
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        //3.让mapper指定时间日期格式为simpleDateFormat
        mapper.setDateFormat(simpleDateFormat);

        //创建一个日期对象
        Date date = new Date();


        return mapper.writeValueAsString(date);
    }
~~~

运行结果

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627195041.png)

>抽取代码为工具类

如果经常编写一些重复的代码，这样会比较麻烦，我们皆可以将这些重复的代码**封装**到一个工具类中

~~~Java
package com.clover.utils;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

import java.text.SimpleDateFormat;

public class JsonUtils {

    /**
     * 这个地方就能够写得更灵活一点，使用函数重载，可以自定义时间日期的格式
     */
    public static String getJson(Object object){
        return getJson(object,"yyyy-MM-dd HH:mm:ss");
    }

    public static String getJson(Object object,String dateFormat){
        ObjectMapper mapper = new ObjectMapper();
        //1.如何让它不返回时间戳，所以我们要关闭它的时间戳功能
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
        //2.时间格式话问题，自定义日期格式对象
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(dateFormat);
        //3.让mapper指定时间日期格式为simpleDateFormat
        mapper.setDateFormat(simpleDateFormat);

        try {
            return mapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return null;
    }
}
~~~

我们使用工具类，代码就更为简洁了

~~~Java
@RequestMapping("/time3")
    @ResponseBody
    public String json6() {
        return JsonUtils.getJson(new Date());
    }
~~~

**AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。**

**Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。**

> 伪造Ajax

编写一个iframe.html用于测试

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>伪造Ajax</title>
</head>
<body>

<script type="text/javascript">
    function loadPage() {
       var targetURL = document.getElementById('url').value;
       console.log(targetURL);
       document.getElementById('iframePosition').src = targetURL;
    }
</script>

<div>
    <p>请输入要加载的地址:</p>
    <p>
        <input type="text" id="url" value="https://cloverfelix.github.io/"/>
        <input type="button" value="提交" onclick="loadPage()"/>
    </p>

</div>

<div>
    <h3>
        加载页面的位置
    </h3>
    <iframe style="width: 100%;height: 800px" id="iframePosition">

    </iframe>
</div>


</body>
</html>
~~~

运行结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627210243.png)

**利用Ajax可以做**：

- 注册时，输入用户名自动检测用户是否已经存在

- 登录时，提示用户名密码错误

- 删除数据行时，将行ID发送到后台，后台在数据库中删除，数据库删除成功后，在页面DOM中将数据行也删除

>jQuery ajax

使用jQuery需要先导入jQuery的js文件

Ajax的核心是**XMLHttpRequest对象(XHR)**。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。

jQuery 不是生产者，而是大自然搬运工。

jQuery Ajax本质就是 XMLHttpRequest，对他进行了封装，方便调用！

~~~jquery
jQuery.get(...)
      部分参数：
            url：请求地址
            data：要发送的数据Key/Value参数
			success：载入成功时回调函数
			dataType：返回内容格式，xml，json，script，text，html
~~~

~~~jquery
jQuery.post(...)
      部分参数：
            url：请求地址
            data：要发送的数据Key/Value参数
			success：载入成功时回调函数
			dataType：返回内容格式，xml，json，script，text，html
~~~

~~~jquery
jQuery.ajax(...)
      部分参数：
            url：请求地址
            type：请求方式，GET、POST（1.9.0之后用method）
        headers：请求头
            data：要发送的数据
    contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
          async：是否异步
        timeout：设置请求超时时间（毫秒）
      beforeSend：发送请求前执行的函数(全局)
        complete：完成之后执行的回调函数(全局)
        success：成功之后执行的回调函数(全局)
          error：失败之后执行的回调函数(全局)
        accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
        dataType：将服务器端返回的数据转换成指定类型
          "xml": 将服务器端返回的内容转换成xml格式
          "text": 将服务器端返回的内容转换成普通文本格式
          "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
        "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
          "json": 将服务器端返回的内容转换成相应的JavaScript对象
        "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
~~~

>获取一个集合对象

实体类User

~~~Java
package com.clover.pojo;

public class User {
    private String name;
    private int age;
    private String sex;

    public User() {
    }

    public User(String name, int age, String sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                '}';
    }
}
~~~

获取一个集合对象，返回给前端

~~~Java
@RequestMapping("/a2")
    @ResponseBody
    public List<User> ajax2(){
        List<User> list = new ArrayList<>();

        //创建一个对象
        User user1 = new User("韋1",3,"男");
        User user2 = new User("韋2",3,"男");
        User user3 = new User("韋3",3,"男");
        User user4 = new User("韋4",3,"男");

        list.add(user1);
        list.add(user2);
        list.add(user3);
        list.add(user4);

        return list;//由于使用了@ResponseBody注解，它会给我们返回一个字符串
    }
~~~

前端页面

~~~html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<input type="button" id="btn" value="获取数据"/>
<table width="80%" align="center">
    <tr>
        <td>姓名</td>
        <td>年龄</td>
        <td>性别</td>
    </tr>
    <tbody id="content">
    </tbody>
</table>

<script src="${pageContext.request.contextPath}/static/js/jquery-3.6.0.js"></script>
<script>
<%--这里使用$表示让其一进入就执行函数--%>
    $(function () {
        $("#btn").click(function () {
            $.post("${pageContext.request.contextPath}/ajax/a2",function (data) {
                console.log(data);
                var html = "";
                for (var i = 0;i<data.length;i++){
                    html += "<tr>"+
                        "<td>" + data[i].name + "</td>"+
                        "<td>" + data[i].age + "</td>"+
                        "<td>" + data[i].sex + "</td>"+
                            "</tr>"
                }
                $("#content").html(html);
            })
        })
    })
</script>

</body>
</html>
~~~

运行结果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627221822.png)

> 注册提示效果

编写一个Controller

~~~Java
@RequestMapping("/a3")
    @ResponseBody
    public String ajax3(String name,String pwd){
        String msg = "";
        if(name!=null){
            if("admin".equals(name)){
                msg = "ok";
            }else {
                msg = "用户名输入有误";
            }
        }
        if(pwd!=null){
            if("123456".equals(pwd)){
                msg = "ok";
            }else {
                msg = "密码输入有误";
            }
        }
        return msg;
    }
~~~

前端页面

~~~html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="${pageContext.request.contextPath}/static/js/jquery-3.6.0.js"></script>
    <script>
        function a1() {
            $.post("${pageContext.request.contextPath}/ajax/a3",{"name":$("#name").val()},function (data) {
                if(data.toString()=='ok'){//信息核对成功
                    $("#userInfo").css("color","green")
                }else{
                    $("#userInfo").css("color","red")
                }
                $("#userInfo").html(data)
            })
        }

        function a2() {
            $.post("${pageContext.request.contextPath}/ajax/a3",{"pwd":$("#pwd").val()},function (data) {
                if(data.toString()=='ok'){//信息核对成功
                    $("#pwdInfo").css("color","green")
                }else{
                    $("#pwdInfo").css("color","red")
                }
                $("#pwdInfo").html(data)
            })
        }
    </script>
</head>
<body>
<p>
    用户名：
    <input type="text" id="name" onblur="a1()"/>
    <span id="userInfo"></span>
</p>

<p>
    密码：
    <input type="text" id="pwd" onblur="a2()"/>
    <span id="pwdInfo"></span>
</p>

</body>
</html>

~~~

**【记得处理json乱码问题】，但我们此处的配置文件中使用了StringHttpMessageConverter处理乱码**

测试一下效果，动态请求响应，局部刷新，就是如此！

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627222047.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210627222100.png)

Ajax总结：

**三部曲：**

1. 编写对应处理的Controller，返回消息或者字符串或者json格式的数据

2. 编写ajax请求

	- url：Controller请求

	- data：键值对

	- success：回调函数

3. 给Ajax绑定事件，点击click，失去焦点onblur，键盘弹起Keyup

**重点：理解ajax中的data**

- **URL**是指你触发了前端的请求然后返回给后端处理是跳动到哪一个页面

- **data**是指你出发了前端的请求，并且输入了值，就一并传回给后端
