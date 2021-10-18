# Springcloud


# 1、从2.2.x和H版本开始说起

## 1.1、SpringBoot版本选择

[GitHub源码地址](https://github.com/spring-projects/spring-boot/releases/)

SpringBoot2.0新特性

- 通过上面官网发现，Boot官方强烈建议你升级到2.X以上版本

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016153757.png)

## 1.2、SpringCloud版本选择

[GitHub源码地址](https://github.com/spring-projects/spring-cloud)

[官网](https://spring.io/projects/spring-cloud)

### 1.2.1、Cloud命名规则

Spring Cloud 采用了英国伦敦地铁站的名称来命名，并由地铁站名称字母A-Z依次类推的形式来发布迭代
版本SpringCloud是一个由许多子项目组成的综合项目，各子项目有不同的发布节奏。为了管理SpringCloud与各子项目的版本依赖关系，发布了一个清单，其中包括了某个SpringCloud版本对应的子项目版本。为了避免SpringCloud版本号与子项目版本号混淆，**SpringCloud版本采用了名称而非版本号的命名，这些版本的名字采用了伦敦地铁站的名字，根据字母表的顺序来对应版本时间顺序**。例如Angel是第一个版本, Brixton是第二个版本。

当SpringCloud的发布内容积累到临界点或者一个重大BUG被解决后，会发布一个"service releases"版本，简称SRX版本，比如Greenwich.SR2就是SpringCloud发布的Greenwich版本的第2个SRX版本。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016154404.png)

## 1.3、SpringCloud和SpringBoot之间的依赖关系如何看

[官网说明](https://spring.io/projects/spring-cloud#overview)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016154648.png)

### 1.3.1、依赖

Finchley 是基于 Spring Boot 2.0.x 构建的不再 Boot 1.5.x

Dalston 和 Edgware 是基于 Spring Boot 1.5.x 构建的，不支持 Spring Boot 2.0.x

Camden 构建于 Spring Boot 1.4.x，但依然能支持 Spring Boot 1.5.x

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016154734.png)

[更详细的版本对应查看方法](https://start.spring.io/actuator/info)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016154945.png)

## 1.4、目前使用版本

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016155101.png)

### 1.4.1、题外话

- boot版本以及到2.2.4为最新(截止2020.2.15)，为什么选2.2.2
	- 只用boot，直接用最新
	- 同时用boot和cloud，需要照顾cloud，由cloud决定boot版本
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016155252.png)
	
- SpringCloud和SpringBoot版本对应关系

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016155334.png)
	
- X版本常用的组件pom

~~~xml
<dependencies>
  <!--spring boot 2.2.2-->
  <dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-dependencies</artifactId>
	<version>2.2.2.RELEASE</version>
	<type>pom</type>
	<scope>import</scope>
  </dependency>
  <!--spring cloud Hoxton.SR1-->
  <dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-dependencies</artifactId>
	<version>Hoxton.SR1</version>
	<type>pom</type>
	<scope>import</scope>
  </dependency>
  <!--spring cloud alibaba 2.1.0.RELEASE-->
  <dependency>
	<groupId>com.alibaba.cloud</groupId>
	<artifactId>spring-cloud-alibaba-dependencies</artifactId>
	<version>2.1.0.RELEASE</version>
	<type>pom</type>
	<scope>import</scope>
  </dependency>
</dependencies>
~~~
	
# 2、关于Cloud各种组件的停更/升级/替换

- 以前学习

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016155723.png)	
	
- 现在

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016155757.png)
	
[SpringCloud官方文档](https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/)
	

[SpringCloud中文文档](https://www.bookstack.cn/read/spring-cloud-docs/docs-index.md)	

[SpringBoot官方文档](https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/htmlsingle/)	

# 3、微服务架构编码构建

**约定 > 配置 > 编码**

## 3.1、IDEA新建project工作空间

1. 创建maven项目
2. 设置字符编码为`utf-8`

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016160424.png)
3. 设置注解生效激活

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016160540.png)
4. Java编译版本选择8

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016160631.png)
5. File Type过滤 **(可选)**

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016160706.png)
	
## 3.2、父工程POM

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.clover.springcloud</groupId>
    <artifactId>SpringCloud</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
 
    <!-- 统一管理jar包版本 -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.18</lombok.version>
        <mysql.version>5.1.47</mysql.version>
        <druid.version>1.1.16</druid.version>
        <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
    </properties>
 
    <!-- 子模块继承之后，提供作用：锁定版本+子modlue不用写groupId和version  -->
    <dependencyManagement>
        <dependencies>
            <!--spring boot 2.2.2-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud Hoxton.SR1-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud alibaba 2.1.0.RELEASE-->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.1.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.spring.boot.version}</version>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
            </dependency>
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
                <optional>true</optional>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                    <addResources>true</addResources>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
~~~

## 3.3、Maven工程落地细节复习

### 3.3.1、Maven中的DependencyManagement和Dependencies

Maven 使用 `dependencyManagement` 元素来提供了一种管理依赖版本号的方式。

**通常会在一个组织或者项目的最顶层的父POM 中看到 dependencyManagement 元素。**

使用 `pom.xml `中的 dependencyManagement 元素能让所有在子项目中引用一个依赖而不用显式的列出版本号。

Maven 会沿着父子层次向上走，直到找到一个拥有 dependencyManagement 元素的项目，然后它就会使用这个

`dependencyManagement` 元素中指定的版本号。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016161035.png)

	这样做的好处就是：
		如果有多个子项目都引用同一样依赖，则可以避免在每个使用的子项目里都声明一个版本号，这样当想升级或切换到另一
		个版本时，只需要在顶层父容器里更新，而不需要一个一个子项目的修改;另外如果某个子项目需要另外的一个版本，只需
		要声明version就可。

- `dependencyManagement` 里只是声明依赖，**并不实现引入**，因此子项目需要显示的声明需要用的依赖。
- **如果不在子项目中声明依赖，是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope都读取自父pom;**
- 如果子项目中指定了版本号，那么会使用子项目中指定的jar版本。

### 3.3.2、Maven中跳过单元测试

1. 配置

~~~xml
<build><!-- maven中跳过单元测试 -->
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-surefire-plugin</artifactId>
			<configuration>
				<skip>true</skip>
			</configuration>
		</plugin>
	</plugins>
</build>
~~~
2. IDEA工具支持 **(推荐)**
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016161438.png)
	
## 3.4、Rest微服务工程构建

### 3.4.1、提供者模块构建

`cloud-provider-payment8001`微服务提供者支付Module模块

1. 新建 cloud-provider-payment8001
2. 修改POM文件

~~~xml
 <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"  
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
	
	 <parent>  
 		<artifactId>SpringCloud</artifactId>  
 		<groupId>com.clover.springcloud</groupId>  
 		<version>1.0-SNAPSHOT</version>  
 	</parent>  
 	<modelVersion>4.0.0</modelVersion>  
  
 	<artifactId>cloud-provider-payment8001</artifactId>


    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <!--mysql-connector-java-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
~~~
3. 编写YML

~~~xml
server:
  port: 8001

spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包 com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springcloud?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: xn123456


mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.clover.springcloud.entities    # 所有Entity别名类所在包
~~~
4. 建立主启动类

~~~java 
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class PaymentMain8001 {

    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}

~~~
5. 业务类
6. 测试

### 3.4.2、小知识点补充

1、为什么`DAO层`传入参数时为什么使用`@Param注解`

	因为java没有保存行参的记录，java在运行的时候会把List queryAll(int offset,int limit);中的参数变成这样:
	queryAll(int arg0,int arg1),这样我们就没有办法去传递多个参数。所以需要使用@Param注解给方法参数命名，然后在xml
	文件的该dao层方法对应的sql语句中就可以正常使用@Param注解的参数名。
	还可以传入多个参数

[参考博客](https://blog.csdn.net/zijikanwa/article/details/103056857)

2、`Controller层`使用`GetMapping`时为什么使用`PathVariable注解`

	1.因为PathVariable注解用于处理动态URL
	2.在GetMapping定义的URL路径中存在变量传递，使用PathVariable注解可获取该变量的值
	3.默认情况下，Spring会对PathVariable注解的变量进行自动赋值，当然你也可以指定PathVariable注解使用哪一个URL中的变量
	4.你也可以定义多个URL变量，但是你在函数的形参中使用该注解时，必须显示的表名具体是哪个URL变量值
[参考博客](https://blog.csdn.net/qq_37370132/article/details/107972265)

3、`POST请求`时为什么要使用`RequestBody注解`

	RequestBody注解可以将请求体中的JSON字符串绑定到相应的bean上，也可以将其绑定到对应的字符串上
[参考博客1](https://blog.csdn.net/qq_35246620/article/details/59620858)
[参考博客2](https://www.jianshu.com/p/bd998a400f29)

**主实体Payment**
~~~java 
package com.clover.springcloud.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment implements Serializable
{

    private Long id;
    private String serial;
}
~~~

**Json封装体CommonResult**
~~~java 
package com.clover.springcloud.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/*
 * 返回给前端人员的数据，实体类是供给我们后端人员使用的
 * 给前端人员只需要将状态与数据返回过去给其就行
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T>
{
    private Integer code;
    private String message;
    private  T data;

    public CommonResult(Integer code,String message)
    {
        this(code,message,null);
    }
}
~~~

**接口PaymentDao**
~~~java 
package com.clover.springcloud.dao;

import com.clover.springcloud.entities.Payment;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

@Mapper
public interface PaymentDao
{
    public int create(Payment payment);

    public Payment getPaymentById(@Param("id") Long id);
}
~~~

**映射文件PaymentMapper.xml**
~~~java 
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

<mapper namespace="com.clover.springcloud.dao.PaymentDao">

    <resultMap id="BaseResultMap" type="Payment">
        <id column="id" property="id" jdbcType="BIGINT"/>
        <result column="serial" property="serial" jdbcType="VARCHAR"/>
    </resultMap>

    <insert id="create" parameterType="Payment" useGeneratedKeys="true" keyProperty="id">
        insert into payment(serial) values(#{serial})
    </insert>

    <select id="getPaymentById" parameterType="Long" resultMap="BaseResultMap">
        select * from payment where id = #{id}
    </select>
</mapper>
~~~

**接口PaymentService**
~~~java 
package com.clover.springcloud.service;

import com.clover.springcloud.entities.Payment;
import org.apache.ibatis.annotations.Param;

public interface PaymentService
{
    public int create(Payment payment);

    public Payment getPaymentById(@Param("id") Long id);
}
~~~

**实现类**
~~~java 
package com.clover.springcloud.service.Impl;

import com.clover.springcloud.dao.PaymentDao;
import com.clover.springcloud.entities.Payment;
import com.clover.springcloud.service.PaymentService;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class PaymentServiceImpl implements PaymentService {

    @Resource
    public PaymentDao paymentDao;

    @Override
    public int create(Payment payment) {
        return paymentDao.create(payment);
    }

    @Override
    public Payment getPaymentById(Long id) {
        return paymentDao.getPaymentById(id);
    }
}
~~~

**controller**
~~~java 
package com.clover.springcloud.controller;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import com.clover.springcloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {

    @Resource
    public PaymentService paymentService;

    @PostMapping(value = "/payment/create")
    public CommonResult create(@RequestBody Payment payment)
    {
        int result = paymentService.create(payment);
        log.info("********插入操作结果："+ result);
        if (result > 0){
            return new CommonResult(200,"插入数据成功",result);
        }else {
            return new CommonResult(404,"插入数据失败",null);
        }
    }

    @GetMapping(value = "/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id)
    {
        Payment payment = paymentService.getPaymentById(id);
        log.info("********查询结果："+ payment);
        if (payment !=null){
            return new CommonResult(200,"查询成功",payment);
        }else {
            return new CommonResult(404,"查询失败",null);
        }
    }
}
~~~

### 3.4.3、消费者模块构建

`cloud-consumer-order80`微服务消费者订单Module模块

1. 新建 cloud-consumer-order80 模块
2. 改写POM

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>SpringCloud</artifactId>
        <groupId>com.clover.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consumer-order80</artifactId>


    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
~~~
3. 编写YML

~~~yml
server:
  port: 80
~~~
4. 构建主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class OrderMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain80.class,args);
    }
}
~~~
5. 业务类
6. 测试

### 3.4.4、小知识点补充

1、首说RestTemplate是什么？

	RestTemplate提供了多种便捷访问远程Http服务的方法， 
	是一种简单便捷的访问restful服务模板类，是Spring提供的用于访问Rest服务的客户端模板工具集

2、使用

	使用restTemplate访问restful接口非常的简单粗暴无脑。(url, requestMap, ResponseBean.class)这三个参数分别代表
	REST请求地址、请求参数、HTTP响应转换被转换成的对象类型。

[RestTemplate官网](https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html)

**业务类：**

**主实体Payment**
~~~java 
package com.clover.springcloud.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment implements Serializable
{

    private Long id;
    private String serial;
}
~~~

**Json封装体CommonResult**
~~~java 
package com.clover.springcloud.entities;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/*
 * 返回给前端人员的数据，实体类是供给我们后端人员使用的
 * 给前端人员只需要将状态与数据返回过去给其就行
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T>
{
    private Integer code;
    private String message;
    private  T data;

    public CommonResult(Integer code,String message)
    {
        this(code,message,null);
    }
}
~~~

**config配置类**
~~~java 
package com.clover.springcloud.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {

    @Bean
    public RestTemplate restTemplate()
    {
        return new RestTemplate();
    }
}
~~~

**controller**
~~~java 
package com.clover.springcloud.controller;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderController {
    public static final String Payment_URL = "http://localhost:8001";

    @Resource
    private RestTemplate restTemplate;

    //客户端用浏览器是get请求，但是底层实质发送post调用服务端8001
    @GetMapping("/consumer/payment/create")
    public CommonResult create(Payment payment)
    {
        return restTemplate.postForObject(Payment_URL + "/payment/create",payment,CommonResult.class);
    }

    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult getPaymentById(@PathVariable("id") Long id)
    {
        return restTemplate.getForObject(Payment_URL + "/payment/get/" + id,CommonResult.class);
    }
}
~~~

	注意：
	在提供者的Controller中创建一个payment的时候，不要忘记了RequestBody注解，否则会造成数据库插入成功，但是数据没有插入进去

## 3.5、工程重构

**发现问题：项目中有重复部分，重构**

1. 新建 cloud-api-commons 模块
2. 修改POM

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>SpringCloud</artifactId>
        <groupId>com.clover.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-api-commons</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.1.0</version>
        </dependency>
    </dependencies>

</project>
~~~
3. 将entities复制过来
4. 执行maven的 clean 和 install 命令
5. 改造订单80和支付8001
	1. 删除原有的entities文件夹
	2. 各自粘贴POM内容
~~~xml
<dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
    <groupId>com.clover.springcloud</groupId>
    <artifactId>cloud-api-commons</artifactId>
    <version>${project.version}</version>
</dependency>
~~~


# 4、Eureka服务注册与发现

## 4.1、Eureka基础知识

### 4.1.1、什么是服务治理

SpringCloud 封装了 Netflix 公司开发的 Eureka 模块来**实现服务治理**

在传统的rpc远程调用框架中，管理每个服务与服务之间依赖关系比较复杂，管理比较复杂，所以需要使用服务治理，管理服务与服务之间依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现与注册。

### 4.1.2、什么是服务注册

Eureka 采用了CS的设计架构，Eureka Server 作为服务注册功能的服务器，它是服务注册中心。而系统中的其他微服务，使用 Eureka 的客户端连接到 Eureka Server 并维持心跳连接。这样系统的维护人员就可以通过 Eureka Server 来监控系统中各个微服务是否正常运行。

在服务注册与发现中，有一个注册中心。当服务器启动的时候，会把当前自己服务器的信息,比如 服务地址、通讯地址等以别名方式注册到注册中心上。另一方（消费者|服务提供者），以该别名的方式去注册中心上获取到实际的服务通讯地址，然后再实现本地RPC调用RPC远程框架核心设计思想：在于注册中心，因为使用注册中心管理每个服务与服务之间的一个依赖关系(服务治理概念)。在任何RPC远程框架中，都会有一个注册中心(存放服务地址相关信息(接口地址))

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017112259.png)

### 4.1.3、Eureka两组件

**Eureka Server**提供服务注册服务

各个微服务节点通过配置启动后，会在EurekaServer中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。

**EurekaClient**通过注册中心进行访问

是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置的、使用轮询(round-robin)负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳(默认周期为30秒)。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除（默认90秒）

## 4.2、单机Eureka构建步骤

### 4.2.1、IDEA生成eurekaServer端服务注册中心类似物业公司

1. 新建 springcloud-eureka-server7001 模块
2. 修改POM

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>SpringCloud</artifactId>
        <groupId>com.clover.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springcloud-eureka-server7001</artifactId>


    <dependencies>
        <!--eureka-server-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        <!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
        <dependency>
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <!--boot web actuator-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
    </dependencies>

</project>
~~~
3. 编写YML

~~~yml
server:
  port: 7001

eureka:
  instance:
    hostname: localhost #eureka服务端的实例名称
  client:
    #false表示不向注册中心注册自己。
    register-with-eureka: false
    #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址。
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
~~~
4. 新建主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7001 {
    public static void main(String[] args)
    {
        SpringApplication.run(EurekaMain7001.class,args);
    }
}
~~~

### 4.2.2、EurekaClient端cloud-provider-payment8001

将其注册进 EurekaServer 成为服务提供者provider，类似尚硅谷学校对外提供授课服务

1. 改写POM

~~~xml
<!--eureka-client-->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
~~~
2. 改写YML

~~~yml
eureka:
  client:
    #表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
~~~
3. 改变主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class PaymentMain8001 {

    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}
~~~


测试：

1、**先要启动EurekaServer**

2、访问 http://localhost:7001

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017194840.png)

3、微服务注册名配置说明

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017195005.png)

`自我保护机制`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017200238.png)

### 4.2.3、EurekaClient端cloud-consumer-order80

将其注册进 EurekaServer 成为服务消费者consumer，类似来尚硅谷上课消费的各位同学

1. 修改POM

~~~xml
<!--eureka-client-->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
~~~
2. 修改YML

~~~yml
spring:
  application:
    name: cloud-order-service

eureka:
  client:
    #表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
~~~
3. 修改主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class OrderMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain80.class,args);
    }
}
~~~

测试：

1、**先要启动EurekaServer，7001服务，再启动服务提供者provider，8001服务**

2、再启动80服务


## 4.3、集群Eureka构建步骤

### 4.3.1、Eureka集群原理说明

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017201223.png)

`EurekaServer 集群是互相注册，相互守望`

问题：微服务RPC远程服务调用最核心的是什么 

	高可用，试想你的注册中心只有一个only one， 它出故障了那就呵呵(￣▽￣)"了，会导致整个为服务环境不可用，所以
	解决办法：搭建Eureka注册中心集群 ，实现负载均衡+故障容错
	
### 4.3.2、EurekaServer集群环境构建步骤

1、参考 springcloud-eureka-server7001 新建 springcloud-eureka-server7002

2、修改POM

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>SpringCloud</artifactId>
        <groupId>com.clover.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springcloud-eureka-server7002</artifactId>


    <dependencies>
        <!--eureka-server-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        <!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
        <dependency>
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <!--boot web actuator-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
    </dependencies>

</project>
~~~

3、修改映射配置
-	找到C:\Windows\System32\drivers\etc路径下的hosts文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017204314.png)
-	修改映射配置添加进hosts文件
	-	127.0.0.1  eureka7001.com
	-	127.0.0.1  eureka7002.com

`如果你的用户无法编辑host文件`：[参考博客](https://blog.csdn.net/yuanlaijike/article/details/79668711)

4、改写以前的YML **(以前单机)**

5、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7002 {
    public static void main(String[] args)
    {
        SpringApplication.run(EurekaMain7002.class,args);
    }

}
~~~

### 4.3.3、将支付服务8001微服务发布到上面2台Eureka集群配置中

~~~yml
server:
  port: 8001

spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包 com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springcloud?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: xn123456

eureka:
  client:
    #表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      #defaultZone: http://localhost:7001/eureka # 单机版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  # 集群版

mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.clover.springcloud.entities    # 所有Entity别名类所在包
~~~

### 4.3.4、将订单服务80微服务发布到上面2台Eureka集群配置中

~~~yml
server:
  port: 80

spring:
  application:
    name: cloud-order-service

eureka:
  client:
    #表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      #defaultZone: http://localhost:7001/eureka # 单机版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  # 集群版
~~~

测试

1、**先要启动EurekaServer，7001/7002服务**

2、**再要启动服务提供者provider，8001**

3、**再要启动消费者，80**

4、最后用查询验证是否正确：http://localhost/consumer/payment/get/1


### 4.3.5、支付服务提供者8001集群环境构建

1、参考 cloud-provider-payment8001 新建 cloud-provider-payment8002

2、修改POM

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>SpringCloud</artifactId>
        <groupId>com.clover.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8002</artifactId>


    <dependencies>
        <!--eureka-client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <!--mysql-connector-java-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
~~~

3、编写YML

~~~yml
server:
  port: 8002

spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包 com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springcloud?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: xn123456

eureka:
  client:
    #表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      #defaultZone: http://localhost:7001/eureka # 单机版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  # 集群版

mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.clover.springcloud.entities    # 所有Entity别名类所在包
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class PaymentMain8002 {

    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8002.class,args);
    }
}
~~~

5、业务类：直接从8001copy

6、修改8001/8002的Controller

~~~Java
package com.clover.springcloud.controller;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import com.clover.springcloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {

    @Resource
    public PaymentService paymentService;

    @Value("${server.port}")
    private String serverPort;

    @PostMapping(value = "/payment/create")
    public CommonResult create(@RequestBody Payment payment)
    {
        int result = paymentService.create(payment);
        log.info("********插入操作结果："+ result);
        if (result > 0){
            return new CommonResult(200,"插入数据成功，serverPort：" + serverPort,result);
        }else {
            return new CommonResult(404,"插入数据失败",null);
        }
    }

    @GetMapping(value = "/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id)
    {
        Payment payment = paymentService.getPaymentById(id);
        log.info("********查询结果："+ payment);
        if (payment !=null){
            return new CommonResult(200,"查询成功，serverPort：" + serverPort,payment);
        }else {
            return new CommonResult(404,"查询失败",null);
        }
    }
}
~~~

~~~Java
package com.clover.springcloud.controller;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import com.clover.springcloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {

    @Resource
    public PaymentService paymentService;

    @Value("${server.port}")
    private String serverPort;

    @PostMapping(value = "/payment/create")
    public CommonResult create(@RequestBody Payment payment)
    {
        int result = paymentService.create(payment);
        log.info("********插入操作结果："+ result);
        if (result > 0){
            return new CommonResult(200,"插入数据成功，serverPort：" + serverPort,result);
        }else {
            return new CommonResult(404,"插入数据失败",null);
        }
    }

    @GetMapping(value = "/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id)
    {
        Payment payment = paymentService.getPaymentById(id);
        log.info("********查询结果："+ payment);
        if (payment !=null){
            return new CommonResult(200,"查询成功，serverPort：" + serverPort,payment);
        }else {
            return new CommonResult(404,"查询失败",null);
        }
    }
}
~~~

### 4.3.6、负载均衡

1. 订单服务访问地址不能写死

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017213252.png)

2. **使用@LoadBalanced注解赋予RestTemplate负载均衡的能力**
3. 修改ApplicationContextBean

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017213316.png)

测试：

1、**先要启动EurekaServer，7001/7002服务**

2、**再要启动服务提供者provider，8001/8002服务**

3、最后用查询验证是否正确：http://localhost/consumer/payment/get/1

4、结果

- 负载均衡效果达到
- 8001/8002端口交替出现

5、Ribbon和Eureka整合后Consumer可以直接调用服务而不用再关心地址和端口号，且该服务还有负载功能了。

## 4.4、actuator微服务信息完善

### 4.4.1、主机名称:服务名称修改

1. 当前问题

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017214651.png)

2. 修改cloud-provider-payment8001/8002

~~~yml
server:
  port: 8001

spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包
    url: jdbc:mysql://localhost:3306/db2019?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456

eureka:
  client:
    #表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  # 集群版
      #defaultZone: http://localhost:7001/eureka  # 单机版
  instance:
    instance-id: payment8001


mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.atguigu.springcloud.entities    # 所有Entity别名类所在包
~~~
3. 修改之后

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017215016.png)

### 4.4.2、访问信息有IP信息提示

1. 当前问题：**没有IP提示**
2. 修改cloud-provider-payment8001/8002

~~~yml
server:
  port: 8001

spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包
    url: jdbc:mysql://localhost:3306/db2019?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456

eureka:
  client:
    #表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  # 集群版
      #defaultZone: http://localhost:7001/eureka  # 单机版
  instance:
    instance-id: payment8001
    prefer-ip-address: true     #访问路径可以显示IP地址

mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.atguigu.springcloud.entities    # 所有Entity别名类所在包
~~~
3. 修改之后

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017215122.png)

## 4.5、服务发现Discovery

**对于注册进eureka里面的微服务，可以通过服务发现来获得该服务的信息**

1、修改 cloud-provider-payment8001 的Controller

~~~Java
package com.clover.springcloud.controller;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import com.clover.springcloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;
import java.util.List;

@RestController
@Slf4j
public class PaymentController {

    @Resource
    public PaymentService paymentService;

    @Value("${server.port}")
    private String serverPort;

    @Resource
    private DiscoveryClient discoveryClient;

    @PostMapping(value = "/payment/create")
    public CommonResult create(@RequestBody Payment payment)
    {
        int result = paymentService.create(payment);
        log.info("********插入操作结果："+ result);
        if (result > 0){
            return new CommonResult(200,"插入数据成功，serverPort：" + serverPort,result);
        }else {
            return new CommonResult(404,"插入数据失败",null);
        }
    }

    @GetMapping(value = "/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id)
    {
        Payment payment = paymentService.getPaymentById(id);
        log.info("********查询结果："+ payment);
        if (payment !=null){
            return new CommonResult(200,"查询成功，serverPort：" + serverPort,payment);
        }else {
            return new CommonResult(404,"查询失败",null);
        }
    }

    @GetMapping(value = "/payment/discovery")
    public Object discovery()
    {
        // 用于查询Eureka上有几个服务
        List<String> services = discoveryClient.getServices();
        for (String element: services) {
            log.info("***************element:" + element);
        }

        // 用于查询某一个名称下的实例
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        for (ServiceInstance element: instances) {
            log.info(element.getServiceId() + "\t" + element.getHost() + "\t" + element.getPort() + "\t" + element.getUri());
        }

        return this.discoveryClient;
    }
}
~~~

2、在8001主启动类上加上`@EnableDiscoveryClient注解`

3、测试

- **先启动EurekaServer**
- **再启动8001主启动类**
- 最后用查询验证是否正确：http://localhost:8001/payment/discovery

 ## 4.6、Eureka自我保护机制
 
 ### 4.6.1、故障现象
 
 #### 4.6.1.1、概述
 
保护模式主要用于一组客户端和Eureka Server之间存在网络分区场景下的保护。一旦进入保护模式，
**Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据，也就是不会注销任何微服务**。

如果在Eureka Server的首页看到以下这段提示，则说明Eureka进入了保护模式：

EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. 
RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE 

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018110517.png)

### 4.6.2、导致原因

#### 4.6.2.1、为什么会产生Eureka自我保护机制？

为了防止EurekaClient可以正常运行，但是与 EurekaServer 网络不通情况下，EurekaServer**不会立刻**将EurekaClient服务剔除


#### 4.6.2.2、什么是自我保护模式

默认情况下，如果EurekaServer在一定时间内没有接收到某个微服务实例的心跳，EurekaServer将会注销该实例（默认90秒）。但是`当网络分区故障发生(延时、卡顿、拥挤)时`，微服务与EurekaServer之间无法正常通信，以上行为可能变得非常危险了——因为微服务本身其实是健康的，**此时本不应该注销这个微服务**。Eureka通过“自我保护模式”来解决这个问题——当EurekaServer节点在短时间内丢失过多客户端时（可能发生了网络分区故障），那么这个节点就会进入自我保护模式。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018110839.png)

**在自我保护模式中，EurekaServer会保护服务注册表中的信息，不再注销任何服务实例。**

它的设计哲学就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。**一句话讲解：好死不如赖活着**


综上，自我保护模式是一种应对网络异常的安全保护措施。它的架构哲学是宁可同时保留所有微服务（健康的微服务和不健康的微服务都会保留）也不盲目注销任何健康的微服务。使用自我保护模式，可以让Eureka集群更加的健壮、稳定。

`一句话：某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对该微服务的信息进行保存`

### 4.6.3、怎么禁止自我保护


#### 4.6.3.1、修改注册中心eureakeServer端7001

1、出厂默认，自我保护机制是开启的`eureka.server.enable-self-preservation=true`

2、使用`eureka.server.enable-self-preservation = false` 可以禁用自我保护模式

~~~yml
server:
  port: 7001


eureka:
  instance:
    hostname: eureka7001.com #eureka服务端的实例名称
  client:
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url:
      #defaultZone: http://eureka7002.com:7002/eureka/
      defaultZone: http://eureka7001.com:7001/eureka
  server:
    #关闭自我保护机制，保证不可用服务被及时踢除
    enable-self-preservation: false
    eviction-interval-timer-in-ms: 2000
~~~

3、关闭效果

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018112826.png)



#### 4.6.3.2、修改生产者客户端eureakeClient端8001

1、默认设置
`eureka.instance.lease-renewal-interval-in-seconds=30`单位为秒(默认是30秒)
 `eureka.instance.lease-expiration-duration-in-seconds=90`单位为秒(默认是90秒)
 
 2、配置
 
 ~~~yml
 server:
  port: 8001

spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包 com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springcloud?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: xn123456

eureka:
  client:
    #表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka # 单机版
      #defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka  # 集群版
  #心跳检测与续约时间
  #开发时设置小些，保证服务关闭后注册中心能及时剔除服务
  instance:
    instance-id: payment8001
    prefer-ip-address: true     #访问路径可以显示IP地址
    #Eureka客户端向服务端发送心跳的时间间隔，单位为秒(默认是30秒)
    lease-renewal-interval-in-seconds: 1
    #Eureka服务端在收到最后一次心跳后等待时间上限，单位为秒(默认是90秒)，超时将剔除服务
    lease-expiration-duration-in-seconds: 2

mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: com.clover.springcloud.entities    # 所有Entity别名类所在包
 ~~~
 
 3、测试
 - 7001和8001都配置完成
 - 先启动7001再启动8001
 - 先关闭8001，马上就被删除了

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018113352.png)

