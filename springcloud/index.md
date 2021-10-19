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


# 5、Zookeeper服务注册与发现

## 5.1、SpringCloud整合Zookeeper代替Eureka

### 5.1.1、注册中心Zookeeper

- zookeeper是一个分布式协调工具，可以实现注册中心功能
- 关闭Linux服务器防火墙后启动zookeeper服务器
- zookeeper服务器取代Eureka服务器，zk作为服务注册中心

### 5.1.2、服务提供者

1、新建cloud-provider-payment8004

2、修改POM

~~~xml
<dependencies>
        <!-- SpringBoot整合Web组件 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <!-- SpringBoot整合zookeeper客户端 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
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
3、编写YML

~~~yml
#8004表示注册到zookeeper服务器的支付服务提供者端口号
server:
  port: 8004

#服务别名----注册zookeeper到注册中心名称
spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      connect-string: 192.168.167.48:2181
~~~
4、创建主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient //该注解用于向使用consul或者zookeeper作为注册中心时注册服务
public class PaymentMain8004 {
    public static void main(String[] args)
    {
        SpringApplication.run(PaymentMain8004.class,args);
    }

}
~~~
5、编写Controller

~~~Java
package com.clover.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.UUID;

@RestController
@Slf4j
public class PaymentController {
    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/zk")
    public String paymentzk()
    {
        return "springcloud with zookeeper:" + serverPort + "\t" + UUID.randomUUID().toString();
    }
}
~~~
6、启动8004注册进zookeeper
- 启动后问题
	- 成功关闭了Linux防火墙时出现的问题
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018163705.png)
	- 如果你没有关闭防火墙就运行，会出现该报错
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018161906.png)

7、why？
- 解决zookeeper版本jar包冲突问题
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018161954.png)
- 排出zk冲突后的新POM
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

    <artifactId>cloud-provider-payment8004</artifactId>


    <dependencies>
        <!-- SpringBoot整合Web组件 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <!-- SpringBoot整合zookeeper客户端 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
            <!--先排除自带的zookeeper3.5.3-->
            <exclusions>
                <exclusion>
                    <groupId>org.apache.zookeeper</groupId>
                    <artifactId>zookeeper</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!--添加zookeeper3.4.9版本-->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.4.9</version>
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


8、验证测试1：http://localhost:8004/payment/zk

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018164421.png)

9、验证测试2：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018164305.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018164355.png)

	注意：如果你运行的时候出现该错误，说明你没有关闭Linux上的防火墙导致的

[参考博客](https://blog.csdn.net/zjy15203167987/article/details/79029934)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018162922.png)

10、思考：`服务节点是临时节点还是持久节点？`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018170107.png)

	说明这是一个临时结点，当你关闭了提供者后，zookeeper会保留一段时间后，在去除，而当你在启动提供者时，
	它会生成一个新的序列号
	
### 5.1.3、服务消费者

1、新建cloud-consumerzk-order80

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

    <artifactId>cloud-consumerzk-order80</artifactId>


    <dependencies>
        <!-- SpringBoot整合Web组件 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <!-- SpringBoot整合zookeeper客户端 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
            <!--先排除自带的zookeeper-->
            <exclusions>
                <exclusion>
                    <groupId>org.apache.zookeeper</groupId>
                    <artifactId>zookeeper</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!--添加zookeeper3.4.9版本-->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.4.9</version>
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

3、修改YML

~~~yml
server:
  port: 80

spring:
  application:
    name: cloud-consumer-order
  cloud:
    #注册到zookeeper地址
    zookeeper:
      connect-string: 192.168.167.48:2181
~~~

4、编写主启动类

~~~java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class OrderZKMain80 {
    public static void main(String[] args)
    {
        SpringApplication.run(OrderZKMain80.class,args);
    }
}
~~~

5、编写业务类
- 配置Bean

~~~Java
package com.clover.springcloud.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {

    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate()
    {
        return new RestTemplate();
    }
}
~~~
- Controller

~~~Java
package com.clover.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderZKController {

    private static final String INVOKE_URL = "http://cloud-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/zk")
    public String paymentInfo()
    {
        String result = restTemplate.getForObject(INVOKE_URL + "/payment/zk", String.class);

        log.info("消费者调用支付服务(zookeeper)--->result:" + result);

        return result;
    }
}
~~~


6、验证测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018172445.png)

7、访问测试地址

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018172512.png)

# 6、Consul服务注册与发现

## 6.1、Consul简介

1. Consul是什么？
[官网](https://www.consul.io/intro/index.html)
2. Consul能干嘛？
	- 服务发现：提供HTTP和DNS两种发现方式。
	- 健康监测：支持多种方式，HTTP、TCP、Docker、Shell脚本定制化监控
	- KV存储：Key、Value的存储方式
	- 多数据中心：Consul支持多数据中心
	- 可视化Web界面
3. Consul去哪下？
[下载地址](https://www.consul.io/downloads.html)
4. Consul怎么玩？
[Consul中文文档](https://www.springcloud.cc/spring-cloud-consul.html)


## 6.2、安装并运行Consul

[官网安装说明](https://learn.hashicorp.com/consul/getting-started/install.html)

下载完成后只有一个consul.exe文件，硬盘路径下双击运行，查看版本号信息

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018204826.png)

使用开发模式启动
- consul agent -dev
- 通过以下地址可以访问Consul的首页：http://localhost:8500
- 结果页面
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018205022.png)]

## 6.3、服务提供者

1、新建 cloud-providerconsul-payment8006

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

    <artifactId>cloud-providerconsul-payment8006</artifactId>


    <dependencies>
        <!--SpringCloud consul-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>
        <!-- SpringBoot整合Web组件 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--日常通用jar包配置-->
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
###consul服务端口号
server:
  port: 8006

spring:
  application:
    name: consul-provider-payment
  ####consul注册中心地址
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        #hostname: 127.0.0.1
        service-name: ${spring.application.name}
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain8006 {
    public static void main(String[] args)
    {
        SpringApplication.run(PaymentMain8006.class,args);
    }
}
~~~

5、编写Controller

~~~Java
package com.clover.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.UUID;

@RestController
@Slf4j
public class PaymentController {
    @Value("${server.port}")
    private String serverPort;

    @GetMapping("/payment/consul")
    public String paymentInfo()
    {
        return "springcloud with consul: "+serverPort+"\t\t"+ UUID.randomUUID().toString();
    }
}
~~~

6、验证测试：http://localhost:8006/payment/consul

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018212259.png)

## 6.4、服务消费者

1、新建 cloud-consumerconsul-order80

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

    <artifactId>cloud-consumerconsul-order80</artifactId>


    <dependencies>
        <!--SpringCloud consul-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>
        <!-- SpringBoot整合Web组件 -->
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

3、编写YML

~~~yml
###consul服务端口号
server:
  port: 80

spring:
  application:
    name: consul-consumer-order
  ####consul注册中心地址
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        #hostname: 127.0.0.1
        service-name: ${spring.application.name}
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient //该注解用于向使用consul或者zookeeper作为注册中心时注册服务
public class OrderConsulMain80 {
    public static void main(String[] args)
    {
        SpringApplication.run(OrderConsulMain80.class,args);
    }
}
~~~

5、配置Bean

~~~Java
package com.clover.springcloud.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {

    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate()
    {
        return new RestTemplate();
    }
}
~~~

6、编写Controller

~~~Java
package com.clover.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderConsulController {
    private static final String INVOKE_URL = "http://consul-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/consul")
    public String paymentInfo()
    {
        String result = restTemplate.getForObject(INVOKE_URL + "/payment/consul", String.class);

        log.info("消费者调用支付服务(consul)--->result:" + result);

        return result;
    }
}
~~~

7、验证测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018213820.png)

8、访问测试地址：http://localhost/consumer/payment/consul

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018213847.png)


## 6.5、三个注册中心异同点

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018215113.png)

１、CAP

- **C:Consistency（强一致性）**
- **A:Availability（可用性）**
- **P:Partition tolerance（分区容错性）**
- CAP理论关注粒度是数据，而不是整体系统设计的策略

２、经典CAP图

**最多只能同时较好的满足两个。**

CAP理论的核心是：`一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，`

因此，根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则和满足 AP 原则

三大类：
- CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。
- CP -  满足一致性，分区容忍必的系统，通常性能不是特别高。
- AP -  满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。

1. **AP(Eureka)**
	- AP架构

	当网络分区出现后，为了保证可用性，系统B**可以返回旧值**，保证系统的可用性。

		结论：违背了一致性C的要求，只满足可用性和分区容错，即AP

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018220001.png)

2. **CP(Zookeeper/Consul)**
	- CP架构
	
	当网络分区出现后，为了保证一致性，就必须拒接请求，否则无法保证一致性
	
		结论：违背了可用性A的要求，只满足一致性和分区容错，即CP

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018215844.png)


# 7、Ribbon负载均衡服务调用

## 7.1、概述

1. Ribbon是什么？

	- SpringCloud Ribbon是基于Netflix Ribbon实现的**一套客户端**       **负载均衡的工具**。
	- 简单的说，Ribbon是Netflix发布的开源项目，主要功能是提供**客户端的软件负载均衡算法和服务调用**。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，就是在配置文件中列出`LoadBalancer（简称LB）`后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器。我们很容易使用Ribbon实现自定义的负载均衡算法

2. 官网资料

	[官方资料](https://github.com/Netflix/ribbon/wiki/Getting-Started)
	
	Ribbon目前也进入维护模式
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019102224.png)
	
	未来替换方案
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019102313.png)

3. 能干吗？

	- LB(负载均衡)
		- 集中式LB
			- 即在服务的消费方和提供方之间使用独立的LB设施(可以是硬件，如F5, 也可以是软件，如nginx), 由该设施负责把访问请求通过某种策略转发至服务的提供方
		- 进程内LB
			- 将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择出一个合适的服务器
			- **Ribbon就属于进程内LB**，它只是一个类库，**集成于消费方进程**，消费方通过它来获取到服务提供方的地址
	- 前面我们讲解过了80通过轮询负载访问8001/8002
	- 一句话：**负载均衡+RestTemplate调用**

 
**LB负载均衡(Load Balance)是什么**

简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA（高可用）。
常见的负载均衡有软件Nginx，LVS，硬件 F5等。
 
**Ribbon本地负载均衡客户端 VS Nginx服务端负载均衡区别**

 Nginx是服务器负载均衡，客户端所有请求都会交给nginx，然后由nginx实现转发请求。即负载均衡是由服务端实现的。
 
 Ribbon本地负载均衡，在调用微服务接口时候，会在注册中心上获取注册信息服务列表之后缓存到JVM本地，从而在本地实现RPC远程服务调用技术。
 
 
 ## 7.2、Ribbon负载均衡演示
 
 ### 7.2.1、架构说明
 
 ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019104119.png)
 
 Ribbon在工作时分成两步
- 第一步先选择 EurekaServer ,它优先选择在同一个区域内负载较少的server.
- 第二步再根据用户指定的策略，在从server取到的服务注册列表中选择一个地址。

其中Ribbon提供了多种策略：比如轮询、随机和根据响应时间加权。
 
 `总结`：**Ribbon其实就是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和
 eureka结合只是其中的一个实例。**
 
 
### 7.2.2、POM

之前写`cloud-consumer-order80`样例时候没有引入spring-cloud-starter-ribbon也可以使用ribbon

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
~~~

猜测spring-cloud-starter-netflix-eureka-client自带了spring-cloud-starter-ribbon引用，

证明如下： `可以看到spring-cloud-starter-netflix-eureka-client 确实引入了Ribbon`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019104718.png)

### 7.2.3、二说RestTemplate的使用

[官网](https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html)

#### 7.2.3.1、getForObject方法/getForEntity方法

**返回对象为响应体中数据转化成的对象，基本上可以理解为Json**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019105055.png)

**返回对象为ResponseEntity对象，包含了响应中的一些重要信息，比如响应头，状态码、响应体等**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019110324.png)

#### 7.2.3.2、postForObject/postForEntity方法

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019110504.png)

## 7.3、Ribbon核心组件IRule

### 7.3.1、IRule：根据特定算法中从服务列表中选取一个要访问的服务

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019110952.png)

`com.netflix.loadbalancer.RoundRobinRule`：**轮询**

`com.netflix.loadbalancer.RandomRule`：**随机**

`com.netflix.loadbalancer.RetryRule`：**先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重试，获取可用的服务**

`WeightedResponseTimeRule`：**对RoundRobinRule的扩展，响应速度越快的实例选择权重越大，越容易被选择**

`BestAvailableRule`：**会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务**

`AvailabilityFilteringRule`：**先过滤掉故障实例，再选择并发较小的实例**

`ZoneAvoidanceRule`：**默认规则,复合判断server所在区域的性能和server的可用性选择服务器**

### 7.3.2、如何替换

1、修改cloud-consumer-order80

2、注意配置细节

**官方文档明确给出了警告：**
- 这个自定义配置类不能放在`@ComponentScan所扫描的当前包下以及子包下`，
- 否则我们自定义的这个配置类就会被所有的Ribbon客户端所共享，达不到特殊化定制的目的了。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019111846.png)

3、新建package ：com.clover.myrule

4、上面包下新建MySelfRule规则类

~~~Java
package com.clover.myrule;

import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MySelfRule
{
    @Bean
    public IRule myRule()
    {
        return new RandomRule();//定义为随机
    }
}
~~~

5、**主启动类添加@RibbonClient**

~~~Java
package com.clover.springcloud;

import com.clover.myrule.MySelfRule;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

@SpringBootApplication
@EnableEurekaClient
@EnableDiscoveryClient
@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration= MySelfRule.class)
public class PaymentMain8001 {

    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}
~~~

6、测试：http://localhost/consumer/payment/get/1

## 7.4、Ribbon负载均衡算法

### 7.4.1、原理

负载均衡算法：

	rest接口第几次请求数 % 服务器集群总数量 = 实际调用服务器位置下标  ，每次服务重启动后rest接口计数从1开始。
 
List<> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
 
如：   
	List [0] instances = 127.0.0.1:8002
　List [1] instances = 127.0.0.1:8001
 
8001+ 8002 组合成为集群，它们共计2台机器，集群总数为2， 按照轮询算法原理：
 
当总请求数为1时： 1 % 2 =1 对应下标位置为1 ，则获得服务地址为127.0.0.1:8001

当总请求数位2时： 2 % 2 =0 对应下标位置为0 ，则获得服务地址为127.0.0.1:8002

当总请求数位3时： 3 % 2 =1 对应下标位置为1 ，则获得服务地址为127.0.0.1:8001

当总请求数位4时： 4 % 2 =0 对应下标位置为0 ，则获得服务地址为127.0.0.1:8002

如此类推......

 
### 7.4.2、手写一个本地负载均衡器

1、先启动7001/7002集群

2、8001/8002微服务改造

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

    @GetMapping(value = "/payment/lb")
    public String getPaymentLB()
    {
        return serverPort;
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

    @GetMapping(value = "/payment/lb")
    public String getPaymentLB()
    {
        return serverPort;
    }

}
~~~

3、80订单微服务改造

1. ApplicationContextBean 去掉`注解@LoadBalanced`
2. 编写` LoadBalancer 接口`

~~~interface
package com.clover.springcloud.lb;

import org.springframework.cloud.client.ServiceInstance;

import java.util.List;

public interface LoadBalancer {
    ServiceInstance instences(List<ServiceInstance> serviceInstances);
}
~~~
3. 实现 LoadBalancer 接口的实现类

~~~Java
package com.clover.springcloud.lb;

import org.springframework.cloud.client.ServiceInstance;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

@Component
public class MyLB implements LoadBalancer {

    private AtomicInteger atomicInteger = new AtomicInteger(0);

    public final int getAndIncrement()
    {
        int current;
        int next;

        /*
         * CAS是Compare And Set的一个简称，如下理解
         * 1、一致当前内存里面的值current和预期要修改成的值next传入
         * 2、内存中 AtomicInteger 对象地址对应的真实值(因为有可能被修改)real与current对比，
         *    相等标识real违被修改过，是"安全"的，将next赋给real结束然后返回
         *    不相等说明real以及被修改，结束并重新执行1知道修改成功
         * CAS相比 Synchronized，避免了锁的使用，总体性能比 Synchronized 高很多
         */
        do{
            // 获取当前值
            current = this.atomicInteger.get();
            // 设置期望值
            next = current >= 2147483647 ? 0 : current + 1;
        }while (!this.atomicInteger.compareAndSet(current,next));//调用Native方法compareAndSet，执行CAS操作
        System.out.println("************第几次出现：next" + next);
        return next;
    }



    @Override
    public ServiceInstance instences(List<ServiceInstance> serviceInstances) {
        int index = getAndIncrement() % serviceInstances.size();
        return serviceInstances.get(index);
    }
}
~~~
4. 修改 OrderController

~~~Java
package com.clover.springcloud.controller;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import com.clover.springcloud.lb.LoadBalancer;
import lombok.extern.slf4j.Slf4j;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;
import java.net.URI;
import java.util.List;

@RestController
@Slf4j
public class OrderController {
    //public static final String Payment_URL = "http://localhost:8001";

    // 通过在eureka上注册过的微服务名称调用
    public static final String Payment_URL = "http://CLOUD-PAYMENT-SERVICE";

    @Resource
    private RestTemplate restTemplate;

    @Resource
    private DiscoveryClient discoveryClient;

    @Resource
    private LoadBalancer loadBalancer;

    //客户端用浏览器是get请求，但是底层实质发送post调用服务端8001
    @GetMapping("/consumer/payment/create")
    public CommonResult create(Payment payment)
    {
        return restTemplate.postForObject(Payment_URL + "/payment/create",payment,CommonResult.class);
        // return restTemplate.postForEntity(Payment_URL + "/payment/create",payment,CommonResult.class).getBody();
    }

    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult getPaymentById(@PathVariable("id") Long id)
    {
        return restTemplate.getForObject(Payment_URL + "/payment/get/" + id,CommonResult.class);
    }

    @GetMapping("/consumer/payment/getForEntity/{id}")
    public CommonResult getPayment(@PathVariable("id") Long id)
    {
        ResponseEntity<CommonResult> entity = restTemplate.getForEntity(Payment_URL + "/payment/get/" + id, CommonResult.class);
        System.out.println("-----------------");
        System.out.println(entity.getHeaders());
        System.out.println(entity.getBody());
        if(entity.getStatusCode().is2xxSuccessful()){
            return entity.getBody();
        }else {
            return new CommonResult(444,"操作失败");
        }
    }

    @GetMapping(value = "/consumer/payment/lb")
    public String getPaymentLB()
    {
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");

        if(instances == null || instances.size()<=0) {
            return null;
        }
        ServiceInstance serviceInstance = loadBalancer.instences(instances);
        URI uri = serviceInstance.getUri();

        return restTemplate.getForObject(uri+"/payment/lb",String.class);
    }
}
~~~
5. 测试：http://localhost/consumer/payment/lb
	

 		注意：在这个手写负载均衡中使用到了一个方法CompareAndSet，这个是AtomicInteger类compareAndSet
		AtomicInteger类compareAndSet通过原子操作实现了CAS操作，最底层基于汇编语言实现
		CAS是Compare And Set的一个简称，如下理解：
		1，已知当前内存里面的值current和预期要修改成的值new传入
		2，内存中AtomicInteger对象地址对应的真实值(因为有可能别修改)real与current对比，
		相等表示real未被修改过，是“安全”的，将new赋给real结束然后返回；
		不相等说明real已经被修改，结束并重新执行1直到修改成功
 		 
		 
 
 
 # 8、OpenFeign服务接口调用

## 8.1、概述

### 8.1.1、OpenFeign是什么？

1. Feign是一个声明式的Web服务客户端，让编写Web服务客户端变得非常容易，**只需创建一个接口并在接口上添加注解即可**
2. [源码地址](https://github.com/spring-cloud/spring-cloud-openfeign)

[官网解释](https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/#spring-cloud-openfeign)

Feign是一个声明式WebService客户端。使用Feign能让编写WebService客户端更加简单。
它的使用方法是**定义一个服务接口然后在上面添加注解**。Feign也支持可拔插式的编码器和解码器。SpringCloud对Feign进行了封装，使其支持了Spring MVC标准注解和HttpMessageConverters。**Feign可以与Eureka和Ribbon组合使用以支持负载均衡**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019171123.png)

### 8.1.2、能干嘛？

Feign旨在使编写Java Http客户端变得更容易。

前面在使用Ribbon+RestTemplate时，利用 RestTemplate 对http请求的封装处理，形成了一套模版化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，**往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用**。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，**我们只需创建一个接口并使用注解的方式来配置它(以前是Dao接口上面标注Mapper注解,现在是一个微服务接口上面标注一个Feign注解即可)**，即可完成对服务提供方的接口绑定，简化了Spring cloud 使用Ribbon时，自动封装服务调用客户端的开发量。

`Feign集成了Ribbon`

利用Ribbon维护了Payment的服务列表信息，并且通过轮询实现了客户端的负载均衡。而与Ribbon不同的是，**通过feign只需要定义服务绑定接口且以声明式的方法**，优雅而简单的实现了服务调用

### 8.1.3、Feign和OpenFeign两者区别
 
 ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019171433.png)
 
 
 ## 8.2、OpenFeign使用步骤
 
 1、接口 + 注解：**微服务调用接口+@FeignClient**
 
2、新建 cloud-consumer-feign-order80

- Feign在消费端使用

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019174703.png)

3、修改POM

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

    <artifactId>cloud-consumer-feign-order80</artifactId>


    <dependencies>
        <!--openfeign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
        <dependency>
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <!--web-->
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

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients // 开启FeignClients注解的使用
public class OrderFeignMain80 {
    public static void main(String[] args)
    {
        SpringApplication.run(OrderFeignMain80.class,args);
    }
}
~~~

5、业务类
- 业务逻辑接口+**@FeignClient配置调用provider服务**
- 新建PaymentFeignService接口并新增注解@FeignClient

~~~Java
package com.clover.springcloud.service;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient(value = "CLOUD-PAYMENT-SERVICE")
public interface PaymentFeignService {
    @GetMapping(value = "/payment/get/{id}")
    CommonResult<Payment> getPaymentById(@PathVariable("id") Long id);
}
~~~
- 控制层Controller

~~~Java
package com.clover.springcloud.controller;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import com.clover.springcloud.service.PaymentFeignService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
public class OrderFeignController
{
    @Resource
    private PaymentFeignService paymentFeignService;

    @GetMapping(value = "/consumer/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id)
    {
        return paymentFeignService.getPaymentById(id);
    }
}
~~~

6、测试
- 先启动2个eureka集群7001/7002
- 再启动2个微服务8001/8002
- 启动OpenFeign
- http://localhost/consumer/payment/get/1
- **Feign自带负载均衡配置项**

7、总结

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019175149.png)

## 8.3、OpenFeign超时控制

### 8.3.1、超时设置，故意设置超时演示出错情况
- 服务提供方8001故意写暂停程序

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
import java.util.concurrent.TimeUnit;

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

    @GetMapping(value = "/payment/lb")
    public String getPaymentLB()
    {
        return serverPort;
    }

    @GetMapping(value = "/payment/feign/timeout")
    public String paymentFeignTimeOut()
    {
        System.out.println("*****paymentFeignTimeOut from port: "+serverPort);
        //暂停几秒钟线程
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return serverPort;
    }

}
~~~
- 服务消费方80添加超时方法 PaymentFeignService

~~~Java
package com.clover.springcloud.service;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient(value = "CLOUD-PAYMENT-SERVICE")
public interface PaymentFeignService {
    @GetMapping(value = "/payment/get/{id}")
    CommonResult<Payment> getPaymentById(@PathVariable("id") Long id);

    @GetMapping(value = "/payment/feign/timeout")
    String paymentFeignTimeOut();
}
~~~
- 服务消费方80添加超时方法OrderFeignController

~~~Java
package com.clover.springcloud.controller;

import com.clover.springcloud.entities.CommonResult;
import com.clover.springcloud.entities.Payment;
import com.clover.springcloud.service.PaymentFeignService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
public class OrderFeignController
{
    @Resource
    private PaymentFeignService paymentFeignService;

    @GetMapping(value = "/consumer/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id)
    {
        return paymentFeignService.getPaymentById(id);
    }

    @GetMapping(value = "/payment/feign/timeout")
    public String paymentFeignTimeOut()
    {
        return paymentFeignService.paymentFeignTimeOut();
    }

}
~~~
- 测试
	- http://localhost/consumer/payment/feign/timeout
	- 错误页面

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019182910.png)

### 8.3.2、OpenFeign默认等待1秒钟，超过后报错 

### 8.3.3、是什么？

**Feign客户端默认只等待一秒钟**，但是服务端处理需要超过1秒钟，导致Feign客户端不想等待了，直接返回报错。
为了避免这样的情况，有时候我们需要设置Feign客户端的超时控制。
 
yml文件中开启配置，`OpenFeign默认支持Ribbon`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019183128.png)


### 8.3.4、YML文件里需要开启OpenFeign客户端超时控制

~~~yml
server:
  port: 80

eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/

#设置feign客户端超时时间(OpenFeign默认支持ribbon)
ribbon:
  #指的是建立连接所用的时间，适用于网络状况正常的情况下,两端连接所用的时间
  ReadTimeout: 5000
  #指的是建立连接后从服务器读取到可用资源所用的时间
  ConnectTimeout: 5000
~~~

## 8.4、OpenFeign日志打印功能

### 8.4.1、是什么？
 
Feign 提供了日志打印功能，我们可以通过配置来调整日志级别，从而了解 Feign 中 Http 请求的细节。

说白了就是**对Feign接口的调用情况进行监控和输出**
 
 
 ### 8.4.2、日志级别
- NONE：默认的，不显示任何日志；
- BASIC：仅记录请求方法、URL、响应状态码及执行时间；
- HEADERS：除了 BASIC 中定义的信息之外，还有请求和响应的头信息；
- FULL：除了 HEADERS 中定义的信息之外，还有请求和响应的正文及元数据。

### 8.4.3、配置日志bean
~~~Java
package com.clover.springcloud.config;

import feign.Logger;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


@Configuration
public class FeignConfig {

    @Bean
    Logger.Level feignLoggerLevel()
    {
        return Logger.Level.FULL;
    }
}
~~~

### 8.3.4、YML文件里需要开启日志的Feign客户端

~~~yml
server:
  port: 80

eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/

#设置feign客户端超时时间(OpenFeign默认支持ribbon)
ribbon:
  #指的是建立连接所用的时间，适用于网络状况正常的情况下,两端连接所用的时间
  ReadTimeout: 5000
  #指的是建立连接后从服务器读取到可用资源所用的时间
  ConnectTimeout: 5000

logging:
  level:
    # feign日志以什么级别监控哪个接口
    com.clover.springcloud.service.PaymentFeignService: debug
~~~

### 8.3.5、后台日志查看

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019184913.png)
 
# 9、Hystrix断路器

## 9.1、概述

### 9.1.1、分布式系统面临的问题

**复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免地失败**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019200403.png)

服务雪崩

多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其它的微服务，这就是所谓的**扇出**。如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”.
 
对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒钟内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障。这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。
所以，通常当你发现一个模块下的某个实例失败后，这时候这个模块依然还会接收流量，然后这个有问题的模块还调用了其他的模块，这样就会**发生级联故障，或者叫雪崩**。

### 9.1.2、Hystrix是什么？
 
Hystrix是一个用于处理分布式系统的**延迟**和**容错**的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等，Hystrix能够保证在一个依赖出问题的情况下，**不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性**。
 
“断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），**向调用方返回一个符合预期的、可处理的备选响应（FallBack），而不是长时间的等待或者抛出调用方无法处理的异常**，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

### 9.1.3、Hystrix能干嘛？
- 服务降级
- 服务熔断
- 接近实时的监控
 
 ### 9.1.4、官网资料
 
 [官方资料](https://github.com/Netflix/Hystrix/wiki/How-To-Use)
 
 
## 9.2、Hystrix重要概念

### 9.2.1、服务降级

- 服务器忙，请稍后再试，不让客户端等待并立刻返回一个友好提示，fallback
- 哪些情况会触发降级
	- 程序运行异常
	- 超时
	- 服务熔断触发降级
	- 线程池/信号量打满也会导致服务降级

### 9.2.2、服务熔断

- 类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并返回友好提示
- 就是保险丝
	- 服务的降级->进而熔断->恢复调用链路

### 9.2.3、服务限流
- 服务限流
	- 秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒钟N个，有序进行

## 9.3、hystrix案例

### 9.3.1、构建

1、新建 cloud-provider-hystrix-payment8001

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

    <artifactId>cloud-provider-hystrix-payment8001</artifactId>


    <dependencies>
        <!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
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
  port: 8001

spring:
  application:
    name: cloud-provider-hystrix-payment

eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      #defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
      defaultZone: http://eureka7001.com:7001/eureka
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient//本服务启动后会自动注册进eureka服务中
public class PaymentHystrixMain8001 {
    public static void main(String[] args)
    {
        SpringApplication.run(PaymentHystrixMain8001.class,args);
    }
}
~~~

5、编写业务类
- service

~~~Java
package com.clover.springcloud.service;

import org.springframework.stereotype.Service;

import java.util.concurrent.TimeUnit;

@Service
public class PaymentService {

    /**
     * 正常访问，一切OK
     */
    public String paymentInfo_OK(Integer id)
    {
        return "线程池：" + Thread.currentThread().getName() + "paymentInfo_OK,id:" + id;
    }

    /*
     * 超时访问，演示降级
     */
    public String paymentInfo_Timeout(Integer id)
    {
        int timeoutNumber = 3;
        //暂停几秒钟线程
        try {
            TimeUnit.SECONDS.sleep(timeoutNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "线程池：" + Thread.currentThread().getName() + "paymentInfo_Timeout,id:" + id + "耗费时间：" + timeoutNumber + "秒";
    }
}
~~~
- controller

~~~Java
package com.clover.springcloud.controller;

import com.clover.springcloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {

    @Resource
    private PaymentService paymentService;

    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id)
    {
        String result = paymentService.paymentInfo_OK(id);
        log.info("*********result:" + result);
        return result;
    }

    @GetMapping(value = "/payment/hystrix/timeout/{id}")
    public String paymentInfo_Timeout(@PathVariable("id") Integer id)
    {
        String result = paymentService.paymentInfo_Timeout(id);
        log.info("*********result:" + result);
        return result;
    }
}
~~~

6、正常测试
- 启动eureka7001
- 启动cloud-provider-hystrix-payment8001
- 访问
	- success的方法：http://localhost:8001/payment/hystrix/ok/1
	- 每次调用耗费3秒钟：http://localhost:8001/payment/hystrix/timeout/1
- 上述module均OK
	- 以上述为根基平台，从正确->错误->降级熔断->恢复


### 9.3.2、高并发测试

1、Jmeter压测测试
- 开启Jmeter，来20000个并发压死8001，20000个请求都去访问paymentInfo_TimeOut服务
- 再来一个访问：http://localhost:8001/payment/hystrix/ok/1
- 看演示结果
	- 两个都在自己转圈圈
	- 为什么会被卡死
		- tomcat的默认的工作线程数被打满 了，没有多余的线程来分解压力和处理

2、Jmeter压测结论

上面还是服务**提供者8001自己测试**，假如此时外部的消费者80也来访问，那**消费者**只能干等，最终导致消费端80不满意，服务端8001直接被拖死

3、看热闹不嫌弃事大，80新建加入

1. 新建 cloud-consumer-feign-hystrix-order80
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

    <artifactId>cloud-consumer-feign-hystrix-order80</artifactId>


    <dependencies>
        <!--openfeign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
        <dependency>
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--一般基础通用配置-->
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

eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
~~~
4. 编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients
public class OrderHystrixMain80 {
    public static void main(String[] args)
    {
        SpringApplication.run(OrderHystrixMain80.class,args);
    }
}
~~~
5. 编写业务类
- PaymentHystrixService

~~~Java
package com.clover.springcloud.service;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@Component
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT")
public interface PaymentHystrixService {

    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id);

    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_Timeout(@PathVariable("id") Integer id);
}

~~~
-  OrderHystirxController

~~~Java
package com.clover.springcloud.controller;

import com.clover.springcloud.service.PaymentHystrixService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderHystirxController {

    @Resource
    private PaymentHystrixService paymentHystrixService;

    @GetMapping("/consumer/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id)
    {
        return paymentHystrixService.paymentInfo_OK(id);
    }

    @GetMapping("/consumer/payment/hystrix/timeout/{id}")
    public String paymentInfo_Timeout(@PathVariable("id") Integer id)
    {
        return paymentHystrixService.paymentInfo_Timeout(id);
    }
}
~~~
6. 正常测试：http://localhost/consumer/payment/hystrix/ok/1
7. 高并发测试
	1. 2W个线程压8001
	2. 消费端80微服务再去访问正常的Ok微服务8001地址
	3. http://localhost/consumer/payment/hystrix/ok/1
	4. 消费者80
		1. 要么转圈圈等待
		2. 要么消费端报超时错误

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019212820.png)

### 9.3.3、故障现象和导致原因
1. 8001同一层次的其它接口服务被困死，**因为tomcat线程池里面的工作线程已经被挤占完毕**
2. 80此时调用8001，客户端访问响应缓慢，转圈圈

### 9.3.4、上诉结论

正因为有上述故障或不佳表现才有我们的`降级/容错/限流等`技术诞生

### 9.3.5、如何解决？解决的要求

1. 超时导致服务器变慢(转圈)	超时不再等待
2. 出错(宕机或程序运行出错)	出错要有兜底
3. 解决
	1. 对方服务(8001)超时了，调用者(80)不能一直卡死等待，必须有服务降级
	2. 对方服务(8001)down机了，调用者(80)不能一直卡死等待，必须有服务降级
	3. 对方服务(8001)OK，调用者(80)自己出故障或有自我要求（自己的等待时间小于服务提供者），自己处理降级


### 9.3.6、服务降级

1、降级配置  **@HystrixCommand**

2、8001先从自身找问题

- 设置自身调用超时时间的峰值，峰值内可以正常运行，超过了需要有兜底的方法处理，作服务降级fallback

3、8001fallback

- 业务类启用
	- **@HystrixCommand**报异常后如何处理
		- 一旦调用服务方法失败并抛出了错误信息后，会自动调用`@HystrixCommand`标注好的`fallbackMethod`调用类中的指定方法

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019215259.png)
	
	上图故意制造两个异常：
	
   1  int age = 10/0; 计算异常
   
   2  我们能接受3秒钟，它运行5秒钟，超时异常
   
   **当前服务不可用了，做服务降级，兜底的方案都是**`paymentInfo_TimeOutHandler`

4、主启动类激活

- **添加新注解@EnableCircuitBreaker**
