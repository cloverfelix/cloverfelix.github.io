# Springcloud


## 1、从2.2.x和H版本开始说起

### 1.1、SpringBoot版本选择

[GitHub源码地址](https://github.com/spring-projects/spring-boot/releases/)

SpringBoot2.0新特性

- 通过上面官网发现，Boot官方强烈建议你升级到2.X以上版本

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016153757.png)

### 1.2、SpringCloud版本选择

[GitHub源码地址](https://github.com/spring-projects/spring-cloud)

[官网](https://spring.io/projects/spring-cloud)

#### 1.2.1、Cloud命名规则

Spring Cloud 采用了英国伦敦地铁站的名称来命名，并由地铁站名称字母A-Z依次类推的形式来发布迭代
版本SpringCloud是一个由许多子项目组成的综合项目，各子项目有不同的发布节奏。为了管理SpringCloud与各子项目的版本依赖关系，发布了一个清单，其中包括了某个SpringCloud版本对应的子项目版本。为了避免SpringCloud版本号与子项目版本号混淆，**SpringCloud版本采用了名称而非版本号的命名，这些版本的名字采用了伦敦地铁站的名字，根据字母表的顺序来对应版本时间顺序**。例如Angel是第一个版本, Brixton是第二个版本。

当SpringCloud的发布内容积累到临界点或者一个重大BUG被解决后，会发布一个"service releases"版本，简称SRX版本，比如Greenwich.SR2就是SpringCloud发布的Greenwich版本的第2个SRX版本。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016154404.png)

### 1.3、SpringCloud和SpringBoot之间的依赖关系如何看

[官网说明](https://spring.io/projects/spring-cloud#overview)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016154648.png)

#### 1.3.1、依赖

Finchley 是基于 Spring Boot 2.0.x 构建的不再 Boot 1.5.x

Dalston 和 Edgware 是基于 Spring Boot 1.5.x 构建的，不支持 Spring Boot 2.0.x

Camden 构建于 Spring Boot 1.4.x，但依然能支持 Spring Boot 1.5.x

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016154734.png)

[更详细的版本对应查看方法](https://start.spring.io/actuator/info)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016154945.png)

### 1.4、目前使用版本

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016155101.png)

#### 1.4.1、题外话

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
	
## 2、关于Cloud各种组件的停更/升级/替换

- 以前学习

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016155723.png)	
	
- 现在

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016155757.png)
	
[SpringCloud官方文档](https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/)
	

[SpringCloud中文文档](https://www.bookstack.cn/read/spring-cloud-docs/docs-index.md)	

[SpringBoot官方文档](https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/htmlsingle/)	

## 3、微服务架构编码构建

**约定 > 配置 > 编码**

### 3.1、IDEA新建project工作空间

1. 创建maven项目
2. 设置字符编码为`utf-8`

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016160424.png)
3. 设置注解生效激活

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016160540.png)
4. Java编译版本选择8

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016160631.png)
5. File Type过滤 **(可选)**

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211016160706.png)
	
### 3.2、父工程POM

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

### 3.3、Maven工程落地细节复习

#### 3.3.1、Maven中的DependencyManagement和Dependencies

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

#### 3.3.2、Maven中跳过单元测试

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
	
### 3.4、Rest微服务工程构建

#### 3.4.1、提供者模块构建

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

#### 3.4.2、小知识点补充

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

#### 3.4.3、消费者模块构建

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

#### 3.4.4、小知识点补充

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

### 3.5、工程重构

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


## 4、Eureka服务注册与发现

### 4.1、Eureka基础知识

#### 4.1.1、什么是服务治理

SpringCloud 封装了 Netflix 公司开发的 Eureka 模块来**实现服务治理**

在传统的rpc远程调用框架中，管理每个服务与服务之间依赖关系比较复杂，管理比较复杂，所以需要使用服务治理，管理服务与服务之间依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现与注册。

#### 4.1.2、什么是服务注册

Eureka 采用了CS的设计架构，Eureka Server 作为服务注册功能的服务器，它是服务注册中心。而系统中的其他微服务，使用 Eureka 的客户端连接到 Eureka Server 并维持心跳连接。这样系统的维护人员就可以通过 Eureka Server 来监控系统中各个微服务是否正常运行。

在服务注册与发现中，有一个注册中心。当服务器启动的时候，会把当前自己服务器的信息,比如 服务地址、通讯地址等以别名方式注册到注册中心上。另一方（消费者|服务提供者），以该别名的方式去注册中心上获取到实际的服务通讯地址，然后再实现本地RPC调用RPC远程框架核心设计思想：在于注册中心，因为使用注册中心管理每个服务与服务之间的一个依赖关系(服务治理概念)。在任何RPC远程框架中，都会有一个注册中心(存放服务地址相关信息(接口地址))

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017112259.png)

#### 4.1.3、Eureka两组件

**Eureka Server**提供服务注册服务

各个微服务节点通过配置启动后，会在EurekaServer中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。

**EurekaClient**通过注册中心进行访问

是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置的、使用轮询(round-robin)负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳(默认周期为30秒)。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除（默认90秒）

### 4.2、单机Eureka构建步骤

#### 4.2.1、IDEA生成eurekaServer端服务注册中心类似物业公司

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

#### 4.2.2、EurekaClient端cloud-provider-payment8001

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

#### 4.2.3、EurekaClient端cloud-consumer-order80

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


### 4.3、集群Eureka构建步骤

#### 4.3.1、Eureka集群原理说明

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211017201223.png)

`EurekaServer 集群是互相注册，相互守望`

问题：微服务RPC远程服务调用最核心的是什么 

	高可用，试想你的注册中心只有一个only one， 它出故障了那就呵呵(￣▽￣)"了，会导致整个为服务环境不可用，所以
	解决办法：搭建Eureka注册中心集群 ，实现负载均衡+故障容错
	
#### 4.3.2、EurekaServer集群环境构建步骤

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

#### 4.3.3、将支付服务8001微服务发布到上面2台Eureka集群配置中

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

#### 4.3.4、将订单服务80微服务发布到上面2台Eureka集群配置中

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


#### 4.3.5、支付服务提供者8001集群环境构建

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

#### 4.3.6、负载均衡

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

### 4.4、actuator微服务信息完善

#### 4.4.1、主机名称:服务名称修改

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

#### 4.4.2、访问信息有IP信息提示

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

### 4.5、服务发现Discovery

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

 ### 4.6、Eureka自我保护机制
 
 #### 4.6.1、故障现象
 
 ##### 4.6.1.1、概述
 
保护模式主要用于一组客户端和Eureka Server之间存在网络分区场景下的保护。一旦进入保护模式，
**Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据，也就是不会注销任何微服务**。

如果在Eureka Server的首页看到以下这段提示，则说明Eureka进入了保护模式：

EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. 
RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE 

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018110517.png)

#### 4.6.2、导致原因

##### 4.6.2.1、为什么会产生Eureka自我保护机制？

为了防止EurekaClient可以正常运行，但是与 EurekaServer 网络不通情况下，EurekaServer**不会立刻**将EurekaClient服务剔除


##### 4.6.2.2、什么是自我保护模式

默认情况下，如果EurekaServer在一定时间内没有接收到某个微服务实例的心跳，EurekaServer将会注销该实例（默认90秒）。但是`当网络分区故障发生(延时、卡顿、拥挤)时`，微服务与EurekaServer之间无法正常通信，以上行为可能变得非常危险了——因为微服务本身其实是健康的，**此时本不应该注销这个微服务**。Eureka通过“自我保护模式”来解决这个问题——当EurekaServer节点在短时间内丢失过多客户端时（可能发生了网络分区故障），那么这个节点就会进入自我保护模式。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018110839.png)

**在自我保护模式中，EurekaServer会保护服务注册表中的信息，不再注销任何服务实例。**

它的设计哲学就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。**一句话讲解：好死不如赖活着**


综上，自我保护模式是一种应对网络异常的安全保护措施。它的架构哲学是宁可同时保留所有微服务（健康的微服务和不健康的微服务都会保留）也不盲目注销任何健康的微服务。使用自我保护模式，可以让Eureka集群更加的健壮、稳定。

`一句话：某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对该微服务的信息进行保存`

#### 4.6.3、怎么禁止自我保护


##### 4.6.3.1、修改注册中心eureakeServer端7001

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



##### 4.6.3.2、修改生产者客户端eureakeClient端8001

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


## 5、Zookeeper服务注册与发现

### 5.1、SpringCloud整合Zookeeper代替Eureka

#### 5.1.1、注册中心Zookeeper

- zookeeper是一个分布式协调工具，可以实现注册中心功能
- 关闭Linux服务器防火墙后启动zookeeper服务器
- zookeeper服务器取代Eureka服务器，zk作为服务注册中心

#### 5.1.2、服务提供者

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
- 排除zk冲突后的新POM
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
	
#### 5.1.3、服务消费者

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

## 6、Consul服务注册与发现

### 6.1、Consul简介

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


### 6.2、安装并运行Consul

[官网安装说明](https://learn.hashicorp.com/consul/getting-started/install.html)

下载完成后只有一个consul.exe文件，硬盘路径下双击运行，查看版本号信息

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018204826.png)

使用开发模式启动
- consul agent -dev
- 通过以下地址可以访问Consul的首页：http://localhost:8500
- 结果页面
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211018205022.png)]

### 6.3、服务提供者

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

### 6.4、服务消费者

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


### 6.5、三个注册中心异同点

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


## 7、Ribbon负载均衡服务调用

### 7.1、概述

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
 
 
 ### 7.2、Ribbon负载均衡演示
 
 #### 7.2.1、架构说明
 
 ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019104119.png)
 
 Ribbon在工作时分成两步
- 第一步先选择 EurekaServer ,它优先选择在同一个区域内负载较少的server.
- 第二步再根据用户指定的策略，在从server取到的服务注册列表中选择一个地址。

其中Ribbon提供了多种策略：比如轮询、随机和根据响应时间加权。
 
 `总结`：**Ribbon其实就是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和
 eureka结合只是其中的一个实例。**
 
 
#### 7.2.2、POM

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

#### 7.2.3、二说RestTemplate的使用

[官网](https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html)

##### 7.2.3.1、getForObject方法/getForEntity方法

**返回对象为响应体中数据转化成的对象，基本上可以理解为Json**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019105055.png)

**返回对象为ResponseEntity对象，包含了响应中的一些重要信息，比如响应头，状态码、响应体等**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019110324.png)

##### 7.2.3.2、postForObject/postForEntity方法

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019110504.png)

### 7.3、Ribbon核心组件IRule

#### 7.3.1、IRule：根据特定算法中从服务列表中选取一个要访问的服务

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019110952.png)

`com.netflix.loadbalancer.RoundRobinRule`：**轮询**

`com.netflix.loadbalancer.RandomRule`：**随机**

`com.netflix.loadbalancer.RetryRule`：**先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重试，获取可用的服务**

`WeightedResponseTimeRule`：**对RoundRobinRule的扩展，响应速度越快的实例选择权重越大，越容易被选择**

`BestAvailableRule`：**会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务**

`AvailabilityFilteringRule`：**先过滤掉故障实例，再选择并发较小的实例**

`ZoneAvoidanceRule`：**默认规则,复合判断server所在区域的性能和server的可用性选择服务器**

#### 7.3.2、如何替换

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

### 7.4、Ribbon负载均衡算法

#### 7.4.1、原理

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

 
#### 7.4.2、手写一个本地负载均衡器

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
 		 
		 
 
 
 ## 8、OpenFeign服务接口调用

### 8.1、概述

#### 8.1.1、OpenFeign是什么？

1. Feign是一个声明式的Web服务客户端，让编写Web服务客户端变得非常容易，**只需创建一个接口并在接口上添加注解即可**
2. [源码地址](https://github.com/spring-cloud/spring-cloud-openfeign)

[官网解释](https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/#spring-cloud-openfeign)

Feign是一个声明式WebService客户端。使用Feign能让编写WebService客户端更加简单。
它的使用方法是**定义一个服务接口然后在上面添加注解**。Feign也支持可拔插式的编码器和解码器。SpringCloud对Feign进行了封装，使其支持了Spring MVC标准注解和HttpMessageConverters。**Feign可以与Eureka和Ribbon组合使用以支持负载均衡**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019171123.png)

#### 8.1.2、能干嘛？

Feign旨在使编写Java Http客户端变得更容易。

前面在使用Ribbon+RestTemplate时，利用 RestTemplate 对http请求的封装处理，形成了一套模版化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，**往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用**。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，**我们只需创建一个接口并使用注解的方式来配置它(以前是Dao接口上面标注Mapper注解,现在是一个微服务接口上面标注一个Feign注解即可)**，即可完成对服务提供方的接口绑定，简化了Spring cloud 使用Ribbon时，自动封装服务调用客户端的开发量。

`Feign集成了Ribbon`

利用Ribbon维护了Payment的服务列表信息，并且通过轮询实现了客户端的负载均衡。而与Ribbon不同的是，**通过feign只需要定义服务绑定接口且以声明式的方法**，优雅而简单的实现了服务调用

#### 8.1.3、Feign和OpenFeign两者区别
 
 ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019171433.png)
 
 
 ### 8.2、OpenFeign使用步骤
 
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

### 8.3、OpenFeign超时控制

#### 8.3.1、超时设置，故意设置超时演示出错情况
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

#### 8.3.2、OpenFeign默认等待1秒钟，超过后报错 

#### 8.3.3、是什么？

**Feign客户端默认只等待一秒钟**，但是服务端处理需要超过1秒钟，导致Feign客户端不想等待了，直接返回报错。
为了避免这样的情况，有时候我们需要设置Feign客户端的超时控制。
 
yml文件中开启配置，`OpenFeign默认支持Ribbon`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019183128.png)


#### 8.3.4、YML文件里需要开启OpenFeign客户端超时控制

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

### 8.4、OpenFeign日志打印功能

#### 8.4.1、是什么？
 
Feign 提供了日志打印功能，我们可以通过配置来调整日志级别，从而了解 Feign 中 Http 请求的细节。

说白了就是**对Feign接口的调用情况进行监控和输出**
 
 
 #### 8.4.2、日志级别
- NONE：默认的，不显示任何日志；
- BASIC：仅记录请求方法、URL、响应状态码及执行时间；
- HEADERS：除了 BASIC 中定义的信息之外，还有请求和响应的头信息；
- FULL：除了 HEADERS 中定义的信息之外，还有请求和响应的正文及元数据。

#### 8.4.3、配置日志bean
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

#### 8.4.5、YML文件里需要开启日志的Feign客户端

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

#### 8.4.6、后台日志查看

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019184913.png)
 
## 9、Hystrix断路器

### 9.1、概述

#### 9.1.1、分布式系统面临的问题

**复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免地失败**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211019200403.png)

服务雪崩

多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其它的微服务，这就是所谓的**扇出**。如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”.
 
对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒钟内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障。这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。
所以，通常当你发现一个模块下的某个实例失败后，这时候这个模块依然还会接收流量，然后这个有问题的模块还调用了其他的模块，这样就会**发生级联故障，或者叫雪崩**。

#### 9.1.2、Hystrix是什么？
 
Hystrix是一个用于处理分布式系统的**延迟**和**容错**的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等，Hystrix能够保证在一个依赖出问题的情况下，**不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性**。
 
“断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），**向调用方返回一个符合预期的、可处理的备选响应（FallBack），而不是长时间的等待或者抛出调用方无法处理的异常**，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

#### 9.1.3、Hystrix能干嘛？
- 服务降级
- 服务熔断
- 接近实时的监控
 
 #### 9.1.4、官网资料
 
 [官方资料](https://github.com/Netflix/Hystrix/wiki/How-To-Use)
 
 
### 9.2、Hystrix重要概念

#### 9.2.1、服务降级

- 服务器忙，请稍后再试，不让客户端等待并立刻返回一个友好提示，fallback
- 哪些情况会触发降级
	- 程序运行异常
	- 超时
	- 服务熔断触发降级
	- 线程池/信号量打满也会导致服务降级

#### 9.2.2、服务熔断

- 类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并返回友好提示
- 就是保险丝
	- 服务的降级->进而熔断->恢复调用链路

#### 9.2.3、服务限流
- 服务限流
	- 秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒钟N个，有序进行

### 9.3、hystrix案例

#### 9.3.1、构建

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


#### 9.3.2、高并发测试

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

#### 9.3.3、故障现象和导致原因
1. 8001同一层次的其它接口服务被困死，**因为tomcat线程池里面的工作线程已经被挤占完毕**
2. 80此时调用8001，客户端访问响应缓慢，转圈圈

#### 9.3.4、上诉结论

正因为有上述故障或不佳表现才有我们的`降级/容错/限流等`技术诞生

#### 9.3.5、如何解决？解决的要求

1. 超时导致服务器变慢(转圈)	超时不再等待
2. 出错(宕机或程序运行出错)	出错要有兜底
3. 解决
	1. 对方服务(8001)超时了，调用者(80)不能一直卡死等待，必须有服务降级
	2. 对方服务(8001)down机了，调用者(80)不能一直卡死等待，必须有服务降级
	3. 对方服务(8001)OK，调用者(80)自己出故障或有自我要求（自己的等待时间小于服务提供者），自己处理降级


#### 9.3.6、服务降级

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

- 主启动类激活

	- **添加新注解@EnableCircuitBreaker**

4、80fallback
- 80订单微服务，也可以更好的保护自己，自己也依样画葫芦进行客户端降级保护
- `题外话，切记`
	- 我们自己配置过的热部署方式对java代码的改动明显，但对`@HystrixCommand内属性的修改建议重启微服务`
- 修改YML

~~~yml
server:
  port: 80

eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
      
feign:
  hystrix:
    enabled: true
~~~
- 修改主启动类
	- 加入`@EnableHystrix注解`
- 修改业务类

~~~java 
package com.clover.springcloud.controller;

import com.clover.springcloud.service.PaymentHystrixService;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
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
    @HystrixCommand(fallbackMethod = "paymentTimeOutFallbackMethod",commandProperties = {
            @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value="1500")
    })
    public String paymentInfo_Timeout(@PathVariable("id") Integer id)
    {
        return paymentHystrixService.paymentInfo_Timeout(id);
    }

    public String paymentTimeOutFallbackMethod(@PathVariable("id") Integer id)
    {
        return "我是消费者80,对方支付系统繁忙请10秒钟后再试或者自己运行出错请检查自己,o(╥﹏╥)o";
    }
}
~~~

5、目前问题
- 每个业务方法对应一个兜底的方法，代码膨胀
- 统一和自定义的分开

6、解决问题
1. 每个方法配置一个？？？膨胀

	- feign接口系列
	- **@DefaultProperties(defaultFallback = "")**

			@DefaultProperties(defaultFallback = "")
			  1：1 每个方法配置一个服务降级方法，技术上可以，实际上傻X
			  1：N 除了个别重要核心业务有专属，其它普通的可以通过@DefaultProperties(defaultFallback = "")  
			  统一跳转到统一处理结果页面
			  通用的和独享的各自分开，避免了代码膨胀，合理减少了代码量，

	- controller配置

~~~Java
package com.clover.springcloud.controller;

import com.clover.springcloud.service.PaymentHystrixService;
import com.netflix.hystrix.contrib.javanica.annotation.DefaultProperties;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
@Slf4j
@DefaultProperties(defaultFallback = "payment_Global_FallbackMethod")
public class OrderHystirxController {

    @Resource
    private PaymentHystrixService paymentHystrixService;

    @GetMapping("/consumer/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id)
    {
        return paymentHystrixService.paymentInfo_OK(id);
    }

    @GetMapping("/consumer/payment/hystrix/timeout/{id}")
//    @HystrixCommand(fallbackMethod = "paymentTimeOutFallbackMethod",commandProperties = {
//            @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value="1500")
//    })
    @HystrixCommand //加了@DefaultProperties属性注解，并且没有写具体方法名字，就用统一全局的
    public String paymentInfo_Timeout(@PathVariable("id") Integer id)
    {
        int age = 10/0;
        return paymentHystrixService.paymentInfo_Timeout(id);
    }

    public String paymentTimeOutFallbackMethod(@PathVariable("id") Integer id)
    {
        return "我是消费者80,对方支付系统繁忙请10秒钟后再试或者自己运行出错请检查自己,o(╥﹏╥)o";
    }

    public String payment_Global_FallbackMethod()
    {
        return "Global异常处理信息，请稍后再试，/(ㄒoㄒ)/~~";
    }
}
~~~

2. 和业务逻辑混一起？？？混乱

	- **服务降级，客户端去调用服务端，碰上服务端宕机或关闭**
	- 本次案例服务降级处理是`在客户端80实现完成的`，与服务端8001没有关系只需要为Feign客户端定义的接口添加一个服务降级处理的实现类即可实现解耦
	- 未来我们要面对的异常
		- 运行
		- 超时
		- 宕机
	- 再看我们的业务类PaymentController

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020112949.png)

	混合在一起，每个业务方法都要提供一个

	- 修改cloud-consumer-feign-hystrix-order80
	- 根据cloud-consumer-feign-hystrix-order80已经有的PaymentHystrixService接口，重新新建一个类(**PaymentFallbackService**)实现该接口，**统一为接口里面的方法进行异常处理**
	- PaymentFallbackService类实现PaymentFeignClientService接口

	~~~Java
	package com.clover.springcloud.service;

	import org.springframework.stereotype.Component;

	@Component
	public class PaymentFallbackService implements PaymentHystrixService {
		@Override
		public String paymentInfo_OK(Integer id) {
			return "服务调用失败，提示来自：cloud-consumer-feign-order80的paymentInfo_OK";
		}

		@Override
		public String paymentInfo_Timeout(Integer id) {
			return "服务调用失败，提示来自：cloud-consumer-feign-order80的paymentInfo_Timeout";
		}
	}
	~~~
	- PaymentFeignClientService接口

	~~~Java
	package com.clover.springcloud.service;  

	import org.springframework.cloud.openfeign.FeignClient;  
	import org.springframework.stereotype.Component;  
	import org.springframework.web.bind.annotation.GetMapping;  
	import org.springframework.web.bind.annotation.PathVariable;  

	@Component  
	@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT",fallback = PaymentFallbackService.class)  
	public interface PaymentHystrixService {  

	 @GetMapping("/payment/hystrix/ok/{id}")  
	 public String paymentInfo_OK(@PathVariable("id") Integer id);  

		@GetMapping("/payment/hystrix/timeout/{id}")  
	 public String paymentInfo_Timeout(@PathVariable("id") Integer id);  
	}
	~~~
	- 测试
		- 单个eureka先启动7001
		- PaymentHystrixMain8001启动
		- 正常访问测试：http://localhost/consumer/payment/hystrix/ok/1
		- **故意关闭微服务8001**
		- **客户端自己调用提示**
			- 此时服务端provider已经down了，但是我们做了服务降级处理，让客户端在服务端不可用时也会获得提示信息而不会挂起耗死服务器


#### 9.3.7、服务熔断

##### 9.3.7.1、熔断是什么？

**熔断机制概述**

熔断机制是应对雪崩效应的一种微服务链路保护机制。当扇出链路的某个微服务出错不可用或者响应时间太长时，
会进行服务的降级，进而熔断该节点微服务的调用，快速返回错误的响应信息。

**当检测到该节点微服务调用响应正常后，恢复调用链路。**
 
在SpringCloud框架里，熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失败，就会启动熔断机制。熔断机制的注解是`@HystrixCommand。`
 
 ##### 9.3.7.2、实操
 
 1. 修改cloud-provider-hystrix-payment8001
 2. 修改PaymentService
	~~~Java
	//=========服务熔断
	@HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback",commandProperties = {
			@HystrixProperty(name = "circuitBreaker.enabled",value = "true"),
			@HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"), 
			@HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"),
			@HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60"),
	})
	public String paymentCircuitBreaker(@PathVariable("id") Integer id)
	{
		if(id < 0)
		{
			throw new RuntimeException("******id 不能负数");
		}
		String serialNumber = IdUtil.simpleUUID();

		return Thread.currentThread().getName()+"\t"+"调用成功，流水号: " + serialNumber;
	}
	public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id)
	{
		return "id 不能负数，请稍后再试，/(ㄒoㄒ)/~~   id: " +id;
	}
	~~~
	 1. why配置这些参数

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020153119.png)
 3. 修改PaymentController
 ~~~Java
@GetMapping("/payment/circuit/{id}")
public String paymentCircuitBreaker(@PathVariable("id") Integer id)
{
    String result = paymentService.paymentCircuitBreaker(id);
    log.info("****result: "+result);
    return result;
}
 ~~~
 
 
测试：
- 自测cloud-provider-hystrix-payment8001
- 正确访问：http://localhost:8001/payment/circuit/1
- 错误访问：http://localhost:8001/payment/circuit/-1
- 重点测试：**多次错误，然后慢慢正确，发现刚开始不满足条件，就算是正确的访问地址也不能进行**


 ##### 9.3.7.3、原理(小总结)
 
 大神结论
 
 ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020155000.png)
 
 熔断类型
 - 熔断打开
	 - 请求不再进行调用当前服务，内部设置时钟一般为`MTTR（平均故障处理时间)`，当打开时长达到所设时钟则进入半熔断状态
 - 熔断关闭
	 - 熔断关闭不会对服务进行熔断
 - 熔断半开
	 - 部分请求根据规则调用当前服务，如果请求成功且符合规则则认为当前服务恢复正常，关闭熔断

官网断路器流程图

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020155127.png)

1、官网步骤

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020155211.png)

2、断路器在什么情况下开始起作用

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020155432.png)

涉及到断路器的三个重要参数：**快照时间窗、请求总数阀值、错误百分比阀值**。

1：`快照时间窗`：断路器确定是否打开需要统计一些请求和错误数据，而统计的时间范围就是快照时间窗，**默认为最近的10秒**。
 
2：`请求总数阀值`：在快照时间窗内，**必须满足请求总数阀值才有资格熔断。默认为20**，意味着在10秒内，如果该hystrix命令的调用次数不足20次，即使所有的请求都超时或其他原因失败，断路器都不会打开。
 
3：`错误百分比阀值`：当请求总数在快照时间窗内超过了阀值，比如发生了30次调用，如果在这30次调用中，有15次发生了超时异常，**也就是超过50%的错误百分比**，在默认设定50%阀值情况下，这时候就会将断路器打开。

3、断路器开启或者关闭的条件

- 当满足一定的阀值的时候（默认10秒内超过20个请求次数）
- 当失败率达到一定的时候（默认10秒内超过50%的请求失败）
- 到达以上阀值，断路器将会开启
- 当开启的时候，所有请求都不会进行转发
- 一段时间之后（默认是5秒），这个时候断路器是半开状态，会让其中一个请求进行转发。如果成功，断路器会关闭，若失败，继续开启。重复4和5


4、断路器打开之后

1. 再有请求调用的时候，将不会调用主逻辑，而是直接调用降级fallback。通过断路器，实现了自动地发现错误并将降级逻辑切换为主逻辑，减少响应延迟的效果。
 
2. 原来的主逻辑要如何恢复呢？
	
	对于这一问题，hystrix也为我们实现了自动恢复功能。
	当断路器打开，对主逻辑进行熔断之后，hystrix会启动一个休眠时间窗，在这个时间窗内，降级逻辑是临时的成为主逻辑，
	当休眠时间窗到期，断路器将进入半开状态，释放一次请求到原来的主逻辑上，如果此次请求正常返回，那么断路器将继续闭合，
	主逻辑恢复，如果这次请求依然有问题，断路器继续进入打开状态，休眠时间窗重新计时。


**5、ALL配置**

~~~Java
//========================All
@HystrixCommand(fallbackMethod = "str_fallbackMethod",
        groupKey = "strGroupCommand",
        commandKey = "strCommand",
        threadPoolKey = "strThreadPool",

        commandProperties = {
                // 设置隔离策略，THREAD 表示线程池 SEMAPHORE：信号池隔离
                @HystrixProperty(name = "execution.isolation.strategy", value = "THREAD"),
                // 当隔离策略选择信号池隔离的时候，用来设置信号池的大小（最大并发数）
                @HystrixProperty(name = "execution.isolation.semaphore.maxConcurrentRequests", value = "10"),
                // 配置命令执行的超时时间
                @HystrixProperty(name = "execution.isolation.thread.timeoutinMilliseconds", value = "10"),
                // 是否启用超时时间
                @HystrixProperty(name = "execution.timeout.enabled", value = "true"),
                // 执行超时的时候是否中断
                @HystrixProperty(name = "execution.isolation.thread.interruptOnTimeout", value = "true"),
                // 执行被取消的时候是否中断
                @HystrixProperty(name = "execution.isolation.thread.interruptOnCancel", value = "true"),
                // 允许回调方法执行的最大并发数
                @HystrixProperty(name = "fallback.isolation.semaphore.maxConcurrentRequests", value = "10"),
                // 服务降级是否启用，是否执行回调函数
                @HystrixProperty(name = "fallback.enabled", value = "true"),
                // 是否启用断路器
                @HystrixProperty(name = "circuitBreaker.enabled", value = "true"),
                // 该属性用来设置在滚动时间窗中，断路器熔断的最小请求数。例如，默认该值为 20 的时候，
                // 如果滚动时间窗（默认10秒）内仅收到了19个请求， 即使这19个请求都失败了，断路器也不会打开。
                @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold", value = "20"),
                // 该属性用来设置在滚动时间窗中，表示在滚动时间窗中，在请求数量超过
                // circuitBreaker.requestVolumeThreshold 的情况下，如果错误请求数的百分比超过50,
                // 就把断路器设置为 "打开" 状态，否则就设置为 "关闭" 状态。
                @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage", value = "50"),
                // 该属性用来设置当断路器打开之后的休眠时间窗。 休眠时间窗结束之后，
                // 会将断路器置为 "半开" 状态，尝试熔断的请求命令，如果依然失败就将断路器继续设置为 "打开" 状态，
                // 如果成功就设置为 "关闭" 状态。
                @HystrixProperty(name = "circuitBreaker.sleepWindowinMilliseconds", value = "5000"),
                // 断路器强制打开
                @HystrixProperty(name = "circuitBreaker.forceOpen", value = "false"),
                // 断路器强制关闭
                @HystrixProperty(name = "circuitBreaker.forceClosed", value = "false"),
                // 滚动时间窗设置，该时间用于断路器判断健康度时需要收集信息的持续时间
                @HystrixProperty(name = "metrics.rollingStats.timeinMilliseconds", value = "10000"),
                // 该属性用来设置滚动时间窗统计指标信息时划分"桶"的数量，断路器在收集指标信息的时候会根据
                // 设置的时间窗长度拆分成多个 "桶" 来累计各度量值，每个"桶"记录了一段时间内的采集指标。
                // 比如 10 秒内拆分成 10 个"桶"收集这样，所以 timeinMilliseconds 必须能被 numBuckets 整除。否则会抛异常
                @HystrixProperty(name = "metrics.rollingStats.numBuckets", value = "10"),
                // 该属性用来设置对命令执行的延迟是否使用百分位数来跟踪和计算。如果设置为 false, 那么所有的概要统计都将返回 -1。
                @HystrixProperty(name = "metrics.rollingPercentile.enabled", value = "false"),
                // 该属性用来设置百分位统计的滚动窗口的持续时间，单位为毫秒。
                @HystrixProperty(name = "metrics.rollingPercentile.timeInMilliseconds", value = "60000"),
                // 该属性用来设置百分位统计滚动窗口中使用 “ 桶 ”的数量。
                @HystrixProperty(name = "metrics.rollingPercentile.numBuckets", value = "60000"),
                // 该属性用来设置在执行过程中每个 “桶” 中保留的最大执行次数。如果在滚动时间窗内发生超过该设定值的执行次数，
                // 就从最初的位置开始重写。例如，将该值设置为100, 滚动窗口为10秒，若在10秒内一个 “桶 ”中发生了500次执行，
                // 那么该 “桶” 中只保留 最后的100次执行的统计。另外，增加该值的大小将会增加内存量的消耗，并增加排序百分位数所需的计算时间。
                @HystrixProperty(name = "metrics.rollingPercentile.bucketSize", value = "100"),
                // 该属性用来设置采集影响断路器状态的健康快照（请求的成功、 错误百分比）的间隔等待时间。
                @HystrixProperty(name = "metrics.healthSnapshot.intervalinMilliseconds", value = "500"),
                // 是否开启请求缓存
                @HystrixProperty(name = "requestCache.enabled", value = "true"),
                // HystrixCommand的执行和事件是否打印日志到 HystrixRequestLog 中
                @HystrixProperty(name = "requestLog.enabled", value = "true"),
        },
        threadPoolProperties = {
                // 该参数用来设置执行命令线程池的核心线程数，该值也就是命令执行的最大并发量
                @HystrixProperty(name = "coreSize", value = "10"),
                // 该参数用来设置线程池的最大队列大小。当设置为 -1 时，线程池将使用 SynchronousQueue 实现的队列，
                // 否则将使用 LinkedBlockingQueue 实现的队列。
                @HystrixProperty(name = "maxQueueSize", value = "-1"),
                // 该参数用来为队列设置拒绝阈值。 通过该参数， 即使队列没有达到最大值也能拒绝请求。
                // 该参数主要是对 LinkedBlockingQueue 队列的补充,因为 LinkedBlockingQueue
                // 队列不能动态修改它的对象大小，而通过该属性就可以调整拒绝请求的队列大小了。
                @HystrixProperty(name = "queueSizeRejectionThreshold", value = "5"),
        }
)
public String strConsumer() {
    return "hello 2020";
}
public String str_fallbackMethod()
{
    return "*****fall back str_fallbackMethod";
}
~~~

### 9.4、hystrix工作流程

[工作流程](https://github.com/Netflix/Hystrix/wiki/How-it-Works)

Hystrix工作流程

- 官网图例

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020161345.png)
	
- 步骤说明

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020161529.png)
	
	
	
### 9.5、服务监控hystrixDashboard

#### 9.5.1、概述

除了隔离依赖服务的调用以外，Hystrix还提供了**准实时的调用监控（Hystrix Dashboard）**，Hystrix会持续地记录所有通过Hystrix发起的请求的执行信息，并以统计报表和图形的形式展示给用户，包括每秒执行多少请求多少成功，多少失败等。Netflix通过hystrix-metrics-event-stream项目实现了对以上指标的监控。Spring Cloud也提供了Hystrix Dashboard的整合，对监控内容转化成可视化界面。

#### 9.5.2、仪表盘9001

1、新建cloud-consumer-hystrix-dashboard9001

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

    <artifactId>cloud-consumer-hystrix-dashboard9001</artifactId>


    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
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
server:
  port: 9001
~~~

4、新建启动类 HystrixDashboardMain9001+**新注解@EnableHystrixDashboard**

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;

@SpringBootApplication
@EnableHystrixDashboard
public class HystrixDashboardMain9001 {
    public static void main(String[] args)
    {
        SpringApplication.run(HystrixDashboardMain9001.class,args);
    }
}
~~~

5、**所有Provider微服务提供类(8001/8002/8003)都需要监控依赖配置**

~~~xml
<!-- actuator监控信息完善 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

6、启动cloud-consumer-hystrix-dashboard9001该微服务后续将监控微服务8001：http://localhost:9001/hystrix

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020162258.png)

#### 9.5.3、断路器演示(服务监控hystrixDashboard)

1、修改cloud-provider-hystrix-payment8001

- **注意:新版本Hystrix需要在主启动类MainAppHystrix8001中指定监控路径**，否则会报错

~~~Java
/**
 *此配置是为了服务监控而配置，与服务容错本身无关，springcloud升级后的坑
 *ServletRegistrationBean因为springboot的默认路径不是"/hystrix.stream"，
 *只要在自己的项目里配置上下面的servlet就可以了
 */
@Bean
public ServletRegistrationBean getServlet() {
    HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
    ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
    registrationBean.setLoadOnStartup(1);
    registrationBean.addUrlMappings("/hystrix.stream");
    registrationBean.setName("HystrixMetricsStreamServlet");
    return registrationBean;
}
~~~

2、监控测试

- 启动1个eureka或者3个eureka集群均可
- 观察监控窗口
	- 9001监控8001：填写监控地址：http://localhost:8001/hystrix.stream
	- 测试地址
		- http://localhost:8001/payment/circuit/1
		- http://localhost:8001/payment/circuit/-1
		- 先访问正确地址，再访问错误地址，再正确地址，会发现图示断路器都是慢慢放开的
			- 监控结果，成功
			
			![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020164339.png)
			
			- 监控结果，失败
			
			![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020164502.png)

3、如何看懂该图

1. 一圈
	1.  实心圆：共有两种含义。它通过颜色的变化代表了实例的健康程度，它的健康度从**绿色<黄色<橙色<红色递减**
	2.  该实心圆除了颜色的变化之外，它的大小也会根据实例的请求流量发生变化，流量越大该实心圆就越大。所以通过该实心圆的展示，就可以在大量的实例中快速的发现**故障实例和高压力实例**
2. 一线
	1. 曲线：用来记录2分钟内流量的相对变化，可以通过它来观察到流量的上升和下降趋势
3. 整图说明1
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020164742.png)
	
4. 整图说明2
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020164818.png)
	
	
## 10、Gateway新一代网关

### 10.1、概述

[上一代zuul 1.X官网](https://github.com/Netflix/zuul/wiki)

[当前Gateway官网](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/)

#### 10.1.1、Gateway是什么？

1. 概述
	- Gateway是在Spring生态系统之上构建的API网关服务，基于Spring 5，Spring Boot 2和 Project Reactor等技术。
	- Gateway旨在提供一种简单而有效的方式来对API进行路由，以及提供一些强大的过滤器功能， 例如：熔断、限流、重试等
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020171317.png)

	- SpringCloud Gateway 是 SpringCloud 的一个全新项目，基于**Spring 5.0+Spring Boot 2.0 和 Project Reactor**等技术开发的网关，它旨在为微服务架构提供一种简单有效的统一的 API 路由管理方式。
 
	- SpringCloud Gateway 作为 Spring Cloud 生态系统中的网关，目标是替代 Zuul，在Spring Cloud 2.0以上版本中，没有对新版本的Zuul 2.0以上最新高性能版本进行集成，仍然还是使用的Zuul 1.x非Reactor模式的老版本。而为了提升网关的性能，SpringCloud Gateway是**基于WebFlux框架实现的，而WebFlux框架底层则使用了高性能的Reactor模式通信框架Netty。**
 
	- SpringCloud Gateway的目标提供统一的路由方式且基于 Filter 链的方式提供了网关基本的功能，例如：安全，监控/指标，和限流。

2. 一句话
	- **SpringCloud Gateway 使用的Webflux中的reactor-netty响应式编程组件，底层使用了Netty通讯框架**
	- 源码架构
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020171516.png)
	
	
#### 10.1.2、能干嘛？

- 反向代理
- 鉴权
- 流量控制
- 熔断
- 日志监控

#### 10.1.3、微服务架构中网关在哪里

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020171620.png)

#### 10.1.4、有Zuul了怎么又出来了gateway

##### 10.1.4.1、我们为什么选择Gateway？
1. neflix不太靠谱，zuul2.0一直跳票，迟迟不发布
	1.  一方面因为Zuul1.0已经进入了维护阶段，而且Gateway是SpringCloud团队研发的，是亲儿子产品，值得信赖。而且很多功能Zuul都没有用起来也非常的简单便捷。
	2. Gateway是基于**异步非阻塞模型**上进行开发的，性能方面不需要担心。虽然Netflix早就发布了最新的 Zuul 2.x，但 Spring Cloud 貌似没有整合计划。而且Netflix相关组件都宣布进入维护期；不知前景如何？
	3. 多方面综合考虑Gateway是很理想的网关选择。
2. SpringCloud Gateway具有如下特性
	1. **基于Spring Framework 5, Project Reactor 和 Spring Boot 2.0 进行构建**
	2. 动态路由：能够匹配任何请求属性
	3. 可以对路由指定 Predicate（断言）和 Filter（过滤器）
	4. 集成Hystrix的断路器功能
	5. 集成 Spring Cloud 服务发现功能
	6. 易于编写的 Predicate（断言）和 Filter（过滤器）
	7. 请求限流功能
	8. 支持路径重写
3. SpringCloud Gateway 与 Zuul的区别
	1. 在SpringCloud Finchley 正式版之前，SpringCloud 推荐的网关是 Netflix 提供的Zuul
		1. Zuul 1.x，是一个**基于阻塞 I/ O**的 API Gateway
		2. Zuul 1.x **基于Servlet 2. 5使用阻塞架构**它不支持任何长连接(如 WebSocket) Zuul 的设计模式和Nginx较像，每次 I/ O 操作都是从工作线程中选择一个执行，请求线程被阻塞到工作线程完成，但是差别是Nginx 用C++ 实现，Zuul 用 Java 实现，而 JVM 本身会有第一次加载较慢的情况，使得Zuul 的性能相对较差
		3. Zuul 2.x理念更先进，想基于Netty非阻塞和支持长连接，但SpringCloud目前还没有整合。 Zuul 2.x的性能较 Zuul 1.x 有较大提升。在性能方面，根据官方提供的基准测试， Spring Cloud Gateway 的 RPS（每秒请求数）是Zuul 的 1. 6 倍。
		4. SpringCloud Gateway 建立 在 `Spring Framework 5、 Project Reactor 和 Spring Boot 2` 之上， 使用非阻塞 API
		5. SpringCloud Gateway 还 支持 WebSocket， 并且与Spring紧密集成拥有更好的开发体验


##### 10.1.4.2、Zuul1.x模型

1. Springcloud中所集成的Zuul版本，采用的是Tomcat容器，使用的是传统的Servlet IO处理模型
2. 学过尚硅谷web中期课程都知道一个题目，**Servlet的生命周期?** servlet由servlet container进行生命周期管理。
	1. container启动时构造servlet对象并调用servlet init()进行初始化；
	2. container运行时接受请求，并为每个请求分配一个线程（一般从线程池中获取空闲线程）然后调用service()。
	3. container关闭时调用servlet destory()销毁servlet；
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020173823.png)
	
3. `上述模式的缺点`
	- servlet是一个简单的网络IO模型，当请求进入servlet container时，servlet container就会为其绑定一个线程，在**并发不高的场景下**这种模型是适用的。但是一旦高并发(比如抽风用jemeter压)，线程数量就会上涨，而线程资源代价是昂贵的（上线文切换，内存消耗大）严重影响请求的处理时间。在一些简单业务场景下，不希望为每个request分配一个线程，只需要1个或几个线程就能应对极大并发的请求，这种业务场景下servlet模型没有优势
	- 所以Zuul 1.X是**基于servlet之上的一个阻塞式处理模型**，即spring实现了处理所有request请求的一个servlet（DispatcherServlet）并由该servlet阻塞式处理处理。所以Springcloud Zuul无法摆脱servlet模型的弊端

##### 10.1.4.3、GateWay模型
1. WebFlux是什么?[官网说明](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#webflux-new-framework)
2. 说明
	1. 传统的Web框架，比如说：struts2，springmvc等都是基于Servlet API与Servlet容器基础之上运行的
	2. 但是**在Servlet3.1之后有了异步非阻塞的支持**。而WebFlux是一个典型`非阻塞异步的框架`，它的核心是**基于Reactor的相关API实现的**。相对于传统的web框架来说，它可以运行在诸如Netty，Undertow及支持Servlet3.1的容器上。非阻塞式+函数式编程（Spring5必须让你使用java8）
	3. Spring WebFlux 是 Spring 5.0 引入的新的响应式框架，区别于 Spring MVC，它不需要依赖Servlet API，**它是完全异步非阻塞的**，并且基于 Reactor 来实现响应式流规范

### 10.2、三大核心概念

- Route(路由)
	- 路由是构建网关的基本模块，它由`ID，目标URI，一系列的断言和过滤器组成`，**如果断言为true则匹配该路由**
- Predicate(断言)
	- 参考的是Java8的java.util.function.Predicate,开发人员可以匹配HTTP请求中的所有内容(例如请求头或请求参数)，**如果请求与断言相匹配则进行路由**
- Filter(过滤)
	- 指的是Spring框架中GatewayFilter的实例，使用过滤器，可以在**请求被路由前或者之后**对请求进行修改。
- 总体
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020195903.png)
	- web请求，通过一些匹配条件，定位到真正的服务节点。并在这个转发过程的前后，进行一些精细化控制
	- predicate就是我们的匹配条件
	- 而filter，就可以理解为一个无所不能的拦截器。有了这两个元素，再加上目标uri，就可以实现一个具体的路由了

### 10.3、Gateway工作流程

1. 官网总结

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020200034.png)
	
	1. 客户端向 SpringCloud Gateway 发出请求。然后在 Gateway Handler Mapping 中找到与请求相匹配的路由，将其发送到 Gateway Web Handler
	2. Handler 再通过指定的过滤器链来将请求发送到我们实际的服务执行业务逻辑，然后返回
	3. 过滤器之间用虚线分开是因为过滤器可能会在**发送代理请求之前(“pre”)** 或**之后(“post”)** 执行业务逻辑
	4. Filter在`pre`类型的过滤器可以做参数校验、权限校验、流量监控、日志输出、协议转换等
	5. 在`post`类型的过滤器中可以做响应内容、响应头的修改，日志的输出，流量监控等有着非常重要的作用
2. 核心逻辑
	- **路由转发+执行过滤器链** 

### 10.4、入门配置

1、新建 cloud-gateway-gateway9527

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

    <artifactId>cloud-gateway-gateway9527</artifactId>


    <dependencies>
        <!--gateway-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
        <!--eureka-client-->
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
        <!--一般基础配置类-->
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
  port: 9527

spring:
  application:
    name: cloud-gateway

eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class GateWayMain9527 {
    public static void main(String[] args)
    {
        SpringApplication.run(GateWayMain9527.class,args);
    }
}
~~~

5、9527网关如何做路由映射那？？？
- cloud-provider-payment8001看看controller的访问地址
	- get
	- lb
- 我们目前不想暴露8001端口，希望在8001外面套一层9527

6、YML新增网关配置

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          uri: http://localhost:8001          #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          uri: http://localhost:8001          #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由

eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
~~~

7、测试

1. 启动7001
2. 启动cloud-provider-payment8001
3. 启动9527网关
4. 访问说明
	1. 添加网关前：http://localhost:8001/payment/get/1
	2. 添加网关后：http://localhost:9527/payment/get/1

8、**YML配置说明**
- Gateway网关路由有两种配置方式
	1. 在配置文件yml中配置
	2. 代码中注入RouteLocator的Bean
		1. 自己写一个
			1. 业务需求：通过9527网关访问到外网的百度新闻网址
			2. 编码
				1. 业务实现：config

				~~~Java
				package com.clover.springcloud.config;

				import org.springframework.cloud.gateway.route.RouteLocator;
				import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
				import org.springframework.context.annotation.Bean;
				import org.springframework.context.annotation.Configuration;

				@Configuration
				public class GateWayConfig {

					/**
					 * 配置了一个id为route-name的路由规则，
					 * 当访问地址 http://localhost:9527/guonei时会自动转发到地址：http://news.baidu.com/guonei
					 * @param routeLocatorBuilder
					 * @return
					 */
					@Bean
					public RouteLocator custom(RouteLocatorBuilder routeLocatorBuilder)
					{
						RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
						routes.route("path_route_clover",r -> r.path("/guonei").uri("http://news.baidu.com/guonei")).build();
						return routes.build();
					}
				}
				~~~
				
### 10.5、通过微服务名实现动态路由
1. 默认情况下Gateway会根据注册中心注册的服务列表，以注册中心上微服务名为路径创建**动态路由进行转发，从而实现动态路由的功能**
2. 启动：一个eureka7001 + 两个服务提供者8001/8002
3. 修改POM

~~~xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
~~~
4. 修改YML
	1. **需要注意的是uri的协议为lb，表示启用Gateway的负载均衡功能**
	2. `lb://serviceName`是spring cloud gateway在微服务中自动为我们创建的负载均衡uri

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能，利用微服务名进行路由
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由

eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
~~~
5. 测试: http://localhost:9527/payment/lb   8001/8002两个端口切换

### 10.6、Predicate的使用

#### 10.6.1、Predicate是什么？

启动我们的gateway9527

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020213340.png)


#### 10.6.2、Route Predicate Factories这个是什么东东?

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020213415.png)

- Spring Cloud Gateway将路由匹配作为Spring WebFlux HandlerMapping基础架构的一部分。
- Spring Cloud Gateway包括许多内置的Route Predicate工厂。所有这些Predicate都与HTTP请求的不同属性匹配。多个Route Predicate工厂可以进行组合
 
- Spring Cloud Gateway 创建 Route 对象时， 使用 RoutePredicateFactory 创建 Predicate 对象，Predicate 对象可以赋值给 Route。
-  Spring Cloud Gateway 包含许多内置的Route Predicate Factories。
 
- 所有这些谓词都匹配HTTP请求的不同属性。多种谓词工厂可以组合，并通过逻辑and。

#### 10.6.3、常用的Route Predicate

1、After Route Predicate

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020213842.png)

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
		   - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
		   - After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由


eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
~~~

2、Before Route Predicate

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
            - After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            - Before=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由


eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
~~~

3、Between Route Predicate

~~~yml
 
- Between=2019-12-02T17:45:06.206+08:00[Asia/Shanghai],2019-12-02T18:59:06.206+08:00[Asia/Shanghai]
 
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
            #- After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Before=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            - Between=2021-10-21T13:37:41.094+08:00[Asia/Shanghai],2021-10-21T14:37:41.094+08:00[Asia/Shanghai]


eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka

#id：我们自定义的路由 ID，保持唯一
 ##uri：目标服务地址
 ##predicates：路由条件，Predicate接受一个输入参数返回一个布尔值。
 ##            该属性包含多种默认方法来将Predicate组合成其他复杂的逻辑(比如：与，或，非)
~~~

4、Cookie Route Predicate

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
            - After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Before=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Between=2021-10-21T13:37:41.094+08:00[Asia/Shanghai],2021-10-21T14:37:41.094+08:00[Asia/Shanghai]
        - Cookie=username,clover


eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka

#id：我们自定义的路由 ID，保持唯一
 ##uri：目标服务地址
 ##predicates：路由条件，Predicate接受一个输入参数返回一个布尔值。
 ##            该属性包含多种默认方法来将Predicate组合成其他复杂的逻辑(比如：与，或，非)
~~~

5、Header Route Predicate

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
            - After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Before=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Between=2021-10-21T13:37:41.094+08:00[Asia/Shanghai],2021-10-21T14:37:41.094+08:00[Asia/Shanghai]
            #- Cookie=username,clover
            - Header=X-Request-Id, \d+  # 请求头要有X-Request-Id属性并且值为整数的正则表达式
~~~

`curl http://localhost:9588/paymentInfo -H "X-Request-Id:123"`：错误

`curl http://localhost:9527/payment/lb -H "X-Request-Id:123"`：正确

6、Host Route Predicate

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
            - After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Before=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Between=2021-10-21T13:37:41.094+08:00[Asia/Shanghai],2021-10-21T14:37:41.094+08:00[Asia/Shanghai]
            #- Cookie=username,clover
            #- Header=X-Request-Id, \d+  # 请求头要有X-Request-Id属性并且值为整数的正则表达式
            - Host=**.clover.com


eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka

#id：我们自定义的路由 ID，保持唯一
 ##uri：目标服务地址
 ##predicates：路由条件，Predicate接受一个输入参数返回一个布尔值。
 ##            该属性包含多种默认方法来将Predicate组合成其他复杂的逻辑(比如：与，或，非)
~~~

**正确**：`curl http://localhost:9527/payment/lb -H "Host: www.clover.com"`
**正确**：`curl http://localhost:9527/payment/lb -H "Host: java.clover.com"`
**错误**：`curl http://localhost:9527/payment/lb -H "Host: java.clover.net"`

7、Method Route Predicate

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
            - After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Before=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Between=2021-10-21T13:37:41.094+08:00[Asia/Shanghai],2021-10-21T14:37:41.094+08:00[Asia/Shanghai]
            #- Cookie=username,clover
            #- Header=X-Request-Id, \d+  # 请求头要有X-Request-Id属性并且值为整数的正则表达式
            #- Host=**.clover.com
            - Method=GET


eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka

#id：我们自定义的路由 ID，保持唯一
 ##uri：目标服务地址
 ##predicates：路由条件，Predicate接受一个输入参数返回一个布尔值。
 ##            该属性包含多种默认方法来将Predicate组合成其他复杂的逻辑(比如：与，或，非)
~~~

8、Path Route Predicate

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
            - After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Before=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Between=2021-10-21T13:37:41.094+08:00[Asia/Shanghai],2021-10-21T14:37:41.094+08:00[Asia/Shanghai]
            #- Cookie=username,clover
            #- Header=X-Request-Id, \d+  # 请求头要有X-Request-Id属性并且值为整数的正则表达式
            #- Host=**.clover.com
            - Method=GET


eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka

#id：我们自定义的路由 ID，保持唯一
 ##uri：目标服务地址
 ##predicates：路由条件，Predicate接受一个输入参数返回一个布尔值。
 ##            该属性包含多种默认方法来将Predicate组合成其他复杂的逻辑(比如：与，或，非)
~~~

9、Query Route Predicate

~~~yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
           - After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Before=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Between=2021-10-21T13:37:41.094+08:00[Asia/Shanghai],2021-10-21T14:37:41.094+08:00[Asia/Shanghai]
            #- Cookie=username,clover
            #- Header=X-Request-Id, \d+  # 请求头要有X-Request-Id属性并且值为整数的正则表达式
            #- Host=**.clover.com
            - Method=GET
            - Query=username, \d+  # 要有参数名username并且值还要是整数才能路由


eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka

#id：我们自定义的路由 ID，保持唯一
 ##uri：目标服务地址
 ##predicates：路由条件，Predicate接受一个输入参数返回一个布尔值。
 ##            该属性包含多种默认方法来将Predicate组合成其他复杂的逻辑(比如：与，或，非)
~~~

**正确**：`curl http://localhost:9527/payment/lb?username=1`
 
**错误**：`curl http://localhost:9527/payment/lb?username=-1`


10、小总结
- ALL

~~~xml
 server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
            - After=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Before=2021-10-21T13:37:41.094+08:00[Asia/Shanghai]         # 断言，路径相匹配的进行路由
            #- Between=2021-10-21T13:37:41.094+08:00[Asia/Shanghai],2021-10-21T14:37:41.094+08:00[Asia/Shanghai]
            #- Cookie=username,clover
            #- Header=X-Request-Id, \d+  # 请求头要有X-Request-Id属性并且值为整数的正则表达式
            #- Host=**.clover.com
            - Method=GET
            - Query=username, \d+  # 要有参数名username并且值还要是整数才能路由


eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka

#id：我们自定义的路由 ID，保持唯一
 ##uri：目标服务地址
 ##predicates：路由条件，Predicate接受一个输入参数返回一个布尔值。
 ##            该属性包含多种默认方法来将Predicate组合成其他复杂的逻辑(比如：与，或，非)
~~~
- 说白了，Predicate就是为了实现一组匹配规则，让请求过来找到对应的Route进行处理。

### 10.7、Filter的使用 
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020215804.png)
1. 是什么？
2. Spring Cloud Gateway的Filter
	- **生命周期，Only Two**
		- pre
		- post
	- **种类，Only Two**
		- GatewayFilter 
			
			[官网地址](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/#the-addrequestparameter-gatewayfilter-factory)
			
			![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020220240.png)
			
		- GlobalFilter
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211020220258.png)
		
3. 常用的GatewayFilter
	- AddRequestParameter
	
	~~~yml

	server:
	  port: 9588

	spring:
	  application:
		name: cloud-gateway
	  cloud:
		gateway:
		  discovery:
			locator:
			  enabled: true #开启从注册中心动态创建路由的功能
			  lower-case-service-id: true #使用小写服务名，默认是大写
		  routes:
			- id: payment_routh #payment_route #路由的ID，没有固定规则但要求唯一，建议配合服务名
			  uri: lb://cloud-provider-payment #匹配后的目标服务地址，供服务的路由地址
			  #uri: http://localhost:8001 #匹配后提供服务的路由地址
			  filters:
				- AddRequestParameter=X-Request-Id,1024 #过滤器工厂会在匹配的请求头加上一对请求头，名称为X-Request-Id值为1024
			  predicates:
				- Path=/paymentInfo/**        # 断言，路径相匹配的进行路由
				- Method=GET,POST

	eureka:
	  instance:
		hostname: cloud-gateway-service
	  client: #服务提供者provider注册进eureka服务列表内
		service-url:
		  register-with-eureka: true
		  fetch-registry: true
		  defaultZone: http://eureka7001.com:7001/eureka
	~~~
	
4. 自定义过滤器
	- 自定义全局GlobalFilter
		- 两个主要接口介绍：**GlobalFilter,Ordered**
		- 能干嘛
			- 全局日志记录
			- 统一网关鉴权
		- 案例代码
			~~~Java
			package com.clover.springcloud.filter;

			import lombok.extern.slf4j.Slf4j;
			import org.springframework.cloud.gateway.filter.GatewayFilterChain;
			import org.springframework.cloud.gateway.filter.GlobalFilter;
			import org.springframework.core.Ordered;
			import org.springframework.http.HttpStatus;
			import org.springframework.stereotype.Component;
			import org.springframework.web.server.ServerWebExchange;
			import reactor.core.publisher.Mono;

			import java.util.Date;

			@Component
			@Slf4j
			public class MyLogGateWayFilter implements GlobalFilter, Ordered {
				@Override
				public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
					log.info("time:"+new Date()+"\t 执行了自定义的全局过滤器: "+"MyLogGateWayFilter"+"hello");
					String uname = exchange.getRequest().getQueryParams().getFirst("uname");
					if(uname == null)
					{
						log.info("****用户名为null，无法登录");
						exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);
						return exchange.getResponse().setComplete();
					}
					return chain.filter(exchange);
				}

				// 加载过滤器的顺序，一般数字越小，优先级越高
				@Override
				public int getOrder() {
					return 0;
				}
			}
			~~~
		- 测试
			- 正确：http://localhost:9527/payment/lb?uname=clover
			- 错误
				- 没有参数uname
				- http://localhost:9527/payment/lb：无法正常使用转发



## 11、SpringCloud Config分布式配置中心

### 11.1、概述

1. 分布式系统面临的---配置问题
	- 微服务意味着要将单体应用中的业务拆分成一个个子服务，每个服务的粒度相对较小，因此系统中会出现大量的服务。由于每个服务都需要必要的配置信息才能运行，所以一套**集中式的、动态的配置管理**设施是必不可少的。
 
	- SpringCloud提供了ConfigServer来解决这个问题，我们每一个微服务自己带着一个application.yml，上百个配置文件的管理..
2. 是什么
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211021162538.png) 
	
	- SpringCloud Config为微服务架构中的微服务提供集中化的外部配置支持，配置服务器为**各个不同微服务应用**的所有环境提供了一个**中心化的外部配置**
	- SpringCloud Config分为**服务端和客户端两部分**
		- 服务端也称为**分布式配置中心**，**它是一个独立的微服务应用**，用来连接配置服务器并为客户端提供获取配置信息，加密/解密信息等访问接口
		- 客户端则是通过指定的配置中心来管理应用资源，以及与业务相关的配置内容，并在启动的时候从配置中心获取和加载配置信息配置服务器默认采用git来存储配置信息，这样就有助于对环境配置进行版本管理，并且可以通过git客户端工具来方便的管理和访问配置内容
3. 能干嘛
	- 集中管理配置文件
	- 不同环境不同配置，动态化的配置更新，分环境部署比如dev/test/prod/beta/release
	- 运行期间动态调整配置，不再需要在每个服务部署的机器上编写配置文件，服务会向配置中心统一拉取配置自己的信息
	- 当配置发生变动时，服务不需要重启即可感知到配置的变化并应用新的配置
	- 将配置信息以REST接口的形式暴露
4. 与GitHub整合配置
	- 由于SpringCloud Config默认使用Git来存储配置文件(也有其它方式,比如支持SVN和本地文件)，但最推荐的还是Git，而且使用的是http/https访问的形式

### 11.2、Config服务端配置与测试

1、用你自己的账号在GitHub上新建一个名为springcloud-config的新Repository

2、由上一步获得刚新建的git地址：`https://github.com/cloverfelix/springcloud-config.git`

3、新建Module模块 cloud-config-center-3344 它即为Cloud的配置中心模块cloudConfig Center

4、修改POM

~~~yml
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

    <artifactId>cloud-config-center-3344</artifactId>


    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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

5、编写YML

~~~yml
server:
  port: 3344

spring:
  application:
    name:  cloud-config-center #注册进Eureka服务器的微服务名
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/ren_zuo_wei/springcloud-config.git #Gitee上面的git仓库名字
          ####搜索目录
          search-paths:
            - springcloud-config
      ####读取分支
      label: master

#服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
~~~

6、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class ConfigCenterMain3344 {
    public static void main(String[] args)
    {
        SpringApplication.run(ConfigCenterMain3344.class,args);
    }
}
~~~

7、测试通过Config微服务是否可以从GitHub上获取配置内容

http://localhost:3344/master/config-dev.yml

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211021174221.png)

**大坑！！！！！！**

**现在在GitHub中创建一个新的仓库的时候，默认分支为main分支，需要你自己手动创建一个master分支，并且，访问uri使用HTTP，不使用SSL，使用SSL会报身份验证问题**

[参考博客](https://www.cxybb.com/article/weixin_51578439/109579295)

配置读取规则
- 官网
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211021174455.png)
- /{label}/{application}-{profile}.yml
	- master分支
		- http://localhost:3344/master/config-dev.yml
		- http://localhost:3344/master/config-test.yml
		- http://localhost:3344/master/config-prod.yml
	- dev分支
		- http://localhost:3344/dev/config-dev.yml
- /{application}-{profile}.yml
	- http://localhost:3344/config-dev.yml
- /{application}/{profile}[/{label}]
	- http://localhost:3344/config/dev/master

`重要配置细节总结`
- /{name}-{profiles}.yml
- /{label}-{name}-{profiles}.yml

		label：分支(branch)
		name ：服务名
		profiles：环境(dev/test/prod)

### 11.3、Config客户端配置与测试

1、新建cloud-config-client-3355

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

    <artifactId>cloud-config-client-3355</artifactId>


    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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

3、新建`bootstrap.yml`

- bootstrap.yml是什么？
	- applicaiton.yml是**用户级**的资源配置项
	- bootstrap.yml是**系统级的**，**优先级更加高**

- SpringCloud会创建一个“Bootstrap Context”，作为Spring应用的`Application Context`的父上下文。初始化的时候，`Bootstrap Context`负责**从外部源加载配置属性并解析配置**。这两个上下文共享一个从外部获取的`Environment`
- `Bootstrap`属性有高优先级，默认情况下，它们不会被本地配置覆盖。 `Bootstrap context`和`Application Context`有着不同的约定，所以新增了一个`bootstrap.yml`文件，保证`Bootstrap Context`和`Application Context`配置的分离
- **要将Client模块下的application.yml文件改为bootstrap.yml,这是很关键的**，因为bootstrap.yml是比application.yml先加载的。bootstrap.yml优先级高于application.yml

~~~yml
server:
  port: 3355

spring:
  application:
    name: config-client
  cloud:
    #Config客户端配置
    config:
      label: master #分支名称
      name: config #配置文件名称
      profile: dev #读取后缀名称   上述3个综合：master分支上config-dev.yml的配置文件被读取http://config-3344.com:3344/master/config-dev.yml
      uri: http://localhost:3344 #配置中心地址k

#服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class ConfigClientMain3355 {
    public static void main(String[] args)
    {
        SpringApplication.run(ConfigClientMain3355.class,args);
    }
}
~~~

5、编写业务类

~~~Java
package com.clover.springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ConfigClientController {
    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo()
    {
        return configInfo;
    }
}
~~~

6、测试
1. 启动Config配置中心3344微服务并自测：http://localhost:3344/master/config-dev.yml
2. 启动3355作为Client准备访问：http://localhost:3355/configInfo

**成功实现了客户端3355访问SpringCloud Config3344通过GitHub获取配置信息**

`问题随时而来，分布式配置的动态刷新问题`
- Linux运维修改GitHub上的配置文件内容做调整
- 刷新3344，发现ConfigServer配置中心立刻响应
- 刷新3355，发现ConfigClient客户端没有任何响应
- 3355没有变化除非自己重启或者重新加载
- 难到每次运维修改配置文件，客户端都需要重启？？噩梦

### 11.4、Config客户端之动态刷新

1、动态刷新
1. 修改3355模块
2. POM引入actuator监控

	~~~xml
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-actuator</artifactId>
	</dependency>
	~~~
3. 修改YML，暴露监控端口

	~~~yml
	# 暴露监控端点
	management:
	  endpoints:
		web:
		  exposure:
			include: "*"
	~~~
4. 业务类Controller上加 **@RefreshScope注解**
5. 此时修改GitHub-->访问3344-->访问3355
	- 但是，此时3355并没有改变
6. 这就需要运维人员在每次修改完毕后发送POST请求刷新3355
	1. 必须是POST请求
	2. curl -X POST "http://localhost:3355/actuator/refresh"
7. 再次访问，成功实现了客户端3355刷新到最新配置内容，避免了服务重启

２、想想还有什么问题？
- 假如有多个微服务客户端3355/3366/3377。。。。。。
- 每个微服务都要执行一次post请求，手动刷新？
- **可否广播，一次通知，处处生效？**
- 我们想大范围的自动刷新，求方法


## 12、SpringCloud Bus消息总线

### 12.1、概述
1. 对Config的加深和扩充
	- SpringCloud Bus 配合 SpringCloud Config 使用可以实现配置的动态刷新
2. Bus是什么？
	- Bus支持两种消息代理：RabbitMQ 和 Kafka

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211021213539.png)

Spring Cloud Bus是用来将分布式系统的节点与轻量级消息系统链接起来的框架，**它整合了Java的事件处理机制和消息中间件的功能。**

3. Bus能干嘛？
	- SpringCloud Bus能管理和传播分布式系统间的消息，就像一个分布式执行器，可用于广播状态更改、事件推送等，也可以当作微服务间的通信通道。
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211021213721.png)

4. 为何被称为总线
	1. 什么是总线
		- 在微服务架构的系统中，通常会使用**轻量级的消息代理**来构建一个**共用的消息主题**，并让系统中所有微服务实例都连接上来。由于**该主题中产生的消息会被所有实例监听和消费，所以称它为消息总线**。在总线上的各个实例，都可以方便地广播一些需要让其他连接在该主题上的实例都知道的消息。
	2. 基本原理
		- ConfigClient实例都监听**MQ中同一个topic(默认是springCloudBus)**。当一个服务刷新数据的时候，它会把这个信息放入到Topic中，这样其它监听同一Topic的服务就能得到通知，然后去更新自身的配置。


### 12.2、SpringCloud Bus动态刷新全局广播

1、必须先具备良好的Rabbit MQ环境先

2、新建 cloud-config-client-3366

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

    <artifactId>cloud-config-client-3366</artifactId>


    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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

4、编写YML

~~~yml
server:
  port: 3366

spring:
  application:
    name: config-client
  cloud:
    #Config客户端配置
    config:
      label: master #分支名称
      name: config #配置文件名称
      profile: dev #读取后缀名称   上述3个综合：master分支上config-dev.yml的配置文件被读取http://config-3344.com:3344/master/config-dev.yml
      uri: http://localhost:3344 #配置中心地址

#服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka

# 暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"
~~~

5、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class ConfigClientMain3366 {
    public static void main(String[] args)
    {
        SpringApplication.run(ConfigClientMain3366.class,args);
    }
}
~~~

6、编写 controller

~~~Java
package com.clover.springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RefreshScope
public class ConfigClientController {

    @Value("${server.port}")
    private String serverPort;

    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo()
    {
        return "serverPort: " + serverPort + "\t\n\n configInfo：" + configInfo;
    }
}
~~~

7、设计思想
1. 利用消息总线触发一个`客户端/bus/refresh`,而刷新所有客户端的配置
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022111007.png) 
	
2. 利用消息总线触发一个`服务端ConfigServer的/bus/refresh端点`，而刷新所有客户端的配置
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022111038.png)
	
3. 图二的架构显然更加适合，图一不适合的原因如下
	1. 打破了微服务的职责单一性，因为微服务本身是业务模块，它本不应该承担配置刷新的职责。
	2. 破坏了微服务各节点的对等性。
	3. 有一定的局限性。例如，微服务在迁移时，它的网络地址常常会发生变化，此时如果想要做到自动刷新，那就会增加更多的修改


8、给cloud-config-center-3344配置中心**服务端**添加消息总线支持

~~~xml
<!--添加消息总线RabbitMQ支持-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

~~~yml
server:
  port: 3344

spring:
  application:
    name:  cloud-config-center #注册进Eureka服务器的微服务名
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/ren_zuo_wei/springcloud-config.git #Gitee上面的git仓库名字
          ####搜索目录
          search-paths:
            - springcloud-config
      ####读取分支
      label: master

  #rabbitmq相关配置
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest

#服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka



##rabbitmq相关配置,暴露bus刷新配置的端点
management:
  endpoints: #暴露bus刷新配置的端点
    web:
      exposure:
        include: 'bus-refresh'
~~~

9、给cloud-config-client-3355**客户端**添加消息总线支持

~~~xml
<!--添加消息总线RabbitMQ支持-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

~~~yml
server:
  port: 3355

spring:
  application:
    name: config-client
  cloud:
    #Config客户端配置
    config:
      label: master #分支名称
      name: config #配置文件名称
      profile: dev #读取后缀名称   上述3个综合：master分支上config-dev.yml的配置文件被读取http://config-3344.com:3344/master/config-dev.yml
      uri: http://localhost:3344 #配置中心地址

  #rabbitmq相关配置 15672是Web管理界面的端口；5672是MQ访问的端口
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest

#服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
# 暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"
~~~

10、给cloud-config-client-3366**客户端**添加消息总线支持

~~~xml
<!--添加消息总线RabbitMQ支持-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

~~~yml
server:
  port: 3366

spring:
  application:
    name: config-client
  cloud:
    #Config客户端配置
    config:
      label: master #分支名称
      name: config #配置文件名称
      profile: dev #读取后缀名称   上述3个综合：master分支上config-dev.yml的配置文件被读取http://config-3344.com:3344/master/config-dev.yml
      uri: http://localhost:3344 #配置中心地址

  #rabbitmq相关配置 15672是Web管理界面的端口；5672是MQ访问的端口
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest

#服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka

# 暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"
~~~

11、测试
- 运维工程师
	- 修改Gitee上配置文件增加版本号
	- 发送POST请求：curl -X POST "http://localhost:3344/actuator/bus-refresh"
	- 一次发送，处处生效
- 配置中心自测：http://localhost:3344/master/config-dev.yml
- 客户端
	- http://localhost:3355/configInfo
	- http://localhost:3366/configInfo
	- 获取配置信息，发现都以及刷新了

### 12.3、SpringCloud Bus动态刷新定点通知

1. 不想全部通知，只想定点通知
	1. 只通知3355	
	2. 不通知3366
2. 简单一句话
	1. 指定具体某一个实例生效而不是全部 
	2. 公式：http://localhost:配置中心的端口号/actuator/bus-refresh/{destination}
	3. /bus/refresh请求不再发送到具体的服务实例上，而是发给config server并通过destination参数类指定需要更新配置的服务或实例
3. 案例
	1. 我们这里以刷新运行在3355端口上的config-client为例
		1. 只通知3355
		2. 不通知3366
	2. curl -X POST "http://localhost:3344/actuator/bus-refresh/config-client:3355"
	3. **destination--->在eureka上的微服务名:端口号**
4. 通知总结All

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022112431.png)

## 13、SpringCloud Stream

### 13.1、消息驱动概述

#### 13.1.1、Stream是什么？

一句话：**屏蔽底层消息中间件的差异,降低切换成本，统一消息的编程模型**

#### 13.1.2、什么是SpringCloudStream

官方定义 SpringCloud Stream 是一个`构建消息驱动微服务的框架`。
 
应用程序通过 inputs 或者 outputs 来与 SpringCloud Stream中binder对象交互。
通过我们配置来binding(绑定) ，而 Spring Cloud Stream 的 binder对象负责与消息中间件交互。
所以，我们只需要搞清楚如何与 Spring Cloud Stream 交互就可以方便使用消息驱动的方式。
 
通过使用Spring Integration来连接消息代理中间件以实现消息事件驱动。
Spring Cloud Stream 为一些供应商的消息中间件产品提供了个性化的自动化配置实现，引用了**发布-订阅、消费组、分区**的三个核心概念。
 
**目前仅支持RabbitMQ、Kafka**

[SpringCloud Stream概述](https://spring.io/projects/spring-cloud-stream#overview)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022155740.png)

[SpringCloud Stream大纲](https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/3.0.1.RELEASE/reference/html/)

[SpringCloud Stream中文指导手册](https://m.wang1314.com/doc/webapp/topic/20971999.html)

#### 13.1.3、设计思想

##### 13.1.3.1、标准MQ

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022161453.png)

- 生产者/消费者之间靠**消息**媒介传递信息内容
	- Message
- 消息必须走特定的**通道**
	- 消息通道MessageChannel
- 消息通道里的消息如何被消费呢，谁负责收发**处理**
	- 消息通道MessageChannel的子接口SubscribableChannel，由MessageHandler消息处理器所订阅

##### 13.1.3.2、为什么用Cloud Stream
1. 比方说我们用到了`RabbitMQ和Kafka`，由于这两个消息中间件的架构上的不同，像**RabbitMQ有exchange，kafka有Topic和Partitions分区**

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022161718.png)
	
2. 这些中间件的差异性导致我们实际项目开发给我们造成了一定的困扰，我们如果用了两个消息队列的其中一种，后面的业务需求，我想往另外一种消息队列进行迁移，这时候无疑就是一个灾难性的，**一大堆东西都要重新推倒重新做**，因为它跟我们的系统耦合了，这时候springcloud Stream给我们提供了一种解耦合的方式

##### 13.1.3.3、stream凭什么可以统一底层差异

- 在没有绑定器这个概念的情况下，我们的SpringBoot应用要直接与消息中间件进行信息交互的时候
- 由于各消息中间件构建的初衷不同，它们的实现细节上会有较大的差异性
- 通过定义绑定器作为中间层，完美地实现了**应用程序与消息中间件细节之间的隔离**
- 通过向应用程序**暴露统一的Channel通道**，使得应用程序不需要再考虑各种不同的消息中间件实现
 
- **通过定义绑定器Binder作为中间层，实现了应用程序与消息中间件细节之间的隔离**

##### 13.1.3.4、Binder

在没有绑定器这个概念的情况下，我们的SpringBoot应用要直接与消息中间件进行信息交互的时候，由于各消息中间件构建的初衷不同，它们的实现细节上会有较大的差异性，通过定义绑定器作为中间层，完美地实现了**应用程序与消息中间件细节之间的隔离**。Stream对消息中间件的进一步封装，可以做到代码层面对中间件的无感知，甚至于动态的切换中间件(rabbitmq切换为kafka)，使得微服务开发的高度解耦，服务可以关注更多自己的业务流程

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022163144.png)

**通过定义绑定器Binder作为中间层，实现了应用程序与消息中间件细节之间的隔离**
 
Binder可以生成Binding，Binding用来绑定消息容器的生产者和消费者，它有两种类型，INPUT和OUTPUT，`INPUT对应于消费者，OUTPUT对应于生产者`

##### 13.1.3.5、Stream中的消息通信方式遵循了发布-订阅模式

Topic主题进行广播
- 在RabbitMQ就是Exchange
- 在Kakfa中就是Topic

#### 13.1.4、Spring Cloud Stream标准流程套路

- Binder
	- 很方便的连接中间件，屏蔽差异
- Channel
	- 通道，是队列Queue的一种抽象，在消息通讯系统中就是实现存储和转发的媒介，通过Channel对队列进行配置
- Source和Sink
	- 简单的可理解为参照对象是Spring Cloud Stream自身，从Stream发布消息就是输出，接受消息就是输入

#### 13.1.5、编码API和常用注解

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022164241.png)

### 13.2、案例说明

1. RabbitMQ环境已经OK
2. 工程中新建三个子模块
	- cloud-stream-rabbitmq-provider8801， 作为生产者进行发消息模块
	- cloud-stream-rabbitmq-consumer8802，作为消息接收模块
	- cloud-stream-rabbitmq-consumer8803，作为消息接收模块

### 13.3、消息驱动之生产者

1、新建 cloud-stream-rabbitmq-provider8801

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

    <artifactId>cloud-stream-rabbitmq-provider8801</artifactId>


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
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
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
  port: 8801

spring:
  application:
    name: cloud-stream-provider
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置rabbitmq的相关的环境配置
            spring:
              rabbitmq:
                host: localhost
                port: 5672
                username: guest
                password: guest
      bindings: # 服务的整合处理
        output: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
          binder: defaultRabbit # 设置要绑定的消息服务的具体设置

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://localhost:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: send-8801.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StreamMQMain8801 {
    public static void main(String[] args)
    {
        SpringApplication.run(StreamMQMain8801.class,args);
    }
}
~~~

5、编写业务类
1. 发送消息接口

	~~~Java
	package com.clover.springcloud.service;

	public interface MyMessageProvider {
		public String send();
	}
	~~~

2. 发送消息接口实现类

	~~~Java
	package com.clover.springcloud.service.Impl;

	import com.clover.springcloud.service.MyMessageProvider;
	import org.springframework.cloud.stream.annotation.EnableBinding;
	import org.springframework.cloud.stream.messaging.Source;
	import org.springframework.integration.support.MessageBuilder;
	import org.springframework.messaging.MessageChannel;

	import javax.annotation.Resource;
	import java.util.UUID;

	@EnableBinding(Source.class) // 可以理解为是一个消息的发送管道的定义
	public class MyMessageProviderImpl implements MyMessageProvider {

		@Resource
		private MessageChannel output;// 消息的发送管道

		@Override
		public String send() {
			String serial = UUID.randomUUID().toString();
			this.output.send(MessageBuilder.withPayload(serial).build());
			System.out.println("*********sreial：" + serial);
			return serial;
		}
	}
	~~~

3. Controller

	~~~Java
	package com.clover.springcloud.controller;

	import com.clover.springcloud.service.MyMessageProvider;
	import org.springframework.web.bind.annotation.GetMapping;
	import org.springframework.web.bind.annotation.RestController;

	import javax.annotation.Resource;

	@RestController
	public class SendMessageController {

		@Resource
		private MyMessageProvider myMessageProvider;

		@GetMapping(value = "/sendMessage")
		public String send()
		{
			return myMessageProvider.send();
		}
	}
	~~~

6、测试
- 启动7001eureka
- 启动rabbitmq
- 启动8801
- 访问：http://localhost:8801/sendMessage

### 13.4、消息驱动之消费者

1、新建 cloud-stream-rabbitmq-provider8802

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

    <artifactId>cloud-stream-rabbitmq-consumer8802</artifactId>


    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
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
server:
  port: 8802

spring:
  application:
    name: cloud-stream-consumer
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置rabbitmq的相关的环境配置
            spring:
              rabbitmq:
                host: localhost
                port: 5672
                username: guest
                password: guest
      bindings: # 服务的整合处理
        input: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为对象json，如果是文本则设置“text/plain”
          binder: defaultRabbit # 设置要绑定的消息服务的具体设置

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://localhost:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: receive-8802.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StreamMQMain8802 {
    public static void main(String[] args)
    {
        SpringApplication.run(StreamMQMain8802.class,args);
    }
}
~~~

5、编写业务类

~~~Java
package com.clover.springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.annotation.StreamListener;
import org.springframework.cloud.stream.messaging.Sink;
import org.springframework.messaging.Message;
import org.springframework.stereotype.Component;

@Component
@EnableBinding(Sink.class)
public class ReceiveMessageListener {
    @Value("${server.port}")
    private String serverPort;

	// 监听队列，用于消费者的队列的消息接收
    @StreamListener(Sink.INPUT)
    public void input(Message<String> message)
    {
        System.out.println("消费者1号，------------>接收到的消息：" + message.getPayload() + "\t port:" + serverPort);
    }
}
~~~

### 13.5、分组消费与持久化

#### 13.5.1、消费
1、新建 cloud-stream-rabbitmq-provider8803

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

    <artifactId>cloud-stream-rabbitmq-consumer8803</artifactId>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
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
server:
  port: 8803

spring:
  application:
    name: cloud-stream-consumer
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置rabbitmq的相关的环境配置
            spring:
              rabbitmq:
                host: localhost
                port: 5672
                username: guest
                password: guest
      bindings: # 服务的整合处理
        input: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为对象json，如果是文本则设置“text/plain”
          binder: defaultRabbit # 设置要绑定的消息服务的具体设置

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://localhost:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: receive-8802.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StreamMQMain8803 {
    public static void main(String[] args)
    {
        SpringApplication.run(StreamMQMain8803.class,args);
    }
}
~~~

5、编写业务类

~~~Java
package com.clover.springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.annotation.StreamListener;
import org.springframework.cloud.stream.messaging.Sink;
import org.springframework.messaging.Message;
import org.springframework.stereotype.Component;

@Component
@EnableBinding(Sink.class)
public class ReceiveMessageListener {
    @Value("${server.port}")
    private String serverPort;

    // 监听队列，用于消费者的队列的消息接收
    @StreamListener(Sink.INPUT)
    public void input(Message<String> message)
    {
        System.out.println("消费者2号，------------>接收到的消息：" + message.getPayload() + "\t port:" + serverPort);
    }
}
~~~

6、运行后有两个问题
1. 有重复消费问题
2. 消息持久化问题

7、消费
- 目前是8802/8803同时都收到了，存在重复消费问题
- 如何解决？
	- **分组和持久化属性group**
- 生产实际案例
	- 比如在如下场景中，订单系统我们做集群部署，都会从RabbitMQ中获取订单信息，那**如果一个订单同时被两个服务获取到**，那么就会造成数据错误，我们得避免这种情况。这时**我们就可以使用Stream中的消息分组来解决**
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022201520.png)
	
	- 注意在Stream中处于`同一个group中`的多个消费者是`竞争关系`，就能够保证消息只会被其中一个应用消费一次
	- **不同组是可以全面消费的(重复消费)**
	- **同一组内会发生竞争关系，只有其中一个可以消费**

#### 13.5.2、分组

1. 原理
	- 微服务应用放置于同一个group中，就能够保证消息只会被其中一个应用消费一次
	- **不同的组是可以消费的，同一个组内会发生竞争关系，只有其中一个可以消费**
2. 8802/8803都变成**不同组**，group两个不同
	-   **group: cloverA、cloverB**
	-   8802修改YML
		~~~xml
		server:
		  port: 8802

		spring:
		  application:
			name: cloud-stream-consumer
		  cloud:
			  stream:
				binders: # 在此处配置要绑定的rabbitmq的服务信息；
				  defaultRabbit: # 表示定义的名称，用于于binding整合
					type: rabbit # 消息组件类型
					environment: # 设置rabbitmq的相关的环境配置
					  spring:
						rabbitmq:
						  host: localhost
						  port: 5672
						  username: guest
						  password: guest
				bindings: # 服务的整合处理
				  input: # 这个名字是一个通道的名称，在分析具体源代码的时候会进行说明
					destination: studyExchange # 表示要使用的Exchange名称定义
					content-type: application/json # 设置消息类型，本次为对象json，如果是文本则设置“text/plain”
			   binder: defaultRabbit # 设置要绑定的消息服务的具体设置
					 group: cloverA
		eureka:
		  client: # 客户端进行Eureka注册的配置
			service-url:
			  defaultZone: http://localhost:7001/eureka
		  instance:
			lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
			lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
			instance-id: receive-8802.com  # 在信息列表时显示主机名称
			prefer-ip-address: true     # 访问的路径变为IP地址
		~~~
	-   8803修改YML
		~~~xml
		server:
		  port: 8802

		spring:
		  application:
			name: cloud-stream-consumer
		  cloud:
			  stream:
				binders: # 在此处配置要绑定的rabbitmq的服务信息；
				  defaultRabbit: # 表示定义的名称，用于于binding整合
					type: rabbit # 消息组件类型
					environment: # 设置rabbitmq的相关的环境配置
					  spring:
						rabbitmq:
						  host: localhost
						  port: 5672
						  username: guest
						  password: guest
				bindings: # 服务的整合处理
				  input: # 这个名字是一个通道的名称，在分析具体源代码的时候会进行说明
					destination: studyExchange # 表示要使用的Exchange名称定义
					content-type: application/json # 设置消息类型，本次为对象json，如果是文本则设置“text/plain”
			   binder: defaultRabbit # 设置要绑定的消息服务的具体设置
					 group: cloverB
		eureka:
		  client: # 客户端进行Eureka注册的配置
			service-url:
			  defaultZone: http://localhost:7001/eureka
		  instance:
			lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
			lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
			instance-id: receive-8802.com  # 在信息列表时显示主机名称
			prefer-ip-address: true     # 访问的路径变为IP地址
		~~~
	- 我们自己配置
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022203110.png)
		
		- 分布式微服务应用为了实现高可用和负载均衡，实际上都会部署多个实例，本例我启动了两个消费微服务(8802/8803)
		- 多数情况，生产者发送消息给某个具体微服务时只希望被消费一次，按照上面我们启动两个应用的例子，虽然它们同属一个应用，但是这个消息出现了被重复消费两次的情况。为了解决这个问题，在Spring Cloud Stream中提供了**消费组**的概念
3. 8802/8803实现了轮询分组，每次只有一个消费者，8801模块的发的消息只能被8802或8803其中一个接收到，这样避免了重复消费
4. 8802/8803都变成相同组，group两个相同
	-   **group: cloverA**
	-   8802修改YML
		~~~xml
		server:
		  port: 8802

		spring:
		  application:
			name: cloud-stream-consumer
		  cloud:
			  stream:
				binders: # 在此处配置要绑定的rabbitmq的服务信息；
				  defaultRabbit: # 表示定义的名称，用于于binding整合
					type: rabbit # 消息组件类型
					environment: # 设置rabbitmq的相关的环境配置
					  spring:
						rabbitmq:
						  host: localhost
						  port: 5672
						  username: guest
						  password: guest
				bindings: # 服务的整合处理
				  input: # 这个名字是一个通道的名称，在分析具体源代码的时候会进行说明
					destination: studyExchange # 表示要使用的Exchange名称定义
					content-type: application/json # 设置消息类型，本次为对象json，如果是文本则设置“text/plain”
			   binder: defaultRabbit # 设置要绑定的消息服务的具体设置
					 group: cloverA
		eureka:
		  client: # 客户端进行Eureka注册的配置
			service-url:
			  defaultZone: http://localhost:7001/eureka
		  instance:
			lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
			lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
			instance-id: receive-8802.com  # 在信息列表时显示主机名称
			prefer-ip-address: true     # 访问的路径变为IP地址
		~~~
	-   8803修改YML
		~~~xml
		server:
		  port: 8802

		spring:
		  application:
			name: cloud-stream-consumer
		  cloud:
			  stream:
				binders: # 在此处配置要绑定的rabbitmq的服务信息；
				  defaultRabbit: # 表示定义的名称，用于于binding整合
					type: rabbit # 消息组件类型
					environment: # 设置rabbitmq的相关的环境配置
					  spring:
						rabbitmq:
						  host: localhost
						  port: 5672
						  username: guest
						  password: guest
				bindings: # 服务的整合处理
				  input: # 这个名字是一个通道的名称，在分析具体源代码的时候会进行说明
					destination: studyExchange # 表示要使用的Exchange名称定义
					content-type: application/json # 设置消息类型，本次为对象json，如果是文本则设置“text/plain”
			   binder: defaultRabbit # 设置要绑定的消息服务的具体设置
					 group: cloverA
		eureka:
		  client: # 客户端进行Eureka注册的配置
			service-url:
			  defaultZone: http://localhost:7001/eureka
		  instance:
			lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
			lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
			instance-id: receive-8802.com  # 在信息列表时显示主机名称
			prefer-ip-address: true     # 访问的路径变为IP地址
		~~~
	- 结论：`同一个组的多个微服务实例，每次只会有一个拿到`


#### 13.5.3、持久化

1. 通过上述，解决了重复消费问题，再看看持久化
2. 停止8802/8803并去除掉8802的分组`group: cloverA`，但是8803的分组`group: cloverA`没有去掉
3. 8801先发送4条消息到rabbitmq
4. 先启动8802，无分组属性配置，后台没有打出来消息
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022204136.png) 
	
5. 再启动8803，有分组属性配置，后台打出来了MQ上的消息
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022204300.png)

6. **在本实例中，如果两个消费端都没有去掉`group: cloverA`，但是将两个消费端暂停，同时提供者发送消息，那么这两个消费端谁先启动就会接收完所有的消息，另外一个消费端则不会接收消息**

## 14、SpringCloud Sleuth分布式请求链路跟踪

### 14.1、概述

#### 14.1.1、为什么会出现这个技术？需要解决哪些问题？

在微服务框架中，一个由客户端发起的请求在后端系统中会经过多个不同的的服务节点调用来协同产生最后的请求结果，每一个前端请求都会形成一条复杂的分布式服务调用链路，链路中的任何一环出现高延时或错误都会引起整个请求最后的失败

#### 14.1.2、是什么？

[官网说明](https://github.com/spring-cloud/spring-cloud-sleuth)

- SpringCloud Sleuth提供了一套完整的服务跟踪的解决方案
- 在分布式系统中提供追踪解决方案并且兼容支持了zipkin

### 14.2、搭建链路监控步骤

1. zipkin
	- 下载：[下载地址](https://dl.bintray.com/openzipkin/maven/io/zipkin/java/zipkin-server/)
		- SpringCloud**从F版**起已不需要自己构建Zipkin Server了，只需调用jar包即可
	- 运行jar：java -jar zipkin-server-2.12.9-exec.jar
	- 运行控制台
		- http://localhost:9411/zipkin/
		- 术语
			- 完整的调用链路 
			![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022211747.png)
			- 上图精简
			![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022211823.png)
			- 名词解释
				- Trace:类似于树结构的Span集合，表示一条调用链路，存在唯一标识
				- span:表示调用链路来源，通俗的理解span就是一次请求信息

2. 服务提供者：cloud-provider-payment8001
	
	~~~xml
	<!--包含了sleuth+zipkin-->
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-zipkin</artifactId>
	</dependency>
	~~~
	
	~~~yml
	spring:
	  application:
		name: cloud-payment-service
	  zipkin:
		base-url: http://localhost:9411
	  sleuth:
		sampler:
		  #采样率值介于 0 到 1 之间，1 则表示全部采集
		 probability: 1
	~~~ 
3. 服务消费者(调用方)：cloud-consumer-order80
	~~~xml
	<!--包含了sleuth+zipkin-->
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-zipkin</artifactId>
	</dependency>
	~~~
	
	~~~yml
	spring:
    application:
        name: cloud-order-service
    zipkin:
      base-url: http://localhost:9411
    sleuth:
      sampler:
        probability: 1
	~~~ 
4. 依次启动eureka7001/8001/80
	- 80调用8001几次测试下
5. 打开浏览器访问：http://localhost:9411
	- 会出现以下界面
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022213115.png)
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022213200.png)
	- 查看依赖关系
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022213133.png)



## 15、SpringCloud Alibaba入门简介

### 15.1、SpringCloud alibaba带来了什么

#### 15.1.2、是什么？

[官网](https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md)

诞生：
2018.10.31，SpringCloud Alibaba 正式入驻了 Spring Cloud 官方孵化器，并在 Maven 中央库发布了第一个版本。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022215147.png)

#### 15.1.3、能干嘛？
 
- 服务限流降级：默认支持 Servlet、Feign、RestTemplate、Dubbo 和 RocketMQ 限流降级功能的接入，可以在运行时通过控制台实时修改限流降级规则，还支持查看限流降级 Metrics 监控
- 服务注册与发现：适配 SpringCloud 服务注册与发现标准，默认集成了 Ribbon 的支持
- 分布式配置管理：支持分布式系统中的外部化配置，配置更改时自动刷新
- 消息驱动能力：基于 SpringCloud Stream 为微服务应用构建消息驱动能力
- 阿里云对象存储：阿里云提供的海量、安全、低成本、高可靠的云存储服务。支持在任何应用、任何时间、任何地点存储和访问任意类型的数据
- 分布式任务调度：提供秒级、精准、高可靠、高可用的定时（基于 Cron 表达式）任务调度服务。同时提供分布式的任务执行模型，如网格任务。网格任务支持海量子任务均匀分配到所有 Worker（schedulerx-client）上执行

#### 15.1.4、怎么玩？

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211022215359.png)

### 15.2、SpringCloud alibaba学习资料获取

[SpringCloud Alibaba官网](https://spring.io/projects/spring-cloud-alibaba#overview)

Spring Cloud Alibaba 致力于提供微服务开发的一站式解决方案。此项目包含开发分布式应用微服务的必需组件，方便开发者通过 SpringCloud 编程模型轻松使用这些组件来开发分布式应用服务。
 
依托 Spring Cloud Alibaba，您只需要添加一些注解和少量配置，就可以将 Spring Cloud 应用接入阿里微服务解决方案，通过阿里中间件来迅速搭建分布式应用系统

[SpringCloud Alibaba参考文档](https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html)

## 16、SpringCloud Alibaba Nacos服务注册和配置中心

### 16.1、Nacos简介

#### 16.1.1、为什么叫Nacos

前四个字母分别为Naming和Configuration的前两个字母，最后的s为Service。

#### 16.1.2、Nacos是什么？

- 一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台
- Nacos: Dynamic Naming and Configuration Service
- Nacos就是注册中心 + 配置中心的组合 <===> **Nacos = Eureka+Config +Bus**

#### 16.1.3、Nacos能干嘛？

- 替代Eureka做服务注册中心
- 替代Config做服务配置中心

#### 16.1.4、去哪下？

[Nacos Github地址](https://github.com/alibaba/Nacos)

[官网文档](https://nacos.io/zh-cn/index.html)

#### 16.1.5、各种注册中心比较

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023161548.png)

### 16.2、安装并运行Nacos
1. **本地Java8+Maven环境已经OK**
2. 先从官网下载Nacos
3. 解压安装包，直接运行bin目录下的startup.cmd
4. 命令运行成功后直接访问：http://localhost:8848/nacos，默认账号密码都是nacos
5. 结果页面

**大坑！！！！**

**注意：运行Nacos必备Java环境，如果运行报错：Please set the JAVA_HOME variable in your environment, We need java(x64)! jdk8，那么你需要在你本机的环境变量中去新建一个JAVA_HOME变量环境**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023165232.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023165255.png)

### 16.3、Nacos作为服务注册中心演示

#### 16.3.1、官网文档

[官网文档](https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html#_spring_cloud_alibaba_nacos_config)

#### 16.3.2、基于Nacos的服务提供者

1、新建 cloud-alibaba-provider-payment9001

2、修改POM

父POM
~~~xml
<!--spring cloud alibaba 2.1.0.RELEASE-->
<dependency>
	<groupId>com.alibaba.cloud</groupId>
	<artifactId>spring-cloud-alibaba-dependencies</artifactId>
	<version>2.1.0.RELEASE</version>
	<type>pom</type>
	<scope>import</scope>
</dependency>
~~~

本模块POM
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

    <artifactId>cloud-alibaba-provider-payment9001</artifactId>


    <dependencies>
        <!--SpringCloud ailibaba nacos -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
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
server:
  port: 9001

spring:
  application:
    name: nacos-payment-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #配置Nacos地址

management:
  endpoints:
    web:
      exposure:
        include: '*'
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud.alibaba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain9001 {
    public static void main(String[] args)
    {
        SpringApplication.run(PaymentMain9001.class,args);
    }
}
~~~

5、编写业务类

~~~Java
package com.clover.springcloud.alibaba.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class PaymentController {

    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/nacos/{id}")
    public String getPayment(@PathVariable("id") Integer id)
    {
        return "nacos registry, serverPort: "+ serverPort+"\t id"+id;
    }
}
~~~

6、测试
- http://localhost:9001/payment/nacos/1
- nacos控制台

7、为下面演示nacos的负载均衡，参照9001建立9002

#### 16.3.3、基于Nacos的服务消费者

1、新建 cloud-alibaba-consumer-nacos-order83

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

    <artifactId>cloud-alibaba-consumer-nacos-order83</artifactId>


    <dependencies>
        <!--SpringCloud ailibaba nacos -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
        <dependency>
            <groupId>com.clover.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
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

`为什么nacos支持负载均衡？`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023203831.png)

3、编写YML

~~~yml
server:
  port: 83


spring:
  application:
    name: nacos-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848


#消费者将要去访问的微服务名称(注册成功进nacos的微服务提供者)
service-url:
  nacos-user-service: http://nacos-payment-provider 
~~~

4、编写主启动类

~~~Java
package com.clover.springcloud.alibaba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class OrderNacosMain83 {
    public static void main(String[] args)
    {
        SpringApplication.run(OrderNacosMain83.class,args);
    }
}
~~~

5、编写业务类

~~~Java
package com.clover.springcloud.alibaba.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderNacosController {

    @Value("${service-url.nacos-user-service}")
    private String serverURL;

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/nacos/{id}")
    public String paymentInfo(@PathVariable("id") Long id)
    {
        return restTemplate.getForObject(serverURL + "/payment/nacos/" + id,String.class);
    }
}
~~~

~~~Java
package com.clover.springcloud.alibaba.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextBean {

    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate()
    {
        return new RestTemplate();
    }
}
~~~

6、测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023204554.png)

访问：http://localhost:83/consumer/payment/nacos/1

- 83访问9001/9002，轮询负载OK

#### 16.3.4、服务注册中心对比

Nacos全景图所示

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023205701.png)

Nacos和CAP

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023205740.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023205750.png)


**Nacos 支持AP和CP模式的切换**

- **C是所有节点在同一时间看到的数据是一致的；而A的定义是所有的请求都会收到响应**
- `何时选择使用何种模式？`
	- 一般来说，如果不需要存储服务级别的信息且服务实例是通过nacos-client注册，并能够保持心跳上报，那么就可以选择AP模式。当前主流的服务如 Spring cloud 和 Dubbo 服务，都适用于AP模式，AP模式为了服务的可能性而减弱了一致性，因此AP模式下只支持注册临时实例。
	- 如果需要在服务级别编辑或者存储配置信息，那么 CP 是必须，K8S服务和DNS服务则适用于CP模式
	- CP模式下则支持注册持久化实例，此时则是以 Raft 协议为集群运行模式，该模式下注册实例之前必须先注册服务，如果服务不存在，则会返回错误。
	- 切换命令：`curl -X PUT '$NACOS_SERVER:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP'`


### 16.4、Nacos作为服务配置中心演示

#### 16.4.1、Nacos作为配置中心-基础配置
1、新建 cloud-alibaba-config-nacos-client3377

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

    <artifactId>cloud-alibaba-config-nacos-client3377</artifactId>

    <dependencies>
        <!--nacos-config-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>
        <!--nacos-discovery-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--web + actuator-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--一般基础配置-->
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

- **为什么要配置两个yml？**
	- Nacos同springcloud-config一样，在项目初始化时，要保证`先从配置中心进行配置拉取`，拉取配置之后，才能保证项目的正常启动
	- springboot中配置文件的加载是存在优先级顺序的，**bootstrap优先级高于application**

~~~yml
# nacos配置
server:
  port: 3377

spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #Nacos服务注册中心地址
      config:
        server-addr: localhost:8848 #Nacos作为配置中心地址
        file-extension: yaml #指定yaml格式的配置


  # ${spring.application.name}-${spring.profile.active}.${spring.cloud.nacos.config.file-extension}
~~~

~~~yml
spring:
  profiles:
    active: dev # 表示开发环境
~~~
4、编写主启动类

~~~Java
package com.clover.spingcloud.alibaba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class NacosConfigClientMain3377 {
    public static void main(String[] args)
    {
        SpringApplication.run(NacosConfigClientMain3377.class,args);
    }
}
~~~

5、编写业务类

~~~Java
package com.clover.spingcloud.alibaba.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RefreshScope //在控制器类加入@RefreshScope注解使当前类下的配置支持Nacos的动态刷新功能
public class ConfigClientController {
    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/config/info")
    public String getConfigInfo() {
        return configInfo;
    }
}
~~~

**6、在Nacos中添加配置信息**
- Nacos中的匹配规则
	- 理论
		- Nacos中的dataId的组成格式及与SpringBoot配置文件中的匹配规则 [官网说明](https://nacos.io/zh-cn/docs/quick-start-spring-cloud.html)
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023213409.png)
		- 最后公式：`${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}`
	- 实操
		- 新增配置
		- Nacos界面配置对应
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023213604.png)
		- 设置DataId
			- 公式：`${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}`
			- `prefix`:**默认为 spring.application.name 的值**
			- `spring.profile.active`： **即为当前环境对应的 profile**，可以通过配置项 spring.profile.active 来配置。
			- `file-exetension` ：**为配置内容的数据格式**，可以通过配置项spring.cloud.nacos.config.file-extension 来配置
			- 小总结说明
			![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023213759.png)


7、测试
1. 启动前需要在nacos客户端-配置管理-配置管理栏目下有对应的yaml配置文件
2. 运行cloud-config-nacos-client3377的主启动类
3. 调用接口查看配置信息：http://localhost:3377/config/info

8、自带动态刷新
- 修改下Nacos中的yaml配置文件，再次调用查看配置的接口，就会发现配置已经刷新

#### 16.4.2、Nacos作为配置中心-分类配置

##### 16.4.2.1、问题

多环境多项目管理
- 问题1：
	
	实际开发中，通常一个系统会准备，dev开发环境、test测试环境、prod生产环境。
	
	如何保证指定环境启动时服务能正确读取到Nacos上相应环境的配置文件呢？
	
- 问题2：

	一个大型分布式微服务系统会有很多微服务子项目，每个微服务项目又都会有相应的开发环境、测试环境、预发环境、正式环境......
	
	那怎么对这些微服务配置进行管理呢？
	
	
##### 16.4.2.2、Nacos的图形化管理界面

1. 配置管理
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023215042.png)
2. 命名空间
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023215100.png)
	
	
##### 16.4.2.3、Namespace+Group+Data ID三者关系？为什么这么设计？
1. 是什么
	- 类似Java里面的package名和类名
	- **最外层的namespace是可以用于区分部署环境的**，`Group和DataID逻辑上区分两个目标对象`

2. 三者之间的情况
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211023215222.png)
	
3. 默认情况
	- **Namespace=public，Group=DEFAULT_GROUP, 默认Cluster是DEFAULT**
	- `Nacos默认的命名空间是public，Namespace主要用来实现隔离`，比方说我们现在有三个环境：开发、测试、生产环境，我们就可以创建三个Namespace，不同的Namespace之间是隔离的
	- `Group默认是DEFAULT_GROUP`，Group可以把不同的微服务划分到同一个分组里面去
	- `Service就是微服务`；一个Service可以包含多个Cluster（集群），Nacos默认Cluster是DEFAULT，**Cluster是对指定微服务的一个虚拟划分**
	- 比方说为了容灾，将Service微服务分别部署在了杭州机房和广州机房，这时就可以给杭州机房的Service微服务起一个集群名称（HZ），给广州机房的Service微服务起一个集群名称（GZ），还可以尽量让同一个机房的微服务互相调用，以提升性能
	- 最后是Instance，就是微服务的实例


##### 16.4.2.4、三种方案加载配置

###### 16.4.2.4.1、DataID方案

- 指定spring.profile.active和配置文件的DataID来使不同环境下读取不同的配置
- **默认空间+默认分组+新建dev和test两个DataID**
	- 新建dev配置DataID
	- 新建test配置DataID
- 通过spring.profile.active属性就能进行多环境下配置文件的读取
- 测试
	- http://localhost:3377/config/info
	- 配置是什么就加载什么


###### 16.4.2.4.2、Group方案
- 通过Group实现环境区分
- DataId相同，但是属于不同的组
- 修改bootstrap+application
	- `在config下增加一条group的配置即可。可配置为DEV_GROUP或TEST_GROUP`

###### 16.4.2.4.3、Namespace方案
- 新建dev/test的Namespace
- 在对应的命名空间下建立不同的配置
- 修改bootstrap.yml文件
	- 在group同级的地方加上namespace

### 16.5、Nacos集群和持久化配置（重要）

#### 16.5.1、官网说明

[Nacos集群化部署说明](https://nacos.io/zh-cn/docs/cluster-mode-quick-start.html)

官网架构图
- 老版架构图

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024111315.png)

- 现在版(2021.10)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024111352.png)


对上图官网翻译，真实情况

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024111437.png)

说明：
- 默认Nacos使用嵌入式数据库实现数据的存储。所以，如果启动多个默认配置下的Nacos节点，数据存储是存在一致性问题的
- 为了解决这个问题，Nacos采用了**集中式存储的方式来支持集群化部署，目前只支持MySQL的存储**

- **重点说明：**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024111550.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024111610.png)


 #### 16.5.2、Nacos持久化配置解释
 1. Nacos默认自带的是嵌入式数据库derby
	 - [去Github源码的pom文件中查看](https://github.com/alibaba/nacos/blob/develop/config/pom.xml)
 2. derby到mysql切换配置步骤
	 - nacos-server-1.1.4\nacos\conf目录下找到sql脚本
		 - nacos-mysql.sql
			~~~sql

			CREATE DATABASE nacos_config;
			USE nacos_config;

			/******************************************/
			/*   数据库全名 = nacos_config   */
			/*   表名称 = config_info   */
			/******************************************/
			CREATE TABLE `config_info` (
			  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
			  `data_id` VARCHAR(255) NOT NULL COMMENT 'data_id',
			  `group_id` VARCHAR(255) DEFAULT NULL,
			  `content` LONGTEXT NOT NULL COMMENT 'content',
			  `md5` VARCHAR(32) DEFAULT NULL COMMENT 'md5',
			  `gmt_create` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
			  `gmt_modified` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
			  `src_user` TEXT COMMENT 'source user',
			  `src_ip` VARCHAR(20) DEFAULT NULL COMMENT 'source ip',
			  `app_name` VARCHAR(128) DEFAULT NULL,
			  `tenant_id` VARCHAR(128) DEFAULT '' COMMENT '租户字段',
			  `c_desc` VARCHAR(256) DEFAULT NULL,
			  `c_use` VARCHAR(64) DEFAULT NULL,
			  `effect` VARCHAR(64) DEFAULT NULL,
			  `type` VARCHAR(64) DEFAULT NULL,
			  `c_schema` TEXT,
			  PRIMARY KEY (`id`),
			  UNIQUE KEY `uk_configinfo_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
			) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info';

			/******************************************/
			/*   数据库全名 = nacos_config   */
			/*   表名称 = config_info_aggr   */
			/******************************************/
			CREATE TABLE `config_info_aggr` (
			  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
			  `data_id` VARCHAR(255) NOT NULL COMMENT 'data_id',
			  `group_id` VARCHAR(255) NOT NULL COMMENT 'group_id',
			  `datum_id` VARCHAR(255) NOT NULL COMMENT 'datum_id',
			  `content` LONGTEXT NOT NULL COMMENT '内容',
			  `gmt_modified` DATETIME NOT NULL COMMENT '修改时间',
			  `app_name` VARCHAR(128) DEFAULT NULL,
			  `tenant_id` VARCHAR(128) DEFAULT '' COMMENT '租户字段',
			  PRIMARY KEY (`id`),
			  UNIQUE KEY `uk_configinfoaggr_datagrouptenantdatum` (`data_id`,`group_id`,`tenant_id`,`datum_id`)
			) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='增加租户字段';


			/******************************************/
			/*   数据库全名 = nacos_config   */
			/*   表名称 = config_info_beta   */
			/******************************************/
			CREATE TABLE `config_info_beta` (
			  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
			  `data_id` VARCHAR(255) NOT NULL COMMENT 'data_id',
			  `group_id` VARCHAR(128) NOT NULL COMMENT 'group_id',
			  `app_name` VARCHAR(128) DEFAULT NULL COMMENT 'app_name',
			  `content` LONGTEXT NOT NULL COMMENT 'content',
			  `beta_ips` VARCHAR(1024) DEFAULT NULL COMMENT 'betaIps',
			  `md5` VARCHAR(32) DEFAULT NULL COMMENT 'md5',
			  `gmt_create` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
			  `gmt_modified` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
			  `src_user` TEXT COMMENT 'source user',
			  `src_ip` VARCHAR(20) DEFAULT NULL COMMENT 'source ip',
			  `tenant_id` VARCHAR(128) DEFAULT '' COMMENT '租户字段',
			  PRIMARY KEY (`id`),
			  UNIQUE KEY `uk_configinfobeta_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
			) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_beta';

			/******************************************/
			/*   数据库全名 = nacos_config   */
			/*   表名称 = config_info_tag   */
			/******************************************/
			CREATE TABLE `config_info_tag` (
			  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
			  `data_id` VARCHAR(255) NOT NULL COMMENT 'data_id',
			  `group_id` VARCHAR(128) NOT NULL COMMENT 'group_id',
			  `tenant_id` VARCHAR(128) DEFAULT '' COMMENT 'tenant_id',
			  `tag_id` VARCHAR(128) NOT NULL COMMENT 'tag_id',
			  `app_name` VARCHAR(128) DEFAULT NULL COMMENT 'app_name',
			  `content` LONGTEXT NOT NULL COMMENT 'content',
			  `md5` VARCHAR(32) DEFAULT NULL COMMENT 'md5',
			  `gmt_create` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
			  `gmt_modified` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
			  `src_user` TEXT COMMENT 'source user',
			  `src_ip` VARCHAR(20) DEFAULT NULL COMMENT 'source ip',
			  PRIMARY KEY (`id`),
			  UNIQUE KEY `uk_configinfotag_datagrouptenanttag` (`data_id`,`group_id`,`tenant_id`,`tag_id`)
			) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_tag';

			/******************************************/
			/*   数据库全名 = nacos_config   */
			/*   表名称 = config_tags_relation   */
			/******************************************/
			CREATE TABLE `config_tags_relation` (
			  `id` BIGINT(20) NOT NULL COMMENT 'id',
			  `tag_name` VARCHAR(128) NOT NULL COMMENT 'tag_name',
			  `tag_type` VARCHAR(64) DEFAULT NULL COMMENT 'tag_type',
			  `data_id` VARCHAR(255) NOT NULL COMMENT 'data_id',
			  `group_id` VARCHAR(128) NOT NULL COMMENT 'group_id',
			  `tenant_id` VARCHAR(128) DEFAULT '' COMMENT 'tenant_id',
			  `nid` BIGINT(20) NOT NULL AUTO_INCREMENT,
			  PRIMARY KEY (`nid`),
			  UNIQUE KEY `uk_configtagrelation_configidtag` (`id`,`tag_name`,`tag_type`),
			  KEY `idx_tenant_id` (`tenant_id`)
			) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_tag_relation';

			/******************************************/
			/*   数据库全名 = nacos_config   */
			/*   表名称 = group_capacity   */
			/******************************************/
			CREATE TABLE `group_capacity` (
			  `id` BIGINT(20) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键ID',
			  `group_id` VARCHAR(128) NOT NULL DEFAULT '' COMMENT 'Group ID，空字符表示整个集群',
			  `quota` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
			  `usage` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '使用量',
			  `max_size` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
			  `max_aggr_count` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数，，0表示使用默认值',
			  `max_aggr_size` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
			  `max_history_count` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
			  `gmt_create` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
			  `gmt_modified` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
			  PRIMARY KEY (`id`),
			  UNIQUE KEY `uk_group_id` (`group_id`)
			) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='集群、各Group容量信息表';

			/******************************************/
			/*   数据库全名 = nacos_config   */
			/*   表名称 = his_config_info   */
			/******************************************/
			CREATE TABLE `his_config_info` (
			  `id` BIGINT(64) UNSIGNED NOT NULL,
			  `nid` BIGINT(20) UNSIGNED NOT NULL AUTO_INCREMENT,
			  `data_id` VARCHAR(255) NOT NULL,
			  `group_id` VARCHAR(128) NOT NULL,
			  `app_name` VARCHAR(128) DEFAULT NULL COMMENT 'app_name',
			  `content` LONGTEXT NOT NULL,
			  `md5` VARCHAR(32) DEFAULT NULL,
			  `gmt_create` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00',
			  `gmt_modified` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00',
			  `src_user` TEXT,
			  `src_ip` VARCHAR(20) DEFAULT NULL,
			  `op_type` CHAR(10) DEFAULT NULL,
			  `tenant_id` VARCHAR(128) DEFAULT '' COMMENT '租户字段',
			  PRIMARY KEY (`nid`),
			  KEY `idx_gmt_create` (`gmt_create`),
			  KEY `idx_gmt_modified` (`gmt_modified`),
			  KEY `idx_did` (`data_id`)
			) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='多租户改造';


			/******************************************/
			/*   数据库全名 = nacos_config   */
			/*   表名称 = tenant_capacity   */
			/******************************************/
			CREATE TABLE `tenant_capacity` (
			  `id` BIGINT(20) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键ID',
			  `tenant_id` VARCHAR(128) NOT NULL DEFAULT '' COMMENT 'Tenant ID',
			  `quota` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
			  `usage` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '使用量',
			  `max_size` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
			  `max_aggr_count` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数',
			  `max_aggr_size` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
			  `max_history_count` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
			  `gmt_create` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
			  `gmt_modified` DATETIME NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
			  PRIMARY KEY (`id`),
			  UNIQUE KEY `uk_tenant_id` (`tenant_id`)
			) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='租户容量信息表';


			CREATE TABLE `tenant_info` (
			  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
			  `kp` VARCHAR(128) NOT NULL COMMENT 'kp',
			  `tenant_id` VARCHAR(128) DEFAULT '' COMMENT 'tenant_id',
			  `tenant_name` VARCHAR(128) DEFAULT '' COMMENT 'tenant_name',
			  `tenant_desc` VARCHAR(256) DEFAULT NULL COMMENT 'tenant_desc',
			  `create_source` VARCHAR(32) DEFAULT NULL COMMENT 'create_source',
			  `gmt_create` BIGINT(20) NOT NULL COMMENT '创建时间',
			  `gmt_modified` BIGINT(20) NOT NULL COMMENT '修改时间',
			  PRIMARY KEY (`id`),
			  UNIQUE KEY `uk_tenant_info_kptenantid` (`kp`,`tenant_id`),
			  KEY `idx_tenant_id` (`tenant_id`)
			) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='tenant_info';

			CREATE TABLE users (
				username VARCHAR(50) NOT NULL PRIMARY KEY,
				PASSWORD VARCHAR(500) NOT NULL,
				enabled BOOLEAN NOT NULL
			);

			CREATE TABLE roles (
				username VARCHAR(50) NOT NULL,
				role VARCHAR(50) NOT NULL
			);

			INSERT INTO users (username, PASSWORD, enabled) VALUES ('nacos', '$2a$10$EuWPZHzz32dJN7jexM34MOeYirDdFAZm2kuWj7VEOJhhZkDrxfvUu', TRUE);

			INSERT INTO roles (username, role) VALUES ('nacos', 'ROLE_ADMIN');
			~~~
		- nacos-server-1.1.4\nacos\conf目录下找到application.properties
			~~~sql
			spring.datasource.platform=mysql

			db.num=1
			db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
			db.user=root
			db.password=xn123456
			~~~
 3. 启动Nacos，可以看到是个全新的空记录界面，以前是记录进derby

#### 16.5.3、Linux版Nacos+MySQL生产环境配置

1、预计需要，1个Nginx+3个nacos注册中心+1个mysql

2、Nacos下载Linux版

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024114515.png)

[Nacos下载地址](https://github.com/alibaba/nacos/releases/tag/1.1.4)

3、**集群配置步骤(重点)**

1. Linux服务器上mysql数据库配置
	1. sql语句源文件：![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024161408.png)
	2. 自己Linux机器上的Mysql数据库粘贴，执行后结果
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024161453.png)
2. application.properties 配置
	1. 位置，**在对它进行操作前，记得备份一份原始数据，避免自己玩坏了没备份！！！！**
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024161542.png)
		
	2. 内容
		~~~sql
		spring.datasource.platform=mysql

		db.num=1
		db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
		db.user=root
		db.password=xn123456
		~~~
3. Linux服务器上nacos的集群配置cluster.conf
	1. **复制出cluster.conf作为备份，避免自己把数据玩坏了**
	2. 内容：**这个IP不能写127.0.0.1，必须是Linux命令hostname -i能够识别的IP**
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024161859.png)
	
4. 编辑Nacos的启动脚本startup.sh，使它能够接受不同的启动端口
	1. /opt/nacos/bin 目录下有startup.sh
	2. 在什么地方，修改什么，怎么修改
	3. 思考
		1. 平时单机版的启动，都是./startup.sh即可
		2. 但是，集群启动，我们希望可以类似其它软件的shell命令，**传递不同的端口号启动不同的nacos实例**
		3. **命令**：./startup.sh `-p 3333` 表示启动端口号为3333的nacos服务器实例，和上一步的cluster.conf配置的一致。
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024162100.png)
		
		`注意：此处要注意空格，格式对其，不要写错了！！！！`
		
	4. 执行方式
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024164843.png)
	
5. Nginx的配置，由它作为负载均衡器
	1. 修改nginx的配置文件：`/usr/local/nginx/conf`
	2. 修改nginx.conf
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024172904.png)
		
		~~~xml
		upstream cluster{
				server 127.0.0.1:3333;
				server 127.0.0.1:4444;
				server 127.0.0.1:5555;
			}

		server {
				listen       1111;
				server_name  localhost;
				#charset koi8-r;
				#access_log  logs/host.access.log  main;
				location / {
					#root   html;
					#index  index.html index.htm;
					proxy_pass http://cluster;
				}
		~~~
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024173020.png)
	
	**注意：坑！！！！！**
	
	**如果你安装nginx时执行启动命令时报错，那么你需要在nginx下建立一个logs文件，然后修改其权限，再次启动就好了，但是如果时出现页面无法访问，说明你防火墙没有开启该端口**
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024173158.png)
	
	[参考博客](https://blog.csdn.net/runingman0000/article/details/107584744)
	
6. 截止到此处，1个Nginx+3个nacos注册中心+1个mysql
	- 测试通过nginx访问nacos：http://192.168.167.48:1111/nacos/#/login
	- 新建一个配置测试
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024173707.png)
	
	- linux服务器的mysql插入一条记录
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024173738.png)
	
	
	
4、测试
- 微服务cloudalibaba-provider-payment9002启动注册进nacos集群
- 修改YML
	~~~yml
		server:
	  port: 9002

	spring:
	  application:
		name: nacos-payment-provider
	  cloud:
		nacos:
		  discovery:
			#server-addr: localhost:8848 #配置Nacos地址
			server-addr: 192.168.167.48:1111 # 换成nginx的1111端口，做集群

	management:
	  endpoints:
		web:
		  exposure:
			include: '*'
	~~~
- 结果

5、高可用小总结

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211024175339.png)

## 17、SpringCloud Alibaba Sentinel实现熔断与限流

### 17.1、Sentinel

[官网](https://github.com/alibaba/Sentinel)

1. Sentinel是什么？
	- 一句话解释，之前我们讲解过的Hystrix

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025194204.png)

2. 去哪下？[下载地址](https://github.com/alibaba/Sentinel/releases)
3. 能干嘛？
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025194449.png)
	
4. 怎么玩？[官方指导文档](https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html#_spring_cloud_alibaba_sentinel)
	- 服务使用中的各种问题
		- 服务雪崩
		- 服务降级
		- 服务熔断
		- 服务限流


### 17.2、安装Sentinel控制台
sentinel组件由2部分构成：
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025200335.png)

安装步骤:
- 运行命令：`java -jar sentinel-dashboard-1.7.0.jar`
- 前提
	- Java8环境可以用
	- 8080端口不能被占用
- 访问sentinel管理界面
	- http://localhost:8080
	- 登录账号密码均为sentinel

### 17.3、初始化演示工程

1、启动Nacos8848成功

2、新建 cloud-alibaba-sentinel-service8401

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

    <artifactId>cloud-alibaba-sentinel-service8401</artifactId>


    <dependencies>
        <!--SpringCloud ailibaba nacos -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--SpringCloud ailibaba sentinel-datasource-nacos 后续做持久化用到-->
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-datasource-nacos</artifactId>
        </dependency>
        <!--SpringCloud ailibaba sentinel -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <!--openfeign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <!-- SpringBoot整合Web组件+actuator -->
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
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>4.6.3</version>
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

4、编写YML

~~~yml
server:
  port: 8401

spring:
  application:
    name: cloud-alibaba-sentinel-service
  cloud:
    nacos:
      discovery:
        #Nacos服务注册中心地址
        server-addr: localhost:8848
    sentinel:
      transport:
        #配置Sentinel dashboard地址
        dashboard: localhost:8080
        #默认8719端口，假如被占用会自动从8719开始依次+1扫描,直至找到未被占用的端口
        port: 8719

management:
  endpoints:
    web:
      exposure:
        include: '*'
~~~

5、编写主启动类

~~~Java
package com.clover.springcloud.alibaba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class MainApp8401 {
    public static void main(String[] args)
    {
        SpringApplication.run(MainApp8401.class,args);
    }
}
~~~

6、编写业务类

~~~Java
package com.clover.springcloud.alibaba.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class FlowLimitController {

    @GetMapping("/testA")
    public String testA()
    {
        return "------testA";
    }

    @GetMapping("/testB")
    public String testB()
    {
        return "------testB";
    }
}
~~~

7、启动Sentinel8080：`java -jar sentinel-dashboard-1.7.0.jar`

8、启动微服务8401

9、启动8401微服务后查看sentinel控制台
- 空空如也

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025203327.png)

- Sentinel采用懒加载说明
	- 执行一次访问即可
		- http://localhost:8401/testA
		- http://localhost:8401/testB
	- 效果
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025203630.png)
	
- 结论：**sentinel8080正在监控微服务8401**

### 17.4、流控规则

#### 17.4.1、基本介绍

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025204441.png)

进一步解释：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025204457.png)


#### 17.4.2、流控模式

##### 17.4.2.1、直接(默认)

- 直接->快速失败(`系统默认`)
- 配置及说明
	- 表示1秒钟内查询1次就是OK，若超过次数1，就直接-快速失败，报默认错误
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025205705.png)
	
- 测试
	- 快速点击访问http://localhost:8401/testA
	- 结果：**Blocked by Sentinel (flow limiting)**
	- 思考
		- 直接调用默认报错信息，技术方面OK，但是是否应该有我们自己的后续处理?
		- 类似有个fallback的兜底方法？


##### 17.4.2.2、关联

1. 关联是什么？
- 当关联的资源达到阈值时，就限流自己
- 当与A关联的资源B达到阀值后，就限流A自己
- B惹事，A挂了

2. 配置A

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025212346.png)

- 设置效果：当关联资源/testB的QPS阀值超过1时，就限流/testA的Rest访问地址，**当关联资源到阈值后限制配置好的资源名**

3. postman模拟并发密集访问testB
4. 运行后发现testA挂了
	- 点击访问：http://localhost:8401/testA
	- 结果：**Blocked by Sentinel (flow limiting)**


##### 17.4.2.3、链路


#### 17.4.3、流控效果

##### 17.4.3.1、直接->快速失败(默认的流控处理)

- 直接失败，抛出异常
	- `Blocked by Sentinel (flow limiting)`
- 源码
	- com.alibaba.csp.sentinel.slots.block.flow.controller.DefaultController

##### 17.4.3.2、预热

1. 说明：**公式：阈值除以coldFactor(默认值为3),经过预热时长后才会达到阈值**
2. 官网
	- 默认coldFactor为3，即请求 QPS 从 threshold / 3 开始，经预热时长逐渐升至设定的 QPS 阈值。
	- [限流 冷启动官网说明](https://github.com/alibaba/Sentinel/wiki/%E9%99%90%E6%B5%81---%E5%86%B7%E5%90%AF%E5%8A%A8)
3. 源码
	- com.alibaba.csp.sentinel.slots.block.flow.controller.WarmUpController
4. WarmUp配置
	- 默认 coldFactor 为 3，即请求QPS从(threshold / 3) 开始，经多少预热时长才逐渐升至设定的 QPS 阈值
	- 案例，阀值为10+预热时长设置5秒
	- 系统初始化的阀值为10 / 3 约等于3,即阀值刚开始为3；然后过了5秒后阀值才慢慢升高恢复到10
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025213919.png)
	
5. 多次点击：http://localhost:8401/testB；刚开始不行，后续慢慢OK
6. 应用场景
	- 如：秒杀系统在开启的瞬间，会有很多流量上来，很有可能把系统打死，预热方式就是把为了保护系统，可慢慢的把流量放进来，慢慢的把阀值增长到设置的阀值


##### 17.4.3.3、排队等待

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025221047.png)

- 匀速排队，让请求以均匀的速度通过，**阀值类型必须设成QPS，否则无效**
- 设置含义：/testA每秒1次请求，超过的话就排队等待，等待的超时时间为20000毫秒

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025221148.png)

1. 官网：[流量控制指导文档](https://github.com/alibaba/Sentinel/wiki/%E6%B5%81%E9%87%8F%E6%8E%A7%E5%88%B6)
2. 源码：com.alibaba.csp.sentinel.slots.block.flow.controller.RateLimiterController 


### 17.5、降级规则

[官网](https://github.com/alibaba/Sentinel/wiki/%E7%86%94%E6%96%AD%E9%99%8D%E7%BA%A7)

#### 17.5.1、基本介绍

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211025222700.png)

1. **RT（平均响应时间，秒级）**
	- 平均响应时间   **超出阈值**  且   **在时间窗口内通过的请求>=5**，两个条件同时满足后触发降级
	- 窗口期过后关闭断路器
	- RT最大4900（更大的需要通过-Dcsp.sentinel.statistic.max.rt=XXXX才能生效）
2. **异常比列（秒级）**
	- QPS >= 5 且异常比例（秒级统计）超过阈值时，触发降级；时间窗口结束后，关闭降级
3. **异常数（分钟级）**
	- 异常数（分钟统计）超过阈值时，触发降级；时间窗口结束后，关闭降级
4. 进一步说明
	- Sentinel 熔断降级会在调用链路中某个资源出现不稳定状态时（例如调用超时或异常比例升高），对这个资源的调用进行限制，让请求快速失败，避免影响到其它的资源而导致级联错误
	- 当资源被降级后，在接下来的降级时间窗口之内，对该资源的调用都自动熔断（默认行为是抛出 DegradeException）
	- Sentinel的断路器是**没有半开**状态的
		- 半开的状态系统自动去检测是否请求有异常，没有异常就关闭断路器恢复使用，有异常则继续打开断路器不可用。具体可以参考Hystrix


#### 17.5.2、降级策略实战

##### 17.5.2.1、RT是什么？

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026104635.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026104702.png)

##### 17.5.2.2、测试

1. 代码

~~~Java
@GetMapping("/testD")
public String testD()
{
    //暂停几秒钟线程
    try { TimeUnit.SECONDS.sleep(1); } catch (InterruptedException e) { e.printStackTrace(); }
    log.info("testD 测试RT");
    return "------testD";
}
~~~

2.  配置
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026104834.png)
	
3.  jmeter压测
4.  结论

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026105243.png)

- 按照上述配置
 - 永远一秒钟打进来10个线程（大于5个了）调用testD，我们希望200毫秒处理完本次任务，如果超过200毫秒还没处理完，在未来1秒钟的时间窗口内，断路器打开(保险丝跳闸)微服务不可用，保险丝跳闸断电了
- 后续我停止jmeter，没有这么大的访问量了，断路器关闭(保险丝恢复)，微服务恢复OK

##### 17.5.2.3、异常比例是什么？

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026110436.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026110528.png)

##### 17.5.2.4、测试

1. 代码
	~~~Java
		@GetMapping("/testD")
	public String testD()
	{
		log.info("testD 测试RT");
		int age = 10/0;
		return "------testD";
	}
	~~~
2. 配置
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026110746.png)
	
3. jmeter压测
4. 结论
	- 按照上述配置，**单独访问一次**，必然来一次报错一次(int age  = 10/0)，调一次错一次
	- 开启jmeter后，直接高并发发送请求，多次调用达到我们的配置条件了
	- 断路器开启(保险丝跳闸)，微服务不可用了，不再报错error而是服务降级了

##### 17.5.2.5、异常数是什么？

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026111734.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026111803.png)

**异常数是按照分钟统计的**

##### 17.5.2.6、测试

1. 代码
	~~~Java
	@GetMapping("/testE")
	public String testE()
	{
		log.info("testE 测试异常数");
		int age = 10/0;
		return "------testE 测试异常数";
	~~~
2. 配置
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026111942.png)
	
	- http://localhost:8401/testE，第一次访问绝对报错，因为除数不能为零，我们看到error窗口，但是达到5次报错后，进入熔断后降级

### 17.6、热点key限流

#### 17.6.1、基本介绍

**何为热点**
- 热点即经常访问的数据，很多时候我们希望统计或者限制某个热点数据中访问频次最高的TopN数据，并对其访问进行限流或者其它操作

[# 热点参数限流指导文档](https://github.com/alibaba/Sentinel/wiki/%E7%83%AD%E7%82%B9%E5%8F%82%E6%95%B0%E9%99%90%E6%B5%81)

#### 17.6.2、承上启下复习
- **兜底方法，分为系统默认和客户自定义，两种**
- 之前的case，限流出问题后，都是用sentinel系统默认的提示：Blocked by Sentinel (flow limiting)
- 我们能不能自定?类似hystrix，某个方法出问题了，就找对应的兜底降级方法？
- 结论：**从HystrixCommand 到@SentinelResource**

代码：com.alibaba.csp.sentinel.slots.block.BlockException

~~~Java
@GetMapping("/testHotKey")
@SentinelResource(value = "testHotKey",blockHandler = "dealHandler_testHotKey")
public String testHotKey(@RequestParam(value = "p1",required = false) String p1, 
                         @RequestParam(value = "p2",required = false) String p2){
    return "------testHotKey";
}
public String dealHandler_testHotKey(String p1,String p2,BlockException exception)
{
    return "-----dealHandler_testHotKey";
}
~~~
- sentinel系统默认的提示：Blocked by Sentinel (flow limiting)

#### 17.6.3、配置

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026115340.png)


两种情况
- 第一种情况
	- `@SentinelResource(value = "testHotKey")`
	- 异常打到了前台用户界面看到，不友好
- 第二种情况
	- `@SentinelResource(value = "testHotKey",blockHandler ="dealHandler_testHotKey")`
	- 方法testHotKey里面第一个参数只要QPS超过每秒1次，马上降级处理，用了我们自己定义的

#### 17.6.4、测试
- `error`：http://localhost:8401/testHotKey?p1=abc
- `error`：http://localhost:8401/testHotKey?p1=abc&p2=33
- `right`：http://localhost:8401/testHotKey?p2=abc

#### 17.6.5、参数例外项(重点！！！！！)
- 上述案例演示了第一个参数p1，当QPS超过1秒1次点击后马上被限流
- 特例情况
	- 普通：超过1秒钟一个后，达到阈值1后马上被限流
	- 我们期望p1参数当它是**某个特殊值**时，它的限流值和平时不一样
	- 特例：假如当p1的值等于5时，它的阈值可以达到200
- 配置
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026160042.png)
	
- 测试
	- `right`：http://localhost:8401/testHotKey?p1=5
	- `error`：http://localhost:8401/testHotKey?p1=3
	- 当p1等于5的时候，阈值变为200
	- 当p1不等于5的时候，阈值就是平常的1
- **前提条件**：热点参数的注意点，参数必须是基本类型或者String
	
#### 17.6.6、其它

当你手动在代码中添加一个异常再次访问时，会直接出现报错界面，而不是你设置的兜底方法

- **@SentinelResource**
	- 处理的是Sentinel控制台配置的违规情况，有blockHandler方法配置的兜底处理
- **RuntimeException**
	- int age = 10/0,这个是java运行时报出的运行时异常RunTimeException，@SentinelResource不管
- 总结：**@SentinelResource主管配置出错，运行出错该走异常走异常**


### 17.7、系统规则

[系统自适应限流](https://github.com/alibaba/Sentinel/wiki/%E7%B3%BB%E7%BB%9F%E8%87%AA%E9%80%82%E5%BA%94%E9%99%90%E6%B5%81)

各项配置参数说明

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026162053.png)

可以配置一个全局QPS

### 17.8、@SentinelResource

#### 17.8.1、按资源名称限流+后续处理

1. 启动Nacos成功
2. 启动Sentinel成功：java -jar sentinel-dashboard-1.7.0.jar
3. 修改cloud-alibaba-sentinel-service8401
	1. 修改POM，引入我们自定义的实体类
		~~~xml
		<dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
			<groupId>com.clover.springcloud</groupId>
			<artifactId>cloud-api-commons</artifactId>
			<version>${project.version}</version>
		</dependency>
		~~~
	2. 增加一个业务类：RateLimitController
		~~~Java
		package com.clover.springcloud.alibaba.controller;

		import com.alibaba.csp.sentinel.annotation.SentinelResource;
		import com.alibaba.csp.sentinel.slots.block.BlockException;
		import com.clover.springcloud.entities.CommonResult;
		import com.clover.springcloud.entities.Payment;
		import org.springframework.web.bind.annotation.GetMapping;
		import org.springframework.web.bind.annotation.RestController;

		@RestController
		public class RateLimitController {
			@GetMapping("/byResource")
			@SentinelResource(value = "byResource",blockHandler = "handleException")
			public CommonResult byResource()
			{
				return new CommonResult(200,"按资源名称限流测试OK",new Payment(2020L,"serial001"));
			}
			public CommonResult handleException(BlockException exception)
			{
				return new CommonResult(444,exception.getClass().getCanonicalName()+"\t 服务不可用");
			}
		}
		~~~
4. 配置流控规则
	- 图形配置和代码关系
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026163646.png)
	
	- 表示1秒钟内查询次数大于1，就跑到我们自定义的处流，限流
5. 测试
	- 1秒钟点击1下，OK
	- 超过上述，疯狂点击，返回了自己定义的限流处理信息，限流发生
6. **额外问题**
	- 此时关闭问服务8401看看
	- **Sentinel控制台，流控规则消失了？？？？？**
		- **临时/持久？**

#### 17.8.2、按照URL地址限流+后续处理
1. 通过访问的URL来限流，会返回Sentinel自带默认的限流处理信息
2. 修改业务类RateLimitController
	~~~Java
	@GetMapping("/rateLimit/byUrl")
		@SentinelResource(value = "byUrl")
		public CommonResult byUrl()
		{
			return new CommonResult(200,"按url限流测试OK",new Payment(2020L,"serial002"));
		}	
	~~~
3. 访问一次 ：http://localhost:8401/rateLimit/byUrl
4. Sentinel控制台配置
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026163923.png)
	
5. 测试
	- 疯狂点击http://localhost:8401/rateLimit/byUrl
	- 结果：会返回Sentinel自带的限流处理结果



#### 17.8.3、上面兜底方案面临的问题
1. 系统默认的，没有体现我们自己的业务要求
2. 依照现有条件，我们自定义的处理方法又和业务代码耦合在一块，不直观
3. 每个业务方法都添加一个兜底的，那代码膨胀加剧
4. 全局统一的处理方法没有体现


#### 17.8.4、客户自定义限流处理逻辑

1. 创建CustomerBlockHandler类用于自定义限流处理逻辑
2. 自定义限流处理类：CustomerBlockHandler
	~~~Java
	package com.clover.springcloud.alibaba.myhandler;

	import com.alibaba.csp.sentinel.slots.block.BlockException;
	import com.clover.springcloud.entities.CommonResult;

	public class CustomerBlockHandler {
		public static CommonResult handleException(BlockException exception){
			return new CommonResult(2020,"自定义的限流处理信息......CustomerBlockHandler---handleException");
		}

		public static CommonResult handleException2(BlockException exception){
			return new CommonResult(2020,"自定义的限流处理信息......CustomerBlockHandler---handleException2");
		}
	}
	~~~
3. 启动微服务后先调用一次
	- http://localhost:8401/rateLimit/customerBlockHandler
4. Sentinel控制台配置
5. 测试后我们自定义的出来了
6. 进一步说明
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026164936.png)
	
	
#### 17.8.5、更多注解属性说明
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026170051.png)
1. 多说一句
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026170115.png)
	
	- 所有的代码都要用try-catch-finally方式进行处理
2. Sentinel主要有三个核心Api
	- SphU定义资源
	- Tracer定义统计
	- ContextUtil定义了上下文


### 17.9、服务熔断功能

sentinel整合ribbon+openFeign+fallback

#### 17.9.1、Ribbon系列
1. 启动nacos和sentinel
2. 提供者9003/9004
	1. 新建cloud-alibaba-provider-payment9003/9004两个一样的做法
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

			<artifactId>cloud-alibaba-provider-payment9003</artifactId>


			<dependencies>
				<!--SpringCloud ailibaba nacos -->
				<dependency>
					<groupId>com.alibaba.cloud</groupId>
					<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
				</dependency>
				<dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
					<groupId>com.clover.springcloud</groupId>
					<artifactId>cloud-api-commons</artifactId>
					<version>${project.version}</version>
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
	3. 编写YML：记得修改不同的端口号
		~~~yml
		server:
		  port: 9003

		spring:
		  application:
			name: nacos-payment-provider
		  cloud:
			nacos:
			  discovery:
				server-addr: localhost:8848 #配置Nacos地址

		management:
		  endpoints:
			web:
			  exposure:
				include: '*'		
		~~~
	4. 编写主启动类
		~~~Java
		package com.clover.springcloud.alibaba;

		import org.springframework.boot.SpringApplication;
		import org.springframework.boot.autoconfigure.SpringBootApplication;
		import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

		@SpringBootApplication
		@EnableDiscoveryClient
		public class PaymentMain9003 {
			public static void main(String[] args)
			{
				SpringApplication.run(PaymentMain9003.class,args);
			} 
		}		
		~~~
	5. 编写业务类
		~~~Java
		package com.clover.springcloud.alibaba.controller;

		import com.clover.springcloud.entities.CommonResult;
		import com.clover.springcloud.entities.Payment;
		import org.springframework.beans.factory.annotation.Value;
		import org.springframework.web.bind.annotation.GetMapping;
		import org.springframework.web.bind.annotation.PathVariable;
		import org.springframework.web.bind.annotation.RestController;

		import java.util.HashMap;

		@RestController
		public class PaymentController {
			@Value("${server.port}")
			private String serverPort;

			// 模拟一个数据库的操作
			public static HashMap<Long, Payment> hashMap = new HashMap<>();
			static
			{
				hashMap.put(1L,new Payment(1L,"28a8c1e3bc2742d8848569891fb42181"));
				hashMap.put(2L,new Payment(2L,"bba8c1e3bc2742d8848569891ac32182"));
				hashMap.put(3L,new Payment(3L,"6ua8c1e3bc2742d8848569891xt92183"));
			}

			@GetMapping(value = "/paymentSQL/{id}")
			public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id)
			{
				Payment payment = hashMap.get(id);
				CommonResult<Payment> result = new CommonResult(200,"from mysql,serverPort:  "+serverPort,payment);
				return result;
			}
		}		
		~~~
	6. 测试地址：http://localhost:9003/paymentSQL/1
3. 消费者84
	1. 新建cloud-alibaba-consumer-nacos-order84
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

			<artifactId>cloud-alibaba-consumer-nacos-order84</artifactId>

			<dependencies>
				<!--SpringCloud ailibaba nacos -->
				<dependency>
					<groupId>com.alibaba.cloud</groupId>
					<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
				</dependency>
				<!--SpringCloud ailibaba sentinel -->
				<dependency>
					<groupId>com.alibaba.cloud</groupId>
					<artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
				</dependency>
				<!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
				<dependency>
					<groupId>com.clover.springcloud</groupId>
					<artifactId>cloud-api-commons</artifactId>
					<version>${project.version}</version>
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
	3. 编写YML
		~~~yml
		server:
		  port: 84


		spring:
		  application:
			name: nacos-order-consumer
		  cloud:
			nacos:
			  discovery:
				server-addr: localhost:8848
			sentinel:
			  transport:
				#配置Sentinel dashboard地址
				dashboard: localhost:8080
				#默认8719端口，假如被占用会自动从8719开始依次+1扫描,直至找到未被占用的端口
				port: 8719


		#消费者将要去访问的微服务名称(注册成功进nacos的微服务提供者)
		service-url:
		  nacos-user-service: http://nacos-payment-provider
		~~~
	4. 编写主启动类
		~~~Java
		package com.clover.springcloud.alibaba;

		import org.springframework.boot.SpringApplication;
		import org.springframework.boot.autoconfigure.SpringBootApplication;
		import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

		@SpringBootApplication
		@EnableDiscoveryClient
		public class OrderNacosMain84 {
			public static void main(String[] args)
			{
				SpringApplication.run(OrderNacosMain84.class,args);
			}
		}
		~~~
	5. 编写业务类
		1. ApplicationContextConfig
			~~~Java
			package com.clover.springcloud.alibaba.config;

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
		2. **CircleBreakerController **
			1. 修改后轻重启微服务
				- 热部署对java代码级生效及时
				- 对@SentinelResource注解内属性，有时效果不好
			2. 目的
				- fallback管运行异常
				- blockHandler管配置违规
			3. 测试地址：http://localhost:84/consumer/fallback/1
			4. 没有任何配置
				- 给客户error页面，不友好
				~~~Java
				package com.clover.springcloud.alibaba.controller;

				import com.alibaba.csp.sentinel.annotation.SentinelResource;
				import com.clover.springcloud.entities.CommonResult;
				import com.clover.springcloud.entities.Payment;
				import lombok.extern.slf4j.Slf4j;
				import org.springframework.web.bind.annotation.PathVariable;
				import org.springframework.web.bind.annotation.RequestMapping;
				import org.springframework.web.bind.annotation.RestController;
				import org.springframework.web.client.RestTemplate;

				import javax.annotation.Resource;

				@RestController
				@Slf4j
				public class CircleBreakerController {
					public static final String SERVICE_URL = "http://nacos-payment-provider";

					@Resource
					private RestTemplate restTemplate;

					@RequestMapping("/consumer/fallback/{id}")
					@SentinelResource(value = "fallback")
					public CommonResult<Payment> fallback(@PathVariable Long id)
					{
						CommonResult<Payment> result = restTemplate.getForObject(SERVICE_URL + "/paymentSQL/"+id,CommonResult.class,id);

						if (id == 4) {
							throw new IllegalArgumentException ("IllegalArgumentException,非法参数异常....");
						}else if (result.getData() == null) {
							throw new NullPointerException ("NullPointerException,该ID没有对应记录,空指针异常");
						}

						return result;
					}
				}
				~~~
			5. 只配置fallback
				- 本例sentinel无配置
				
				![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026202550.png)
				
				- 结果
				- ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026202751.png)
				- ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026202839.png)
				- ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026202854.png)
				
				~~~Java
				package com.clover.springcloud.alibaba.controller;

				import com.alibaba.csp.sentinel.annotation.SentinelResource;
				import com.clover.springcloud.entities.CommonResult;
				import com.clover.springcloud.entities.Payment;
				import lombok.extern.slf4j.Slf4j;
				import org.springframework.web.bind.annotation.PathVariable;
				import org.springframework.web.bind.annotation.RequestMapping;
				import org.springframework.web.bind.annotation.RestController;
				import org.springframework.web.client.RestTemplate;

				import javax.annotation.Resource;

				@RestController
				@Slf4j
				public class CircleBreakerController {
					public static final String SERVICE_URL = "http://nacos-payment-provider";

					@Resource
					private RestTemplate restTemplate;

					@RequestMapping("/consumer/fallback/{id}")
				//    @SentinelResource(value = "fallback")// 没有配置
					@SentinelResource(value = "fallback",fallback = "handlerFallback") //fallback负责业务异常
					public CommonResult<Payment> fallback(@PathVariable Long id)
					{
						CommonResult<Payment> result = restTemplate.getForObject(SERVICE_URL + "/paymentSQL/"+id,CommonResult.class,id);

						if (id == 4) {
							throw new IllegalArgumentException ("IllegalArgumentException,非法参数异常....");
						}else if (result.getData() == null) {
							throw new NullPointerException ("NullPointerException,该ID没有对应记录,空指针异常");
						}

						return result;
					}

					// fallback方法
					public CommonResult handlerFallback(@PathVariable  Long id,Throwable e) {
						Payment payment = new Payment(id,"null");
						return new CommonResult<>(444,"兜底异常handlerFallback,exception内容  "+e.getMessage(),payment);
					}
				}
				~~~
			6. 只配置blockHandler
				- 本例sentinel需要配置，随意配置哪一个都可以
				
				![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026203447.png)
				
				- 结果
				- ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026203737.png)
				
				~~~Java
				package com.clover.springcloud.alibaba.controller;

				import com.alibaba.csp.sentinel.annotation.SentinelResource;
				import com.alibaba.csp.sentinel.slots.block.BlockException;
				import com.clover.springcloud.entities.CommonResult;
				import com.clover.springcloud.entities.Payment;
				import lombok.extern.slf4j.Slf4j;
				import org.springframework.web.bind.annotation.PathVariable;
				import org.springframework.web.bind.annotation.RequestMapping;
				import org.springframework.web.bind.annotation.RestController;
				import org.springframework.web.client.RestTemplate;

				import javax.annotation.Resource;

				@RestController
				@Slf4j
				public class CircleBreakerController {
					public static final String SERVICE_URL = "http://nacos-payment-provider";

					@Resource
					private RestTemplate restTemplate;

					@RequestMapping("/consumer/fallback/{id}")
				//    @SentinelResource(value = "fallback")// 没有配置
				//    @SentinelResource(value = "fallback",fallback = "handlerFallback") //fallback负责业务异常
					@SentinelResource(value = "fallback",blockHandler = "blockHandler") //blockHandler负责在sentinel里面配置的降级限流
					public CommonResult<Payment> fallback(@PathVariable Long id)
					{
						CommonResult<Payment> result = restTemplate.getForObject(SERVICE_URL + "/paymentSQL/"+id,CommonResult.class,id);

						if (id == 4) {
							throw new IllegalArgumentException ("IllegalArgumentException,非法参数异常....");
						}else if (result.getData() == null) {
							throw new NullPointerException ("NullPointerException,该ID没有对应记录,空指针异常");
						}

						return result;
					}

					// 本例是fallback
				//    public CommonResult handlerFallback(@PathVariable  Long id,Throwable e) {
				//        Payment payment = new Payment(id,"null");
				//        return new CommonResult<>(444,"兜底异常handlerFallback,exception内容  "+e.getMessage(),payment);
				//    }

					// 本例是blockHandler
					public CommonResult blockHandler(@PathVariable  Long id, BlockException blockException) {
						Payment payment = new Payment(id,"null");
						return new CommonResult<>(445,"blockHandler-sentinel限流,无此流水: blockException  "+blockException.getMessage(),payment);
					}
				}
				~~~
				
			7. fallback和blockHandler都配置
				- 本例sentinel需要配置，随意配置哪一个都可以
				
				![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026204231.png)
				
				- 结果
				- 若 blockHandler 和 fallback 都进行了配置，则被限流降级而抛出 BlockException 时只会进入 blockHandler 处理逻辑
				- ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026204512.png)

				~~~Java
				package com.clover.springcloud.alibaba.controller;

				import com.alibaba.csp.sentinel.annotation.SentinelResource;
				import com.alibaba.csp.sentinel.slots.block.BlockException;
				import com.clover.springcloud.entities.CommonResult;
				import com.clover.springcloud.entities.Payment;
				import lombok.extern.slf4j.Slf4j;
				import org.springframework.web.bind.annotation.PathVariable;
				import org.springframework.web.bind.annotation.RequestMapping;
				import org.springframework.web.bind.annotation.RestController;
				import org.springframework.web.client.RestTemplate;

				import javax.annotation.Resource;

				@RestController
				@Slf4j
				public class CircleBreakerController {
					public static final String SERVICE_URL = "http://nacos-payment-provider";

					@Resource
					private RestTemplate restTemplate;

					@RequestMapping("/consumer/fallback/{id}")
				//    @SentinelResource(value = "fallback")// 没有配置
				//    @SentinelResource(value = "fallback",fallback = "handlerFallback") //fallback负责业务异常
				//    @SentinelResource(value = "fallback",blockHandler = "blockHandler") //blockHandler负责在sentinel里面配置的降级限流
					@SentinelResource(value = "fallback",fallback = "handlerFallback",blockHandler = "blockHandler")
					public CommonResult<Payment> fallback(@PathVariable Long id)
					{
						CommonResult<Payment> result = restTemplate.getForObject(SERVICE_URL + "/paymentSQL/"+id,CommonResult.class,id);

						if (id == 4) {
							throw new IllegalArgumentException ("IllegalArgumentException,非法参数异常....");
						}else if (result.getData() == null) {
							throw new NullPointerException ("NullPointerException,该ID没有对应记录,空指针异常");
						}

						return result;
					}

					// 本例是fallback
					public CommonResult handlerFallback(@PathVariable  Long id,Throwable e) {
						Payment payment = new Payment(id,"null");
						return new CommonResult<>(444,"兜底异常handlerFallback,exception内容  "+e.getMessage(),payment);
					}

					// 本例是blockHandler
					public CommonResult blockHandler(@PathVariable  Long id, BlockException blockException) {
						Payment payment = new Payment(id,"null");
						return new CommonResult<>(445,"blockHandler-sentinel限流,无此流水: blockException  "+blockException.getMessage(),payment);
					}
				}
				~~~
			8. 忽略属性.......
				1. 本例sentinel无需配置
				
				![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026204930.png)
				
				2. 结果
					- 程序异常打到前台了，对用户不友好
					- ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026205055.png)

#### 17.9.2、Feign系列

1. 修改84模块
2. 修改POM
~~~xml
<!--SpringCloud openfeign -->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
~~~
3. 修改YML：激活Sentinel对Feign的支持
~~~yml
# 激活Sentinel对Feign的支持
feign:
  sentinel:
    enabled: true  
~~~
4. 修改业务类
	1. 带`@FeignClient注解的业务接口`
	~~~Java
	package com.clover.springcloud.alibaba.service;

	import com.clover.springcloud.entities.CommonResult;
	import com.clover.springcloud.entities.Payment;
	import org.springframework.cloud.openfeign.FeignClient;
	import org.springframework.web.bind.annotation.GetMapping;
	import org.springframework.web.bind.annotation.PathVariable;

	@FeignClient(value = "nacos-payment-provider",fallback = PaymentFallbackService.class)//调用中关闭9003服务提供者
	public interface PaymentService {
		@GetMapping(value = "/paymentSQL/{id}")
		public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id);
	}
	~~~
	2. **fallback = PaymentFallbackService.class**
	~~~Java
	package com.clover.springcloud.alibaba.service;

	import com.clover.springcloud.entities.CommonResult;
	import com.clover.springcloud.entities.Payment;
	import org.springframework.stereotype.Component;

	@Component
	public class PaymentFallbackService implements PaymentService {
		@Override
		public CommonResult<Payment> paymentSQL(Long id) {
			return new CommonResult<>(444, "服务降级返回,没有该流水信息", new Payment(id, "errorSerial......"));
		}
	}
	~~~
	3. 修改controller
	~~~Java
	//==================OpenFeign
	@Resource
	private PaymentService paymentService;

	@GetMapping(value = "/consumer/openfeign/{id}")
	public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id)
	{
		if(id == 4)
		{
			throw new RuntimeException("没有该id");
		}
		return paymentService.paymentSQL(id);
	}
	~~~
	
5. 修改主启动类：添加 **@EnableFeignClients** 启动Feign的功能
~~~Java
package com.clover.springcloud.alibaba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableDiscoveryClient
@EnableFeignClients
public class OrderNacosMain84 {
    public static void main(String[] args)
    {
        SpringApplication.run(OrderNacosMain84.class,args);
    }
}
~~~


### 17.10、规则持久化
- 是什么？
	- 一旦我们重启应用，sentinel规则将消失，生产环境需要将配置规则进行持久化
- 怎么玩？
	- 将限流配置规则持久化进Nacos保存，只要刷新8401某个rest地址，sentinel控制台的流控规则就能看到，只要Nacos里面的配置不删除，针对8401上sentinel上的流控规则持续有效


#### 17.10.1、持久化步骤

1. 修改cloud-alibaba-sentinel-service8401
2. 修改POM
	~~~xml
	<!--SpringCloud ailibaba sentinel-datasource-nacos -->
	<dependency>
		<groupId>com.alibaba.csp</groupId>
		<artifactId>sentinel-datasource-nacos</artifactId>
	</dependency>
	~~~
3. 修改YML
	- 添加Nacos数据源配置
	~~~yml
	spring:
	  cloud:
		sentinel:
		  datasource:
			ds1:
			  nacos:
				server-addr: localhost:8848
				dataId: ${spring.application.name}
				groupId: DEFAULT_GROUP
				data-type: json
				rule-type: flow
	~~~
	
	~~~yml
	server:
	  port: 8401

	spring:
	  application:
		name: cloud-alibaba-sentinel-service
	  cloud:
		nacos:
		  discovery:
			#Nacos服务注册中心地址
			server-addr: localhost:8848
		sentinel:
		  transport:
			#配置Sentinel dashboard地址
			dashboard: localhost:8080
			#默认8719端口，假如被占用会自动从8719开始依次+1扫描,直至找到未被占用的端口
			port: 8719
		  datasource:
			ds1:
			  nacos:
				server-addr: localhost:8848
				dataId: cloud-alibaba-sentinel-service
				groupId: DEFAULT_GROUP
				data-type: json
				rule-type: flow

	management:
	  endpoints:
		web:
		  exposure:
			include: '*'
	~~~
4. 添加Nacos业务规则配置
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026213636.png)
	
	- 内容解析
	~~~json
	[
		{
			"resource": "/rateLimit/byUrl",
			"limitApp": "default",
			"grade": 1,
			"count": 1,
			"strategy": 0,
			"controlBehavior": 0,
			"clusterMode": false
		}
	]
	~~~
	
	- resource：资源名称；
	- limitApp：来源应用；
	- grade：阈值类型，0表示线程数，1表示QPS；
	- count：单机阈值；
	- strategy：流控模式，0表示直接，1表示关联，2表示链路；
	- controlBehavior：流控效果，0表示快速失败，1表示Warm Up，2表示排队等待；
	- clusterMode：是否集群
	
5. 启动8401后**刷新sentinel**发现业务规则有了
6. 快速访问测试接口：http://localhost:8401/rateLimit/byUrl
7. 停止8401再看sentinel
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211026214036.png)
	
8. 重新启动8401再看sentinel
	- 乍一看还是没有，稍等一会儿
	- 多次调用：http://localhost:8401/rateLimit/byUrl
	- 重新配置出现了，持久化验证通过

## 18、SpringCloud Alibaba Seata处理分布式事务

### 18.1、分布式事务问题

#### 18.1.1、分布式之前

- 单机单库没这个问题
- 从1：1  ->  1：N  ->  N：N


#### 18.1.2、分布式之后

- 单体应用被拆分成微服务应用，原来的三个模块被拆分成**三个独立的应用**，分别使用**三个独立的数据源**
- 业务操作需要调用三个服务来完成。此时**每个服务内部的数据一致性由`本地`事务来保证**，**但是`全局`的数据一致性问题没法保证**

**一句话**：`一次业务操作需要跨多个数据源或需要跨多个系统进行远程调用，就会产生分布式事务问题`

### 18.2、 Seata简介

#### 18.2.1、Seata是什么？

Seata是一款开源的分布式事务解决方案，致力于在微服务架构下提供高性能和简单易用的分布式事务服务。

[官网地址](http://seata.io/zh-cn/)

#### 18.2.2、能干嘛？
- 一个典型的分布式事务过程
	- 分布式事务处理过程的一ID+三组件模型
		- **Transaction ID XID**：全局唯一的事务ID
		- **Transaction Coordinator (TC)**：事务协调器，维护全局事务的运行状态，负责协调并驱动全局事务的提交或回滚；
		- **Transaction Manager (TM)**：控制全局事务的边界，负责开启一个全局事务，并最终发起全局提交或全局回滚的决议；
		- **Resource Manager (RM)**：控制分支事务，负责分支注册、状态汇报，并接收事务协调器的指令，驱动分支（本地）事务的提交和回滚
	- 处理过程
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027113015.png)
	
	1. TM 向 TC 申请开启一个全局事务，全局事务创建成功并生成一个全局唯一的 XID；
	2. XID 在微服务调用链路的上下文中传播；
	3. RM 向 TC 注册分支事务，将其纳入 XID 对应全局事务的管辖；
	4. TM 向 TC 发起针对 XID 的全局提交或回滚决议；
	5. TC 调度 XID 下管辖的全部分支事务完成提交或回滚请求。

	简单解释：
	- TM：代表一个班级班主任
	- TC：代表一个任课老师
	- RM：代表上课学生

	1. 班主任老师向任课老师说，我们这个课程可以开了嘛？老师说好，然后班主任去创建一个班级号，这个就是我们的XID，全局唯一
	2. 创建班级号后，班主任老师就去微信群里面发送该班级号，让选了该课程的学生进入到该课程的学习
	3. 同学就通过班主任给的班级号进入到直播中来学习，老师就可以实时的看到有多少人成功进入，老师就会把这些学生纳入到该班级管理中
	4. 班主任说让学生进行一个签到，然后告诉老师，说学生全部进入，可以开始上课了
	5. 当老师将该课程上完的时候，老师就会将该班级该门课程进行结课


#### 18.2.3、去哪下？

[发布说明](https://github.com/seata/seata/releases)

#### 18.2.4、怎么玩？
1. **本地@Transactional**
2. **全局@GlobalTransactional**	
	- SEATA 的分布式交易解决方案
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027113819.png)
	
	
### 18.3、Seata-Server安装
1. 官网地址：[官网地址](http://seata.io/zh-cn/)
2. 下载版本
	- https://github.com/seata/seata/releases
	- 下载的是seata-server-0.9.0.zip
3. seata-server-0.9.0.zip解压到指定目录并修改conf目录下的file.conf配置文件	
	- 先备份原始file.conf文件
	- 主要修改：**自定义事务组名称+事务日志存储模式为db+数据库连接信息**
	- file.conf
		- service模块
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027115504.png)
		
		- store模块
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027115533.png)
		
4. mysql5.7数据库新建库seata
5. 在seata库里建表
	- 建表db_store.sql在\seata-server-0.9.0\seata\conf目录里面：db_store.sql
6. 修改seata-server-0.9.0\seata\conf目录下的registry.conf配置文件
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027115630.png)
	
7. 先启动Nacos端口号8848
8. 再启动seata-server

### 18.4、订单/库存/账户业务数据库准备

1. 以下演示都需要**先启动Nacos后启动Seata**，保证两个都OK
	- Seata没启动报错no available server to connect
2. 分布式事务业务说明
	- 这里我们会创建三个服务，一个订单服务，一个库存服务，一个账户服务
	- **当用户下单时，会在订单服务中创建一个订单，然后通过远程调用库存服务来扣减下单商品的库存**
	- **再通过远程调用账户服务来扣减用户账户里面的余额**
	- **最后在订单服务中修改订单状态为已完成**
	- 该操作跨越三个数据库，有两次远程调用，很明显会有分布式事务问题
	- 下订单--->扣库存--->减账户(余额)
3. 创建业务数据库
	- `seata_order`：存储订单的数据库
	- `seata_storage`：存储库存的数据库
	- `seata_account`：存储账户信息的数据库
4. 按照上述3库分别建对应业务表
	- seata_order库下建t_order表
		~~~sql
		CREATE TABLE t_order (
		  `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
		  `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',
		  `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',
		  `count` INT(11) DEFAULT NULL COMMENT '数量',
		  `money` DECIMAL(11,0) DEFAULT NULL COMMENT '金额',
		  `status` INT(1) DEFAULT NULL COMMENT '订单状态：0：创建中；1：已完结' 
		) ENGINE=INNODB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;

		SELECT * FROM t_order;	
		~~~
	- seata_storage库下建t_storage 表
		~~~sql
		CREATE TABLE t_storage (
		 `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
		 `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',
		 `total` INT(11) DEFAULT NULL COMMENT '总库存',
		 `used` INT(11) DEFAULT NULL COMMENT '已用库存',
		 `residue` INT(11) DEFAULT NULL COMMENT '剩余库存'
		) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;


		INSERT INTO seata_storage.t_storage(`id`, `product_id`, `total`, `used`, `residue`)
		VALUES ('1', '1', '100', '0', '100');

		SELECT * FROM t_storage;
		~~~
	- seata_account库下建t_account 
		~~~sql
		CREATE TABLE t_account (
		  `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY COMMENT 'id',
		  `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',
		  `total` DECIMAL(10,0) DEFAULT NULL COMMENT '总额度',
		  `used` DECIMAL(10,0) DEFAULT NULL COMMENT '已用余额',
		  `residue` DECIMAL(10,0) DEFAULT '0' COMMENT '剩余可用额度'
		) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

		INSERT INTO seata_account.t_account(`id`, `user_id`, `total`, `used`, `residue`)  VALUES ('1', '1', '1000', '0', '1000');

		SELECT * FROM t_account;
		~~~
5. 按照上述3库分别建对应的回滚日志表
	- 订单-库存-账户3个库下都需要建各自的回滚日志表
	- \seata-server-0.9.0\seata\conf目录下的db_undo_log.sql
		~~~sql
		-- the table to store seata xid data
		-- 0.7.0+ add context
		-- you must to init this sql for you business databese. the seata server not need it.
		-- 此脚本必须初始化在你当前的业务数据库中，用于AT 模式XID记录。与server端无关（注：业务数据库）
		-- 注意此处0.3.0+ 增加唯一索引 ux_undo_log
		drop table `undo_log`;
		CREATE TABLE `undo_log` (
		  `id` bigint(20) NOT NULL AUTO_INCREMENT,
		  `branch_id` bigint(20) NOT NULL,
		  `xid` varchar(100) NOT NULL,
		  `context` varchar(128) NOT NULL,
		  `rollback_info` longblob NOT NULL,
		  `log_status` int(11) NOT NULL,
		  `log_created` datetime NOT NULL,
		  `log_modified` datetime NOT NULL,
		  `ext` varchar(100) DEFAULT NULL,
		  PRIMARY KEY (`id`),
		  UNIQUE KEY `ux_undo_log` (`xid`,`branch_id`)
		) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
		~~~
		

### 18.5、订单/库存/账户业务微服务准备

#### 18.5.1、业务需求

下订单->减库存->扣余额->改(订单)状态

#### 18.5.2、新建订单Order-Module
1. 新建seata-order-service2001
2. 修改POM
	~~~pom
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

		<artifactId>seata-order-service2001</artifactId>


		<dependencies>
			<!--nacos-->
			<dependency>
				<groupId>com.alibaba.cloud</groupId>
				<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
			</dependency>
			<!--seata-->
			<dependency>
				<groupId>com.alibaba.cloud</groupId>
				<artifactId>spring-cloud-starter-alibaba-seata</artifactId>
				<exclusions>
					<exclusion>
						<artifactId>seata-all</artifactId>
						<groupId>io.seata</groupId>
					</exclusion>
				</exclusions>
			</dependency>
			<dependency>
				<groupId>io.seata</groupId>
				<artifactId>seata-all</artifactId>
				<version>0.9.0</version>
			</dependency>
			<!--feign-->
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-starter-openfeign</artifactId>
			</dependency>
			<!--web-actuator-->
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-web</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-actuator</artifactId>
			</dependency>
			<!--mysql-druid-->
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>5.1.37</version>
			</dependency>
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>druid-spring-boot-starter</artifactId>
				<version>1.1.10</version>
			</dependency>
			<dependency>
				<groupId>org.mybatis.spring.boot</groupId>
				<artifactId>mybatis-spring-boot-starter</artifactId>
				<version>2.0.0</version>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-test</artifactId>
				<scope>test</scope>
			</dependency>
			<dependency>
				<groupId>org.projectlombok</groupId>
				<artifactId>lombok</artifactId>
				<optional>true</optional>
			</dependency>
		</dependencies>

	</project>
	~~~
	
3. 编写YML
	~~~yml
	server:
	  port: 2001

	spring:
	  application:
		name: seata-order-service
	  cloud:
		alibaba:
		  seata:
			#自定义事务组名称需要与seata-server中的对应
			tx-service-group: clover_tx_group
		nacos:
		  discovery:
			server-addr: localhost:8848
	  datasource:
		driver-class-name: com.mysql.jdbc.Driver
		url: jdbc:mysql://localhost:3306/seata_order
		username: root
		password: xn123456

	feign:
	  hystrix:
		enabled: false

	logging:
	  level:
		io:
		  seata: info

	mybatis:
	  mapperLocations: classpath:mapper/*.xml
	~~~
	
4. 创建file.conf文件
	~~~conf
	transport {
	  # tcp udt unix-domain-socket
	  type = "TCP"
	  #NIO NATIVE
	  server = "NIO"
	  #enable heartbeat
	  heartbeat = true
	  #thread factory for netty
	  thread-factory {
		boss-thread-prefix = "NettyBoss"
		worker-thread-prefix = "NettyServerNIOWorker"
		server-executor-thread-prefix = "NettyServerBizHandler"
		share-boss-worker = false
		client-selector-thread-prefix = "NettyClientSelector"
		client-selector-thread-size = 1
		client-worker-thread-prefix = "NettyClientWorkerThread"
		# netty boss thread size,will not be used for UDT
		boss-thread-size = 1
		#auto default pin or 8
		worker-thread-size = 8
	  }
	  shutdown {
		# when destroy server, wait seconds
		wait = 3
	  }
	  serialization = "seata"
	  compressor = "none"
	}

	service {

	  vgroup_mapping.clover_tx_group = "default" #修改自定义事务组名称

	  default.grouplist = "127.0.0.1:8091"
	  enableDegrade = false
	  disable = false
	  max.commit.retry.timeout = "-1"
	  max.rollback.retry.timeout = "-1"
	  disableGlobalTransaction = false
	}


	client {
	  async.commit.buffer.limit = 10000
	  lock {
		retry.internal = 10
		retry.times = 30
	  }
	  report.retry.count = 5
	  tm.commit.retry.count = 1
	  tm.rollback.retry.count = 1
	}

	## transaction log store
	store {
	  ## store mode: file、db
	  mode = "db"

	  ## file store
	  file {
		dir = "sessionStore"

		# branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
		max-branch-session-size = 16384
		# globe session size , if exceeded throws exceptions
		max-global-session-size = 512
		# file buffer size , if exceeded allocate new buffer
		file-write-buffer-cache-size = 16384
		# when recover batch read size
		session.reload.read_size = 100
		# async, sync
		flush-disk-mode = async
	  }

	  ## database store
	  db {
		## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
		datasource = "dbcp"
		## mysql/oracle/h2/oceanbase etc.
		db-type = "mysql"
		driver-class-name = "com.mysql.jdbc.Driver"
		url = "jdbc:mysql://127.0.0.1:3306/seata"
		user = "root"
		password = "xn123456"
		min-conn = 1
		max-conn = 3
		global.table = "global_table"
		branch.table = "branch_table"
		lock-table = "lock_table"
		query-limit = 100
	  }
	}
	lock {
	  ## the lock store mode: local、remote
	  mode = "remote"

	  local {
		## store locks in user's database
	  }

	  remote {
		## store locks in the seata's server
	  }
	}
	recovery {
	  #schedule committing retry period in milliseconds
	  committing-retry-period = 1000
	  #schedule asyn committing retry period in milliseconds
	  asyn-committing-retry-period = 1000
	  #schedule rollbacking retry period in milliseconds
	  rollbacking-retry-period = 1000
	  #schedule timeout retry period in milliseconds
	  timeout-retry-period = 1000
	}

	transaction {
	  undo.data.validation = true
	  undo.log.serialization = "jackson"
	  undo.log.save.days = 7
	  #schedule delete expired undo_log in milliseconds
	  undo.log.delete.period = 86400000
	  undo.log.table = "undo_log"
	}

	## metrics settings
	metrics {
	  enabled = false
	  registry-type = "compact"
	  # multi exporters use comma divided
	  exporter-list = "prometheus"
	  exporter-prometheus-port = 9898
	}

	support {
	  ## spring
	  spring {
		# auto proxy the DataSource bean
		datasource.autoproxy = false
	  }
	}
	~~~
	
5. 创建registry.conf文件
	~~~conf
	registry {
	  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
	  type = "nacos"

	  nacos {
		serverAddr = "localhost:8848"
		namespace = ""
		cluster = "default"
	  }
	  eureka {
		serviceUrl = "http://localhost:8761/eureka"
		application = "default"
		weight = "1"
	  }
	  redis {
		serverAddr = "localhost:6379"
		db = "0"
	  }
	  zk {
		cluster = "default"
		serverAddr = "127.0.0.1:2181"
		session.timeout = 6000
		connect.timeout = 2000
	  }
	  consul {
		cluster = "default"
		serverAddr = "127.0.0.1:8500"
	  }
	  etcd3 {
		cluster = "default"
		serverAddr = "http://localhost:2379"
	  }
	  sofa {
		serverAddr = "127.0.0.1:9603"
		application = "default"
		region = "DEFAULT_ZONE"
		datacenter = "DefaultDataCenter"
		cluster = "default"
		group = "SEATA_GROUP"
		addressWaitTime = "3000"
	  }
	  file {
		name = "file.conf"
	  }
	}

	config {
	  # file、nacos 、apollo、zk、consul、etcd3
	  type = "file"

	  nacos {
		serverAddr = "localhost"
		namespace = ""
	  }
	  consul {
		serverAddr = "127.0.0.1:8500"
	  }
	  apollo {
		app.id = "seata-server"
		apollo.meta = "http://192.168.1.204:8801"
	  }
	  zk {
		serverAddr = "127.0.0.1:2181"
		session.timeout = 6000
		connect.timeout = 2000
	  }
	  etcd3 {
		serverAddr = "http://localhost:2379"
	  }
	  file {
		name = "file.conf"
	  }
	}
	~~~
	
6. 编写domain
	~~~Java
	package com.clover.springcloud.alibaba.domain;

	import lombok.AllArgsConstructor;
	import lombok.Data;
	import lombok.NoArgsConstructor;

	import java.math.BigDecimal;

	@Data
	@AllArgsConstructor
	@NoArgsConstructor
	public class Order {
		private Long id;

		private Long userId;

		private Long productId;

		private Integer count;

		private BigDecimal money;

		/**
		 * 订单状态：0：创建中；1：已完结
		 */
		private Integer status;
	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.domain;

	import lombok.AllArgsConstructor;
	import lombok.Data;
	import lombok.NoArgsConstructor;

	@Data
	@AllArgsConstructor
	@NoArgsConstructor
	public class CommonResult<T> {
		private Integer code;
		private String  message;
		private T       data;

		public CommonResult(Integer code, String message)
		{
			this(code,message,null);
		}
	}
	~~~
7. Dao接口及实现
	~~~Java
	package com.clover.springcloud.alibaba.dao;

	import com.clover.springcloud.alibaba.domain.Order;
	import org.apache.ibatis.annotations.Mapper;
	import org.apache.ibatis.annotations.Param;

	@Mapper
	public interface OrderDao {
		//1.新建订单
		void create(Order order);

		//2.修改订单状态
		void update(@Param("userId")  Long userId, @Param("status")  Integer status);
	}
	~~~
	
	- resources文件夹下新建mapper文件夹后添加
	
	~~~xml
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

	<mapper namespace="com.clover.springcloud.alibaba.dao.OrderDao">
		<resultMap id="baseResultMapper" type="com.clover.springcloud.alibaba.domain.Order">
			<id column="id" property="id" jdbcType="BIGINT"/>
			<result column="user_id" property="userId" jdbcType="BIGINT"/>
			<result column="product_id" property="productId" jdbcType="BIGINT"/>
			<result column="count" property="count" jdbcType="INTEGER"/>
			<result column="money" property="money" jdbcType="DECIMAL"/>
			<result column="status" property="status" jdbcType="INTEGER"/>
		</resultMap>

		<insert id="create">
			INSERT INTO t_order (id,user_id,product_id,count,money,status)
			VALUES (null,#{userId},#{productId},#{count},#{money},0);
		</insert>

		<update id="update">
			update t_order
			set status = 1
			where user_id = #{userId} and status = #{status}
		</update>

	</mapper>
	~~~
	
8. Service接口及实现
	~~~Java
	package com.clover.springcloud.alibaba.service;

	import com.clover.springcloud.alibaba.domain.CommonResult;
	import org.springframework.cloud.openfeign.FeignClient;
	import org.springframework.web.bind.annotation.PostMapping;
	import org.springframework.web.bind.annotation.RequestParam;

	import java.math.BigDecimal;

	@FeignClient(value = "seata-account-service")
	public interface AccountService {

		@PostMapping(value = "/account/decrease")
		CommonResult decrease(@RequestParam("userId") Long userId,@RequestParam("money") BigDecimal money);
	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.service;

	import com.clover.springcloud.alibaba.domain.Order;

	public interface OrderService {
		//1.创建订单
		void create(Order order);
	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.service;

	import com.clover.springcloud.alibaba.domain.CommonResult;
	import org.springframework.cloud.openfeign.FeignClient;
	import org.springframework.web.bind.annotation.PostMapping;
	import org.springframework.web.bind.annotation.RequestParam;

	@FeignClient(value = "seata-storage-service")
	public interface StorageService {

		@PostMapping(value = "/storage/decrease")
		CommonResult decrease(@RequestParam("productId") Long productId,@RequestParam("count") Integer count);
	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.service.Impl;

	import com.clover.springcloud.alibaba.dao.OrderDao;
	import com.clover.springcloud.alibaba.domain.Order;
	import com.clover.springcloud.alibaba.service.AccountService;
	import com.clover.springcloud.alibaba.service.OrderService;
	import com.clover.springcloud.alibaba.service.StorageService;
	import lombok.extern.slf4j.Slf4j;
	import org.springframework.stereotype.Service;

	import javax.annotation.Resource;

	@Service
	@Slf4j
	public class OrderServiceImpl implements OrderService{

		@Resource
		private OrderDao orderDao;

		@Resource
		private AccountService accountService;

		@Resource
		private StorageService storageService;


		@Override
		public void create(Order order) {
			log.info("------->订单开始创建");
			orderDao.create(order);

			log.info("------->order-service中扣减库存开始");
			storageService.decrease(order.getProductId(),order.getCount());
			log.info("------->order-service中扣减库存结束");

			log.info("------->order-service中扣减账户余额开始");
			accountService.decrease(order.getUserId(),order.getMoney());
			log.info("------->order-service中扣减账户余额结束");

			log.info("------->order-service中状态修改开始");
			orderDao.update(order.getUserId(),0);
			log.info("------->order-service中状态修改结束");

			log.info("------->订单创建结束");
		}
	}
	~~~

9. 编写Controller
	~~~Java
	package com.clover.springcloud.alibaba.controller;

	import com.clover.springcloud.alibaba.domain.CommonResult;
	import com.clover.springcloud.alibaba.domain.Order;
	import com.clover.springcloud.alibaba.service.OrderService;
	import lombok.extern.slf4j.Slf4j;
	import org.springframework.web.bind.annotation.GetMapping;
	import org.springframework.web.bind.annotation.RestController;

	import javax.annotation.Resource;

	@RestController
	@Slf4j
	public class OrderController {
		@Resource
		private OrderService orderService;

		@GetMapping(value = "order/create")
		public CommonResult create(Order order)
		{
			orderService.create(order);
			return new CommonResult(200,"创建订单成功");
		}
	}
	~~~
	
10. Config配置
	~~~Java
	package com.clover.springcloud.alibaba.config;

	import com.alibaba.druid.pool.DruidDataSource;
	import io.seata.rm.datasource.DataSourceProxy;
	import org.apache.ibatis.session.SqlSessionFactory;
	import org.mybatis.spring.SqlSessionFactoryBean;
	import org.mybatis.spring.transaction.SpringManagedTransactionFactory;
	import org.springframework.beans.factory.annotation.Value;
	import org.springframework.boot.context.properties.ConfigurationProperties;
	import org.springframework.context.annotation.Bean;
	import org.springframework.context.annotation.Configuration;
	import org.springframework.core.io.support.PathMatchingResourcePatternResolver;

	import javax.sql.DataSource;

	//使用Seata对数据源进行代理
	@Configuration
	public class DataSourceProxyConfig {
		@Value("${mybatis.mapperLocations}")
		private String mapperLocations;

		@Bean
		@ConfigurationProperties(prefix = "spring.datasource")
		public DataSource druidDataSource(){
			return new DruidDataSource();
		}

		@Bean
		public DataSourceProxy dataSourceProxy(DataSource dataSource) {
			return new DataSourceProxy(dataSource);
		}

		@Bean
		public SqlSessionFactory sqlSessionFactoryBean(DataSourceProxy dataSourceProxy) throws Exception {
			SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
			sqlSessionFactoryBean.setDataSource(dataSourceProxy);
			sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources(mapperLocations));
			sqlSessionFactoryBean.setTransactionFactory(new SpringManagedTransactionFactory());
			return sqlSessionFactoryBean.getObject();
		}

	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.config;

	import org.mybatis.spring.annotation.MapperScan;
	import org.springframework.context.annotation.Configuration;

	@Configuration
	@MapperScan({"com.clover.springcloud.alibaba.dao"})
	public class MyBatisConfig {
	}
	~~~
	
11. 主启动类
	~~~Java
	package com.clover.springcloud.alibaba;

	import org.springframework.boot.SpringApplication;
	import org.springframework.boot.autoconfigure.SpringBootApplication;
	import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
	import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
	import org.springframework.cloud.openfeign.EnableFeignClients;


	@EnableDiscoveryClient
	@EnableFeignClients
	@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)//取消数据源的自动创建
	public class SeataOrderMainApp2001 {
		public static void main(String[] args)
		{
			SpringApplication.run(SeataOrderMainApp2001.class,args);
		}
	}
	~~~


#### 18.5.3、新建库存Storage-Module
1. 新建seata-storage-service2002
2. 修改POM
	~~~pom
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

		<artifactId>seata-storage-service2002</artifactId>


		<dependencies>
			<!--nacos-->
			<dependency>
				<groupId>com.alibaba.cloud</groupId>
				<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
			</dependency>
			<!--seata-->
			<dependency>
				<groupId>com.alibaba.cloud</groupId>
				<artifactId>spring-cloud-starter-alibaba-seata</artifactId>
				<exclusions>
					<exclusion>
						<artifactId>seata-all</artifactId>
						<groupId>io.seata</groupId>
					</exclusion>
				</exclusions>
			</dependency>
			<dependency>
				<groupId>io.seata</groupId>
				<artifactId>seata-all</artifactId>
				<version>0.9.0</version>
			</dependency>
			<!--feign-->
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-starter-openfeign</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-web</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-test</artifactId>
				<scope>test</scope>
			</dependency>
			<dependency>
				<groupId>org.mybatis.spring.boot</groupId>
				<artifactId>mybatis-spring-boot-starter</artifactId>
				<version>2.0.0</version>
			</dependency>
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>5.1.37</version>
			</dependency>
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>druid-spring-boot-starter</artifactId>
				<version>1.1.10</version>
			</dependency>
			<dependency>
				<groupId>org.projectlombok</groupId>
				<artifactId>lombok</artifactId>
				<optional>true</optional>
			</dependency>
		</dependencies>

	</project>
	~~~
	
3. 编写YML
	~~~yml
	server:
	  port: 2002

	spring:
	  application:
		name: seata-storage-service
	  cloud:
		alibaba:
		  seata:
			#自定义事务组名称需要与seata-server中的对应
			tx-service-group: clover_tx_group
		nacos:
		  discovery:
			server-addr: localhost:8848
	  datasource:
		driver-class-name: com.mysql.jdbc.Driver
		url: jdbc:mysql://localhost:3306/seata_storage
		username: root
		password: xn123456
		
	feign:
  		hystrix:
    		enabled: false
			
	logging:
	  level:
		io:
		  seata: info

	mybatis:
	  mapperLocations: classpath:mapper/*.xml
	~~~
	
4. 创建file.conf文件
	~~~conf
	transport {
	  # tcp udt unix-domain-socket
	  type = "TCP"
	  #NIO NATIVE
	  server = "NIO"
	  #enable heartbeat
	  heartbeat = true
	  #thread factory for netty
	  thread-factory {
		boss-thread-prefix = "NettyBoss"
		worker-thread-prefix = "NettyServerNIOWorker"
		server-executor-thread-prefix = "NettyServerBizHandler"
		share-boss-worker = false
		client-selector-thread-prefix = "NettyClientSelector"
		client-selector-thread-size = 1
		client-worker-thread-prefix = "NettyClientWorkerThread"
		# netty boss thread size,will not be used for UDT
		boss-thread-size = 1
		#auto default pin or 8
		worker-thread-size = 8
	  }
	  shutdown {
		# when destroy server, wait seconds
		wait = 3
	  }
	  serialization = "seata"
	  compressor = "none"
	}

	service {
	  #vgroup->rgroup
	  vgroup_mapping.clover_tx_group = "default"
	  #only support single node
	  default.grouplist = "127.0.0.1:8091"
	  #degrade current not support
	  enableDegrade = false
	  #disable
	  disable = false
	  #unit ms,s,m,h,d represents milliseconds, seconds, minutes, hours, days, default permanent
	  max.commit.retry.timeout = "-1"
	  max.rollback.retry.timeout = "-1"
	  disableGlobalTransaction = false
	}

	client {
	  async.commit.buffer.limit = 10000
	  lock {
		retry.internal = 10
		retry.times = 30
	  }
	  report.retry.count = 5
	  tm.commit.retry.count = 1
	  tm.rollback.retry.count = 1
	}

	transaction {
	  undo.data.validation = true
	  undo.log.serialization = "jackson"
	  undo.log.save.days = 7
	  #schedule delete expired undo_log in milliseconds
	  undo.log.delete.period = 86400000
	  undo.log.table = "undo_log"
	}

	support {
	  ## spring
	  spring {
		# auto proxy the DataSource bean
		datasource.autoproxy = false
	  }
	}
	~~~
	
5. 创建registry.conf文件
	~~~conf
	registry {
	  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
	  type = "nacos"

	  nacos {
		serverAddr = "localhost:8848"
		namespace = ""
		cluster = "default"
	  }
	  eureka {
		serviceUrl = "http://localhost:8761/eureka"
		application = "default"
		weight = "1"
	  }
	  redis {
		serverAddr = "localhost:6379"
		db = "0"
	  }
	  zk {
		cluster = "default"
		serverAddr = "127.0.0.1:2181"
		session.timeout = 6000
		connect.timeout = 2000
	  }
	  consul {
		cluster = "default"
		serverAddr = "127.0.0.1:8500"
	  }
	  etcd3 {
		cluster = "default"
		serverAddr = "http://localhost:2379"
	  }
	  sofa {
		serverAddr = "127.0.0.1:9603"
		application = "default"
		region = "DEFAULT_ZONE"
		datacenter = "DefaultDataCenter"
		cluster = "default"
		group = "SEATA_GROUP"
		addressWaitTime = "3000"
	  }
	  file {
		name = "file.conf"
	  }
	}

	config {
	  # file、nacos 、apollo、zk、consul、etcd3
	  type = "file"

	  nacos {
		serverAddr = "localhost"
		namespace = ""
	  }
	  consul {
		serverAddr = "127.0.0.1:8500"
	  }
	  apollo {
		app.id = "seata-server"
		apollo.meta = "http://192.168.1.204:8801"
	  }
	  zk {
		serverAddr = "127.0.0.1:2181"
		session.timeout = 6000
		connect.timeout = 2000
	  }
	  etcd3 {
		serverAddr = "http://localhost:2379"
	  }
	  file {
		name = "file.conf"
	  }
	}
	~~~
	
6. 编写domain
	~~~Java
	package com.clover.springcloud.alibaba.domain;

	import lombok.AllArgsConstructor;
	import lombok.Data;
	import lombok.NoArgsConstructor;

	@Data
	@AllArgsConstructor
	@NoArgsConstructor
	public class CommonResult<T> {
		private Integer code;
		private String  message;
		private T       data;

		public CommonResult(Integer code, String message)
		{
			this(code,message,null);
		}
	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.domain;

	import lombok.AllArgsConstructor;
	import lombok.Data;
	import lombok.NoArgsConstructor;

	@Data
	@AllArgsConstructor
	@NoArgsConstructor
	public class Storage {
		private Long id;

		/**
		 * 产品id
		 */
		private Long productId;

		/**
		 * 总库存
		 */
		private Integer total;

		/**
		 * 已用库存
		 */
		private Integer used;

		/**
		 * 剩余库存
		 */
		private Integer residue;
	}
	~~~
	
7. Dao接口及实现
	~~~Java
	package com.clover.springcloud.alibaba.dao;

	import org.apache.ibatis.annotations.Mapper;
	import org.apache.ibatis.annotations.Param;

	@Mapper
	public interface StorageDao {

		void decrease(@Param("productId") Long productId,@Param("count") Integer count);
	}
	~~~
	
	- resources文件夹下新建mapper文件夹后添加
	
	~~~Java
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

	<mapper namespace="com.clover.springcloud.alibaba.dao.StorageDao">

		<resultMap id="BaseResultMap" type="com.clover.springcloud.alibaba.domain.Storage">
			<id column="id" property="id" jdbcType="BIGINT"/>
			<result column="product_id" property="productId" jdbcType="BIGINT"/>
			<result column="total" property="total" jdbcType="INTEGER"/>
			<result column="used" property="used" jdbcType="INTEGER"/>
			<result column="residue" property="residue" jdbcType="INTEGER"/>
		</resultMap>

		<update id="decrease">
			update t_storage
			set used = used + #{count},residue = residue - #{count}
			where product_id = #{productId}
		</update>
	</mapper>
	~~~
	
8. Service接口及实现
	~~~Java
	package com.clover.springcloud.alibaba.service;

	public interface StorageService {
		//扣减库存
		public void decrease(Long productId, Integer count);
	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.service.Impl;

	import com.clover.springcloud.alibaba.dao.StorageDao;
	import com.clover.springcloud.alibaba.service.StorageService;
	import lombok.extern.slf4j.Slf4j;
	import org.springframework.stereotype.Service;

	import javax.annotation.Resource;

	@Service
	@Slf4j
	public class StorageServiceImpl implements StorageService {

		@Resource
		private StorageDao storageDao;

		@Override
		public void decrease(Long productId, Integer count) {
			log.info("------->storage-service中扣减库存开始");
			storageDao.decrease(productId,count);
			log.info("------->storage-service中扣减库存结束");
		}
	}
	~~~

9. 编写Controller
	~~~Java
	package com.clover.springcloud.alibaba.controller;

	import com.clover.springcloud.alibaba.domain.CommonResult;
	import com.clover.springcloud.alibaba.service.StorageService;
	import org.springframework.web.bind.annotation.GetMapping;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.RestController;

	import javax.annotation.Resource;

	@RestController
	public class StorageController {
		@Resource
		private StorageService storageService;

		@RequestMapping("/storage/decrease")
		public CommonResult decrease(Long productId,Integer count)
		{
			storageService.decrease(productId, count);
			return new CommonResult(200,"扣减库存成功");
		}
	}
	~~~
	
10. Config配置
	~~~Java
	package com.clover.springcloud.alibaba.config;

	import com.alibaba.druid.pool.DruidDataSource;
	import io.seata.rm.datasource.DataSourceProxy;
	import org.apache.ibatis.session.SqlSessionFactory;
	import org.mybatis.spring.SqlSessionFactoryBean;
	import org.mybatis.spring.transaction.SpringManagedTransactionFactory;
	import org.springframework.beans.factory.annotation.Value;
	import org.springframework.boot.context.properties.ConfigurationProperties;
	import org.springframework.context.annotation.Bean;
	import org.springframework.context.annotation.Configuration;
	import org.springframework.core.io.support.PathMatchingResourcePatternResolver;

	import javax.sql.DataSource;

	@Configuration
	public class DataSourceProxyConfig {

		@Value("${mybatis.mapperLocations}")
		private String mapperLocations;

		@Bean
		@ConfigurationProperties(prefix = "spring.datasource")
		public DataSource druidDataSource(){
			return new DruidDataSource();
		}

		@Bean
		public DataSourceProxy dataSourceProxy(DataSource dataSource) {
			return new DataSourceProxy(dataSource);
		}

		@Bean
		public SqlSessionFactory sqlSessionFactoryBean(DataSourceProxy dataSourceProxy) throws Exception {
			SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
			sqlSessionFactoryBean.setDataSource(dataSourceProxy);
			sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources(mapperLocations));
			sqlSessionFactoryBean.setTransactionFactory(new SpringManagedTransactionFactory());
			return sqlSessionFactoryBean.getObject();
		}
	}
	~~~

	~~~Java
	package com.clover.springcloud.alibaba.config;

	import org.mybatis.spring.annotation.MapperScan;
	import org.springframework.context.annotation.Configuration;

	@Configuration
	@MapperScan({"com.clover.springcloud.alibaba.dao"})
	public class MyBatisConfig {
	}
	~~~

11. 主启动类
	~~~Java
	package com.clover.springcloud.alibaba;

	import org.springframework.boot.SpringApplication;
	import org.springframework.boot.autoconfigure.SpringBootApplication;
	import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
	import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
	import org.springframework.cloud.openfeign.EnableFeignClients;

	@EnableFeignClients
	@EnableDiscoveryClient
	@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
	public class SeataStorageServiceApplication2002 {
		public static void main(String[] args)
		{
			SpringApplication.run(SeataStorageServiceApplication2002.class,args);
		}
	}
	~~~

#### 18.5.4、新建账户Account-Module
1. 新建seata-account-service2003
2. 修改POM
	~~~pom
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

		<artifactId>seata-account-service2003</artifactId>


		<dependencies>
			<!--nacos-->
			<dependency>
				<groupId>com.alibaba.cloud</groupId>
				<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
			</dependency>
			<!--seata-->
			<dependency>
				<groupId>com.alibaba.cloud</groupId>
				<artifactId>spring-cloud-starter-alibaba-seata</artifactId>
				<exclusions>
					<exclusion>
						<artifactId>seata-all</artifactId>
						<groupId>io.seata</groupId>
					</exclusion>
				</exclusions>
			</dependency>
			<dependency>
				<groupId>io.seata</groupId>
				<artifactId>seata-all</artifactId>
				<version>0.9.0</version>
			</dependency>
			<!--feign-->
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-starter-openfeign</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-web</artifactId>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-test</artifactId>
				<scope>test</scope>
			</dependency>
			<dependency>
				<groupId>org.mybatis.spring.boot</groupId>
				<artifactId>mybatis-spring-boot-starter</artifactId>
				<version>2.0.0</version>
			</dependency>
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>5.1.37</version>
			</dependency>
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>druid-spring-boot-starter</artifactId>
				<version>1.1.10</version>
			</dependency>
			<dependency>
				<groupId>org.projectlombok</groupId>
				<artifactId>lombok</artifactId>
				<optional>true</optional>
			</dependency>
		</dependencies>

	</project>
	~~~
	
3. 编写YML
	~~~yml
	server:
	  port: 2003

	spring:
	  application:
		name: seata-account-service
	  cloud:
		alibaba:
		  seata:
			tx-service-group: clover_tx_group
		nacos:
		  discovery:
			server-addr: localhost:8848
	  datasource:
		driver-class-name: com.mysql.jdbc.Driver
		url: jdbc:mysql://localhost:3306/seata_account
		username: root
		password: xn123456

	feign:
	  hystrix:
		enabled: false

	logging:
	  level:
		io:
		  seata: info

	mybatis:
	  mapperLocations: classpath:mapper/*.xml
	~~~
	
4. 创建file.conf文件
	~~~conf
	transport {
	  # tcp udt unix-domain-socket
	  type = "TCP"
	  #NIO NATIVE
	  server = "NIO"
	  #enable heartbeat
	  heartbeat = true
	  #thread factory for netty
	  thread-factory {
		boss-thread-prefix = "NettyBoss"
		worker-thread-prefix = "NettyServerNIOWorker"
		server-executor-thread-prefix = "NettyServerBizHandler"
		share-boss-worker = false
		client-selector-thread-prefix = "NettyClientSelector"
		client-selector-thread-size = 1
		client-worker-thread-prefix = "NettyClientWorkerThread"
		# netty boss thread size,will not be used for UDT
		boss-thread-size = 1
		#auto default pin or 8
		worker-thread-size = 8
	  }
	  shutdown {
		# when destroy server, wait seconds
		wait = 3
	  }
	  serialization = "seata"
	  compressor = "none"
	}

	service {

	  vgroup_mapping.clover_tx_group = "default" #修改自定义事务组名称

	  default.grouplist = "127.0.0.1:8091"
	  enableDegrade = false
	  disable = false
	  max.commit.retry.timeout = "-1"
	  max.rollback.retry.timeout = "-1"
	  disableGlobalTransaction = false
	}


	client {
	  async.commit.buffer.limit = 10000
	  lock {
		retry.internal = 10
		retry.times = 30
	  }
	  report.retry.count = 5
	  tm.commit.retry.count = 1
	  tm.rollback.retry.count = 1
	}

	## transaction log store
	store {
	  ## store mode: file、db
	  mode = "db"

	  ## file store
	  file {
		dir = "sessionStore"

		# branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
		max-branch-session-size = 16384
		# globe session size , if exceeded throws exceptions
		max-global-session-size = 512
		# file buffer size , if exceeded allocate new buffer
		file-write-buffer-cache-size = 16384
		# when recover batch read size
		session.reload.read_size = 100
		# async, sync
		flush-disk-mode = async
	  }

	  ## database store
	  db {
		## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
		datasource = "dbcp"
		## mysql/oracle/h2/oceanbase etc.
		db-type = "mysql"
		driver-class-name = "com.mysql.jdbc.Driver"
		url = "jdbc:mysql://127.0.0.1:3306/seata"
		user = "root"
		password = "xn123456"
		min-conn = 1
		max-conn = 3
		global.table = "global_table"
		branch.table = "branch_table"
		lock-table = "lock_table"
		query-limit = 100
	  }
	}
	lock {
	  ## the lock store mode: local、remote
	  mode = "remote"

	  local {
		## store locks in user's database
	  }

	  remote {
		## store locks in the seata's server
	  }
	}
	recovery {
	  #schedule committing retry period in milliseconds
	  committing-retry-period = 1000
	  #schedule asyn committing retry period in milliseconds
	  asyn-committing-retry-period = 1000
	  #schedule rollbacking retry period in milliseconds
	  rollbacking-retry-period = 1000
	  #schedule timeout retry period in milliseconds
	  timeout-retry-period = 1000
	}

	transaction {
	  undo.data.validation = true
	  undo.log.serialization = "jackson"
	  undo.log.save.days = 7
	  #schedule delete expired undo_log in milliseconds
	  undo.log.delete.period = 86400000
	  undo.log.table = "undo_log"
	}

	## metrics settings
	metrics {
	  enabled = false
	  registry-type = "compact"
	  # multi exporters use comma divided
	  exporter-list = "prometheus"
	  exporter-prometheus-port = 9898
	}

	support {
	  ## spring
	  spring {
		# auto proxy the DataSource bean
		datasource.autoproxy = false
	  }
	}
	~~~
	
5. 创建registry.conf文件
	~~~conf
	registry {
	  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
	  type = "nacos"

	  nacos {
		serverAddr = "localhost:8848"
		namespace = ""
		cluster = "default"
	  }
	  eureka {
		serviceUrl = "http://localhost:8761/eureka"
		application = "default"
		weight = "1"
	  }
	  redis {
		serverAddr = "localhost:6379"
		db = "0"
	  }
	  zk {
		cluster = "default"
		serverAddr = "127.0.0.1:2181"
		session.timeout = 6000
		connect.timeout = 2000
	  }
	  consul {
		cluster = "default"
		serverAddr = "127.0.0.1:8500"
	  }
	  etcd3 {
		cluster = "default"
		serverAddr = "http://localhost:2379"
	  }
	  sofa {
		serverAddr = "127.0.0.1:9603"
		application = "default"
		region = "DEFAULT_ZONE"
		datacenter = "DefaultDataCenter"
		cluster = "default"
		group = "SEATA_GROUP"
		addressWaitTime = "3000"
	  }
	  file {
		name = "file.conf"
	  }
	}

	config {
	  # file、nacos 、apollo、zk、consul、etcd3
	  type = "file"

	  nacos {
		serverAddr = "localhost"
		namespace = ""
	  }
	  consul {
		serverAddr = "127.0.0.1:8500"
	  }
	  apollo {
		app.id = "seata-server"
		apollo.meta = "http://192.168.1.204:8801"
	  }
	  zk {
		serverAddr = "127.0.0.1:2181"
		session.timeout = 6000
		connect.timeout = 2000
	  }
	  etcd3 {
		serverAddr = "http://localhost:2379"
	  }
	  file {
		name = "file.conf"
	  }
	}
	~~~
	
6. 编写domain
	~~~Java
	package com.clover.springcloud.alibaba.domain;

	import lombok.AllArgsConstructor;
	import lombok.Data;
	import lombok.NoArgsConstructor;

	@Data
	@AllArgsConstructor
	@NoArgsConstructor
	public class CommonResult<T> {
		private Integer code;
		private String  message;
		private T       data;

		public CommonResult(Integer code, String message)
		{
			this(code,message,null);
		}
	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.domain;

	import lombok.AllArgsConstructor;
	import lombok.Data;
	import lombok.NoArgsConstructor;

	import java.math.BigDecimal;

	@Data
	@AllArgsConstructor
	@NoArgsConstructor
	public class Account {
		private Long id;

		/**
		 * 用户id
		 */
		private Long userId;

		/**
		 * 总额度
		 */
		private BigDecimal total;

		/**
		 * 已用额度
		 */
		private BigDecimal used;

		/**
		 * 剩余额度
		 */
		private BigDecimal residue;
	}
	~~~

7. Dao接口及实现
	~~~Java
	package com.clover.springcloud.alibaba.dao;

	import org.apache.ibatis.annotations.Mapper;
	import org.apache.ibatis.annotations.Param;

	import java.math.BigDecimal;

	@Mapper
	public interface AccountDao {
		void decrease(@Param("userId") Long userId, @Param("money") BigDecimal money);
	}
	~~~
	
	~~~Java
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

	<mapper namespace="com.clover.springcloud.alibaba.dao.AccountDao">

		<resultMap id="BaseResultMap" type="com.clover.springcloud.alibaba.domain.Account">
			<id column="id" property="id" jdbcType="BIGINT"/>
			<result column="user_id" property="userId" jdbcType="BIGINT"/>
			<result column="total" property="total" jdbcType="DECIMAL"/>
			<result column="used" property="used" jdbcType="DECIMAL"/>
			<result column="residue" property="residue" jdbcType="DECIMAL"/>
		</resultMap>

		<update id="decrease">
			UPDATE t_account
			SET
			  residue = residue - #{money},used = used + #{money}
			WHERE
			  user_id = #{userId};
		</update>

	</mapper>
	~~~
	
8. Service接口及实现
	~~~Java
	package com.clover.springcloud.alibaba.service;

	import org.springframework.web.bind.annotation.RequestParam;

	import java.math.BigDecimal;

	public interface AccountService {
		/**
		 * 扣减账户余额
		 * @param userId 用户id
		 * @param money 金额
		 */
		void decrease(@RequestParam("userId") Long userId, @RequestParam("money") BigDecimal money);
	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.service.Impl;

	import com.clover.springcloud.alibaba.dao.AccountDao;
	import com.clover.springcloud.alibaba.service.AccountService;
	import lombok.extern.slf4j.Slf4j;
	import org.springframework.stereotype.Service;

	import javax.annotation.Resource;
	import java.math.BigDecimal;

	@Service
	@Slf4j
	public class AccountServiceImpl implements AccountService{
		@Resource
		private AccountDao accountDao;

		public void decrease(Long userId, BigDecimal money)
		{
			log.info("------->account-service中扣减账户余额开始");
			accountDao.decrease(userId, money);
			log.info("------->account-service中扣减账户余额结束");
		}
	}
	~~~

9. 编写Controller
	~~~Java
	package com.clover.springcloud.alibaba.controller;

	import com.clover.springcloud.alibaba.domain.CommonResult;
	import com.clover.springcloud.alibaba.service.AccountService;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.RequestParam;
	import org.springframework.web.bind.annotation.RestController;

	import javax.annotation.Resource;
	import java.math.BigDecimal;

	@RestController
	public class AccountController {

		@Resource
		private AccountService accountService;

		@RequestMapping("/account/decrease")
		public CommonResult decrease(@RequestParam("userId") Long userId, @RequestParam("money") BigDecimal money)
		{
			accountService.decrease(userId, money);
			return new CommonResult(200,"扣减账户余额成功");
		}
	}
	~~~
	
10. Config配置
	~~~Java
	package com.clover.springcloud.alibaba.config;

	import com.alibaba.druid.pool.DruidDataSource;
	import io.seata.rm.datasource.DataSourceProxy;
	import org.apache.ibatis.session.SqlSessionFactory;
	import org.mybatis.spring.SqlSessionFactoryBean;
	import org.mybatis.spring.transaction.SpringManagedTransactionFactory;
	import org.springframework.beans.factory.annotation.Value;
	import org.springframework.boot.context.properties.ConfigurationProperties;
	import org.springframework.context.annotation.Bean;
	import org.springframework.context.annotation.Configuration;
	import org.springframework.core.io.support.PathMatchingResourcePatternResolver;

	import javax.sql.DataSource;

	//使用Seata对数据源进行代理
	@Configuration
	public class DataSourceProxyConfig {
		@Value("${mybatis.mapperLocations}")
		private String mapperLocations;

		@Bean
		@ConfigurationProperties(prefix = "spring.datasource")
		public DataSource druidDataSource(){
			return new DruidDataSource();
		}

		@Bean
		public DataSourceProxy dataSourceProxy(DataSource dataSource) {
			return new DataSourceProxy(dataSource);
		}

		@Bean
		public SqlSessionFactory sqlSessionFactoryBean(DataSourceProxy dataSourceProxy) throws Exception {
			SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
			sqlSessionFactoryBean.setDataSource(dataSourceProxy);
			sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources(mapperLocations));
			sqlSessionFactoryBean.setTransactionFactory(new SpringManagedTransactionFactory());
			return sqlSessionFactoryBean.getObject();
		}

	}
	~~~
	
	~~~Java
	package com.clover.springcloud.alibaba.config;

	import org.mybatis.spring.annotation.MapperScan;
	import org.springframework.context.annotation.Configuration;

	@Configuration
	@MapperScan({"com.clover.springcloud.alibaba.dao"})
	public class MyBatisConfig {
	}
	~~~
	
11. 主启动类
	~~~Java
	package com.clover.springcloud.alibaba;

	import org.springframework.boot.SpringApplication;
	import org.springframework.boot.autoconfigure.SpringBootApplication;
	import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
	import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
	import org.springframework.cloud.openfeign.EnableFeignClients;

	@EnableDiscoveryClient
	@EnableFeignClients
	@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)//取消数据源的自动创建
	public class SeataAccountMainApp2003 {
		public static void main(String[] args)
		{
			SpringApplication.run(SeataAccountMainApp2003.class,args);
		}
	}
	~~~
	
	
### 18.6、Test

#### 18.6.1、正常下单

http://localhost:2001/order/create?userId=1&productId=1&count=10&money=100

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027210906.png)


#### 18.6.2、超时异常，没加@GlobalTransactional

1. AccountServiceImpl添加超时
2. 数据库情况
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027211108.png)
	
3. 故障情况
	1. 当库存和账户金额扣减后，订单状态并没有设置为已经完成，没有从零改为1
	2. 而且由于feign的重试机制，账户余额还有可能被多次扣减


#### 18.6.3、超时异常，添加@GlobalTransactional

1. AccountServiceImpl添加超时
2. OrderServiceImpl添加`@GlobalTransactional`
	- **@GlobalTransactional(name = "clover-create-order",rollbackFor = Exception.class)**
3. 下单后数据库数据并没有任何改变
	- 记录都添加不进来


### 18.7、一部分补充

1. Seata
	- 2019年1月份蚂蚁金服和阿里巴巴共同开源的分布式事务解决方案
	- Simple Extensible Autonomous Transaction Architecture，简单可扩展自治事务框架
	- 2020起始，参加工作后用1.0以后的版本
2. 再看TC/TM/RM三大组件
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027214516.png)
	
	- 分布式事务的执行流程
		- TM 开启分布式事务（TM 向 TC 注册全局事务记录）
		- 按业务场景，编排数据库、服务等事务内资源（RM 向 TC 汇报资源准备状态 ）
		- TM 结束分布式事务，事务一阶段结束（TM 通知 TC 提交/回滚分布式事务）
		- TC 汇总事务信息，决定分布式事务是提交还是回滚
		- TC 通知所有 RM 提交/回滚 资源，事务二阶段结束
	
3. AT模式如何做到对业务的无侵入
	- 是什么
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027214824.png)
		
	- 一阶段加载
		- 在`一阶段`，Seata 会拦截“业务 SQL”
			1. 解析 SQL 语义，找到“业务 SQL”要更新的业务数据，在业务数据被更新前，将其保存成**before image**
			2. 执行“业务 SQL”更新业务数据，在业务数据更新之后
			3. 其保存成**after image**，**最后生成行锁**
		- 以上操作全部在一个数据库事务内完成，这样保证了`一阶段`操作的原子性
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027215024.png)
		
	- 二阶段提交
		- 二阶段如是顺利提交的话；因为“业务 SQL”在一阶段已经提交至数据库，所以Seata框架只需将**一阶段保存的快照数据和行锁删掉，完成数据清理即可**
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027215111.png)
		
	- 二阶段回滚
		- 二阶段如果是回滚的话，Seata 就需要回滚一阶段已经执行的“业务 SQL”，还原业务数据
		- 回滚方式便是用**before image**还原业务数据；但在还原前要首先要**校验脏写**，**对比数据库当前业务数据**和 **after image**
		- 如果两份数据完全一致就说明没有脏写，可以还原业务数据，如果不一致就说明有脏写，出现脏写就需要转人工处理
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027215223.png)
		
4. debug
5. 补充
	
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211027214628.png)
