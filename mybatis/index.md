# Mybatis



# 1、简介

## 1.1、什么是Mybatis





## 1.2、持久化（持久化是一个动作）



**数据持久化**

- **持久化就是将程序的数据在持久状态和瞬时状态转化的过程**
- 内存：**断电即失**
- 数据库（jdbc），io文件持久化。

**为什么需要持久化**有一些对象，不能让他丢掉。用时再取

对象：如支付宝里面的钱不能丢失以及页面的一些信息

因为内存有一个特点是**断电即失**

- 另外一个原因也是内存太贵（外在原因）



## 1.3、持久层（是一个概念）

Dao层，Service层，Controller层...等每一个层都有它们自己要做的事情

- 完成持久化工作的代码块
- 层是界限十分明显



## 1.4、为什么需要Mybatis

- 帮助程序员将数据存入到数据库中
- 方便
- 传统的JDBC代码太复杂了。所以将其简化，出现一个框架
- 不用Mybatis也可以。更容易上手。



# 2、第一个Mybatis程序

思路：搭建环境-->导入Mybatis-->编写代码-->测试！

## 2.1、搭建环境

~~~xshell
CREATE DATABASE `mybatis`;

USE `mybatis`;

CREATE TABLE `user`(
  `id` INT(20) NOT NULL PRIMARY KEY, # 设置id，并设为主键且不为空
  `name` VARCHAR(30) DEFAULT NULL,   # 设置名字，默认为空
  `pwd` VARCHAR(30) DEFAULT NULL
)ENGINE=INNODB DEFAULT CHARSET=utf8; # 设置引擎为INNODB 默认字符集为utf8

INSERT INTO `user`(`id`,`name`,`pwd`) VALUES
(1,'科比','123456'),
(2,'哈登','123456'),
(3,'威少','123456')
~~~



新建项目

	1.新建一个普通的maven项目

	2.删除src目录

	3.导入maven依赖

~~~xml
<dependencies>
        <!--mysql驱动-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
~~~



## 2.2、创建一个模块 

- 编写mybatis的核心配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration核心配置文件-->
<configuration>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3308/mybatis?useSSL=true&amp;userUnicode=true&amp;characterEncoding=UTF-8"/>
                <!--此处的&amp;相当于yml里面的&&,只是在xml文件中得进行转化-->
                <property name="username" value="root"/>
                <property name="password" value="xn123456"/>
            </dataSource>
        </environment>
    </environments>
    
</configuration>
~~~



- 编写mybatis工具类

~~~Java
package com.kuang.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {

    //提升作用域
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            //使用Mybatis第一步 获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
    // SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。

    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
~~~



## 2.3、编写代码

- 实体类

~~~Java
package com.kuang.pojo;

//实体类
public class User {
    private int id;
    private String name;
    private String pwd;

    public User() {

    }

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}

~~~



- Dao接口

~~~Java
public interface UserDao {
    List<User> getUserList();
}
~~~



- 接口实现类（由原来的Use人DaoImpl转变为一个Mapper配置文件）

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace等于绑定一个对于的Dao/Mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">

    <!--select查询语句 id对应的是我们的方法名字-->
    <select id="getUserList" resultType="com.kuang.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
~~~



**注意点：在使用resultType返回结果时，返回集合中的泛型，不要写集合**



## 2.4、测试

注意点：org.apache.ibatis.binding.BindingException: Type interface com.kuang.dao.UserDao is not known to the MapperRegistry.

**MapperRegistry是什么？**

核心配置文件中注册mappers

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165209.png)



~~~xml
<build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
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



- junit测试

~~~Java
package com.kuang.dao;

import com.kuang.pojo.User;
import com.kuang.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class UserDaoTest {

    @Test
    public void test(){

        //第一步：活得SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //方式一：getMapper
        UserDao userDao = sqlSession.getMapper(UserDao.class);

        List<User> userList = userDao.getUserList();

        for (User user : userList) {
            System.out.println(user);
        }

        //关闭sqlsession
        sqlSession.close();
    }
}

~~~

官网推荐的是我们将close放入finally中，自己手动添加一个try-catch语句

可能遇到的问题：

1. 配置文件没有注册

2. 绑定接口错误  namespace绑定问题

3. 方法名不对

4. 返回类型不对 resulttype返回类型

5. Maven导出资源问题

思路：

- 导入最基本需要的依赖包
- 创建一个工具类，构建SqlSessionFactory
- 编写核心配置文件
  - 连接数据库，修改里面的属性
- 写完实体类后写接口，最后写实体类，用mapper.xml代替了原来的Impl



每次收到的HTTP请求（也就是连接我们的数据库），就可以打开一个SqlSession，返回一个响应，就关闭它。



# 3、CRUD


## 3.1、namespace

namespace中的包名要和Dao/mapper接口的包名一致！

## 3.2、select

选择，查询语句

- id：就是对应的namespace中的方法名
- resultType：SQL语句执行的返回值
- parameterType：参数类型！



1.编写接口

~~~Java
//根据ID查询用户
    User getUserById(int id);
~~~

2.编写对应的mapper中的sql语句

~~~xml
<select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
      select * from mybatis.user where id = #{id}
</select>
~~~

3.测试

~~~Java
public void getUserById(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        User user = mapper.getUserById(1);

        System.out.println(user);

        //关闭sqlsession
        sqlSession.close();
    }
~~~



## 3.3、Insert

~~~xml
<!--对象中的属性可以直接提取出来-->
<insert id="addUser" parameterType="com.kuang.pojo.User">
        insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd})
</insert>
~~~



## 3.4、Update

~~~xml
<update id="updateUser" parameterType="com.kuang.pojo.User">
        update mybatis.user set name=#{name},pwd=#{pwd}  where id=#{id};
</update>
~~~



## 3.5、delete

~~~xml
<delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id=#{id};
</delete>
~~~



注意点：

- 增删改需要提交事务，否则添加不进去（代码执行没问题，数据库里面不显示）
- resource绑定mapper，需要使用路径



## 3.6、万能Map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们应当考虑使用Map！

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165256.png)



Map传递参数，直接在sql中取出即可！【parameterType="Map"】

对象传递参数，直接在sql中取对象的属性即可！【parameterType="Object"】

只有一个基本类型参数的情况下，可以直接在sql中取到！

多个参数用Map，**或者注解！**



## 3.7、思考题

模糊查询 

​	1.Java代码执行的时候，传递通配符% %

~~~Java
List<User> userList = mapper.getUserLike("%李%");
~~~

​	2.在sql拼接中使用通配符！

~~~xml
select * from mybatis.user where name like "%"#{value}"%"
~~~



#  4、配置解析


## 4.1、核心配置文件

- mybatis-config.xml
- MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。

~~~xml
configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
environment（环境变量）
transactionManager（事务管理器）
dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
~~~



## 4.2、环境配置（environment）

Mybatis可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

transactionManager：事务管理器，分为JDBC与MANAGED

dataSource：数据源，分为UNPOOLED、POOLED、JNDI；最重要的概念就是：**池**：拥有回收机制，即下次可以继续执行

Mybatis默认的事务管理器就是JDBC，连接池：POOLED



## 4.3、属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。【db.properties】

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165315.png)

编写一个配置文件：db.properties

~~~properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3308/mybatis?useSSL=true&userUnicode=true&characterEncoding=UTF-8
username=root
password=xn123456
~~~



在核心配置文件中引入

~~~xml
<!--引入外部配置文件-->
<properties resource="db.properties">
   <property name="username" value="root"/>
   <property name="password" value="xn123456"/>
</properties>
~~~



- 可以直接引入外部文件
- 可以在其中增加一些属性配置
- 如果两个文件有同一个字段，优先使用外部配置文件的！



## 4.4、类型别名（typeAliases）

- 类型别名可为 Java 类型设置一个缩写名字。
- 意在降低冗余的全限定类名书写。

~~~xml
<!--可以给实体类起别名-->
<typeAliases>
    <typeAlias type="com.kuang.pojo.User" alias="User"/>
</typeAliases>
~~~



也可以指定一个包名，Mybatis会在包名下面搜索需要的Java Bean

扫描实体类的包，它的默认别名就为这个类的类名首字母小写

~~~xml
<!--可以给实体类起别名-->
    <typeAliases>
        <package name="com.kuang.pojo"/>
    </typeAliases>
~~~



在实体类比较少的时候，使用第一种方式

如果实体类十分多，建议使用第二种

第一种可以DIY别名，第二种则不行，如果非要使用别名，**需要在实体类上增加注解，在实体类上注解的同时，也需要在核心配置文件中扫描指定的包。**

~~~Java
@Alias("user")
public class User {
~~~



## 4.5、设置

- 这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165330.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165347.png)



## 4.6、其它配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- plugins（插件]
  - mybatis-generator-core
  - mybatis-plus
  - 通用mapper



## 4.7、映射器（mappers）

MapperRegistry：绑定注册我们的Mapper文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165405.png)



方式一：【推荐使用】

~~~xml
<mappers>
    <mapper resource="com/kuang/dao/UserMapper.xml"/>
</mappers>
~~~



方式二：使用class文件绑定注册

~~~xml
<mappers>
    <mapper class="com.kuang.dao.UserMapper"/>
</mappers>
~~~

注意点：

- 接口和它的mapper配置文件必须同名
- 接口和它的Mapper配置文件必须在同一个包下！



方式三：使用扫描包进行注入绑定

~~~xml
<mappers>
    <mapper package="com.kuang.dao"/>
</mappers>
~~~

注意点：

- 接口和它的mapper配置文件必须同名
- 接口和它的Mapper配置文件必须在同一个包下！



## 4.8、生命周期和作用域

生命周期和作用域是至关重要的，因为错误的使用会导致非常严重的**并发问题**。

**SqlSessionFactoryBuilder：**

- 一旦创建了 SqlSessionFactory，就不再需要它了
- 它是一个局部变量

**SqlSessionFactory：**

- 说白了就是可以想象为：数据库连接池
- SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例**
- 因此SqlSessionFactory的最佳作用域是应用作用域
- 最简单的就是使用**单例模式**或者静态单例模式。使其保证全局唯一

**SqlSession：**

- 连接到连接池的一个请求！
- SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。
- 用完之后需要赶紧关闭，否则资源被占用！



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165423.png)



这里面的每一个Mapper，就代表一个具体的业务！



# 5、解决属性名和字段名不一致的问题



## 5.1、问题

数据库中的字段

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165438.png)



新建一个项目，拷贝之前的，测试实体类字段不一致的情况

~~~Java
public class User {
    private int id;
    private String name;
    private String password;
}
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165449.png)

~~~xml
//	select * from mybatis.user where id = #{id}
//类型处理器
//	select id,name,pwd from mybatis.user where id = #{id}
~~~

解决方法：

- 起别名

~~~xml
<select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
     select id,name,pwd as password from mybatis.user where id = #{id}
</select>
~~~

## 5.2、resultMap

结果集映射

~~~xlm
<!--结果集映射：id就是你下面resultMap的值；type就是结果集要映射成谁就是什么类型-->
<resultMap id="UserMap" type="User">
    <!--column数据库中的字段，property实体类中的属性-->
    <result column="id" property="id"/>
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>

<select id="getUserById" parameterType="int" resultMap="UserMap" >
    select * from mybatis.user where id = #{id}
</select>
~~~



- `resultMap` 元素是 MyBatis 中最重要最强大的元素
- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。
- ResultMap最优秀的地方在于，虽然你已经对它相当了解了，但是根本iju不需要显示地用到它们。



# 6、日志

## 6.1、日志工厂

如果一个数据库操作，出现了异常。日志就是最好的助手

曾经：sout、debug

现在：日志工厂！

- SLF4J
- LOG4J
- LOG4J2
- JDK_LOGGING
- COMMONS_LOGGING
- STDOUT_LOGGING
- NO_LOGGING

在mybatis中具体使用哪一个日志实现，在设置中设定！

**STDOUT_LOGGING标准日志输出**

注意：**使用标准日志输出时，出现报错：1、value后面的值又空格，切记！**

在mybatis核心配置文件中，配置我们的日志

~~~xml
<settings>
	<--标准的日志工厂实现-->
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
~~~



## 6.2、Log4j

什么是log4j？

- Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件
- 我们也可以控制每一条日志的输出格式
- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程
- 通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码



1. 先导入log4j的包

   ~~~xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   
   ~~~

2. kog4j.properties

   ~~~xml
   #将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
   log4j.rootLogger=DEBUG,console,file
   
   #控制台输出的相关设置
   log4j.appender.console=org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target=System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout=org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
   
   #文件输出的相关设置
   log4j.appender.file=org.apache.log4j.RollingFileAppender
   log4j.appender.file.File=./log/kuang.log
   log4j.appender.file.MaxFileSize=10mb
   log4j.appender.file.Threshold=DEBUG
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
   
   #日志输出级别
   log4j.logger.org.mybatis=DEBUG
   log4j.logger.java.sql=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.ResultSet=DEBUG
   log4j.logger.java.sql.PrepareStatement=DEBUG
   ~~~

3. 配置log4j为日志的实现

   ~~~xml
   <settings>
   	<setting name="logImpl" value="LOG4J"/>
   </settings>
   ~~~

4. Log4j的使用，直接测试运行刚才的查询

   ~~~xml
   [org.apache.ibatis.logging.LogFactory]-Logging initialized using 'class org.apache.ibatis.logging.log4j.Log4jImpl' adapter.
   [org.apache.ibatis.logging.LogFactory]-Logging initialized using 'class org.apache.ibatis.logging.log4j.Log4jImpl' adapter.
   [org.apache.ibatis.io.VFS]-Class not found: org.jboss.vfs.VFS
   [org.apache.ibatis.io.JBoss6VFS]-JBoss 6 VFS API is not available in this environment.
   [org.apache.ibatis.io.VFS]-Class not found: org.jboss.vfs.VirtualFile
   [org.apache.ibatis.io.VFS]-VFS implementation org.apache.ibatis.io.JBoss6VFS is not valid in this environment.
   [org.apache.ibatis.io.VFS]-Using VFS adapter org.apache.ibatis.io.DefaultVFS
   [org.apache.ibatis.io.DefaultVFS]-Find JAR URL: file:/G:/IDEADocument/MyBatis-study/Mybatis-study/mybatis-04/target/classes/com/kuang/pojo
   [org.apache.ibatis.io.DefaultVFS]-Not a JAR: file:/G:/IDEADocument/MyBatis-study/Mybatis-study/mybatis-04/target/classes/com/kuang/pojo
   [org.apache.ibatis.io.DefaultVFS]-Reader entry: User.class
   [org.apache.ibatis.io.DefaultVFS]-Listing file:/G:/IDEADocument/MyBatis-study/Mybatis-study/mybatis-04/target/classes/com/kuang/pojo
   [org.apache.ibatis.io.DefaultVFS]-Find JAR URL: file:/G:/IDEADocument/MyBatis-study/Mybatis-study/mybatis-04/target/classes/com/kuang/pojo/User.class
   [org.apache.ibatis.io.DefaultVFS]-Not a JAR: file:/G:/IDEADocument/MyBatis-study/Mybatis-study/mybatis-04/target/classes/com/kuang/pojo/User.class
   [org.apache.ibatis.io.DefaultVFS]-Reader entry: ����   4 @
   [org.apache.ibatis.io.ResolverUtil]-Checking to see if class com.kuang.pojo.User matches criteria [is assignable to Object]
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
   [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Opening JDBC Connection
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-Created connection 2044903525.
   [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Setting autocommit to false on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@79e2c065]
   [com.kuang.dao.UserMapper.getUserList]-==>  Preparing: select * from mybatis.user 
   [com.kuang.dao.UserMapper.getUserList]-==> Parameters: 
   [com.kuang.dao.UserMapper.getUserList]-<==      Total: 3
   User{id=1, name='科比', pwd='123456'}
   User{id=2, name='哈登', pwd='123456'}
   User{id=3, name='威少', pwd='123456'}
   [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Resetting autocommit to true on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@79e2c065]
   [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@79e2c065]
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-Returned connection 2044903525 to pool.
   ~~~



**直接使用**

1. 在使用Log4j的类中，导入包 import org.apache.log4j.Logger;

2. 日志对象，参数为当前类的class

   ~~~xml
   static Logger logger = Logger.getLogger(UserDaoTest.class);
   ~~~

3. 日志级别

   ~~~Java
   logger.info("info:进入了testLog4j");
   logger.debug("debug:进入了testLog4j");
   logger.error("error:进入了testLog4j");
   ~~~




# 7、分页

**思考：为什么要分页？**

- 减少数据的处理量

## 7.1、使用Limit 分页

~~~xml
语法： SELECT * from user limit startIndex,pageSize;   
startIndex:起始的位置，按数组下标开始进行分页
pageSize:每一页的数量

SELECT * from user limit 3; #类似于[0,n]
~~~



使用Mybatis实现分页，核心就是SQL

1. 接口

   ```Java
   //分页
   List<User> getUserByLimit(Map<String,Integer>map);
   ```

2. Mapper.xml

   ```xml
   <select id="getUserByLimit" parameterType="map" resultType="user">
       select * from mybatis.user limit #{startIndex},#{pageSize};
   </select>
   ```

   **如果此处的pwd没有改成password，则不需要进行结果集映射，如果改了，则需要进行结果集映射，就不能使用resultType，则需要使用resultMap**

3. 测试

   ```Java
   @Test
   public void getUserByLimit(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       HashMap<String, Integer> map = new HashMap<>();
   
       map.put("startIndex",0);
       map.put("pageSize",2);
   
       List<User> userList = mapper.getUserByLimit(map);
   
       for (User user : userList) {
           System.out.println(user);
       }
   
       sqlSession.close();
   }
   ```



## 7.2、RowBounds分页

不再使用sql实现分页

1. 接口

   ```Java
   //分页2
   List<User> getUserByRowBounds();
   ```

2. mapper.xml

   ```xml
   <select id="getUserByRowBounds" resultType="user">
       select * from mybatis.user;
   </select>
   ```

3. 测试

   ```Java
   @Test
   public void getUserByRowBounds(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       //通过RowBounds实现分页
       RowBounds rowBounds = new RowBounds(1, 2);
   
       //通过Java代码层实现分页
       List<User> userList = sqlSession.selectList("com.kuang.dao.UserMapper.getUserByRowBounds",null,rowBounds);
   
       for (User user : userList) {
           System.out.println(user);
       }
   
       sqlSession.close();
   }
   ```



## 7.3、分页插件

了解即可，==Mybatis PageHelper==



# 8、使用注解开发

## 8.1、面向接口编程

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165522.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165534.png)



## 8.2、使用注解开发

1. 注解直接在接口上实现

   ```Java
   
   public interface UserMapper {
   
       @Select("select * from user")
       List<User> getUsers();
   }
   ```

2. 需要在核心配置文件中绑定接口！

   ```xml
   <!--绑定接口-->
   <mappers>
       <mapper class="com.kuang.dao.UserMapper"/>
   </mappers>
   ```

3. 测试使用

   ```Java
   @Test
   public void test(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       //底层主要应用反射
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       List<User> userList = mapper.getUsers();
   
       for (User user : userList) {
           System.out.println(user);
       }
   
       sqlSession.close();
   }
   ```



		本质：反射机制实现

		底层：动态代理！



## 8.3、注解的CRUD

我们可以在工具类创建的时候自动提交事务 

```Java
public static SqlSession getSqlSession(){
    return sqlSessionFactory.openSession(true);
}
```

编写接口，增加注解

```Java
public interface UserMapper {

    @Select("select * from user")
    List<User> getUsers();

    //@Param方法存在多个参数，所有的参数前面必须加上@Param("id")注解
    @Select("select * from user where id = #{id}")
    User getUserById(@Param("id") int id);

    @Insert("insert into user(id,name,pwd) values (#{id},#{name},#{pwd})")
    int addUser(User user);

    @Update("update user set name=#{name},pwd=#{pwd} where id = #{id}")
    int updateUser(User user);

    @Delete("delete from user where id = #{uid}")
    int deleteUser(@Param("uid") int id);
}
```



注意：我们必须要将接口注册绑定到我们的核心配置文件中



**使用Param注解时，里面对应的基本类型应当与sql语句中的类型一致，否则报错**



**关于@Param()注解**

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但是建议加上去
- 我们在SQL中引用的就是我们这里的@Param()中设置的属性名





# 9、Lombok

使用步骤：

1. 在IDEA中安装Lombok插件！

2. 在项目中导入Lombok的ｊａｒ包

   ```xml
   <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.18</version>
   </dependency>
   ```

   这是一款懒人插件，虽然简化了许多，但是却对新手造成了困惑，不容易去阅读源码了；对于这种插件，合理的利用才是关键

说明：

```xml
＠Data：无参构造，get、set、toString、hashcode、equals
@AllArgsConstructor：有参构造
@NoArgsConstructor：无参构造
```



# 10、多对一处理

多对一:

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165554.png)

- 多个学生，对应一个老师
- 对于学生这边而言，关联...多个学生，关联一个老师  【多对一】
- 对于老师这边而言，集合...一个老师，有很多学生       【一对多】



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165605.png)

SQL：

~~~sql
CREATE TABLE `teacher`(
    `id` INT(10) NOT NULL,
    `name` VARCHAR(30) DEFAULT NULL,
    PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO teacher(`id`,`name`) VALUES (1,'秦老师');

CREATE TABLE `student`(
    `id` INT(10) NOT NULL,
    `name` VARCHAR(30) DEFAULT NULL,
    `tid` INT(10) DEFAULT NULL,
    PRIMARY KEY(`id`),
    KEY `fktid`(`tid`),
    CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO `student` (`id`,`name`,`tid`) VALUES ('1','小明','1');
INSERT INTO `student` (`id`,`name`,`tid`) VALUES ('2','小红','1');
INSERT INTO `student` (`id`,`name`,`tid`) VALUES ('3','小张','1');
INSERT INTO `student` (`id`,`name`,`tid`) VALUES ('4','小李','1');
INSERT INTO `student` (`id`,`name`,`tid`) VALUES ('5','小王','1');
~~~

实体类：

```Java
public class Student {
    private int id;
    private String name;

    //关联老师
    private Teacher teacher;
}
```

```Java
public class Teacher {
    private int id;
    private String name;
}
```

## 10.1、按照查询嵌套处理

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.dao.StudentMapper">

    <!--思路：
           1.查询所有的学生信息
           2.根据查询出来的学生的tid，寻找对应的那个老师
    -->
    <select id="getStudent" resultMap="StudentTeacher">
        select * from student;
    </select>

    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <!--复杂的属性，我们需要单独处理，如果属性是对象，我们采用association，如果属性是集合，我们采用collection
            javaType是因为属性为对象，所以得给其设置一个类型，select作用是通过column中的字段要来进行什么操作
			javaType="" 指定属性的类型
			集合中的泛型信息，我们使用ofType获取
         -->
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
    </resultMap>

    <select id="getTeacher" resultType="Teacher">
        select * from teacher;
    </select>

</mapper>
```

相当于是子查询，先查询出所有的学生，在通过查询出来的结果再进行一次查询。如下这种形式

```sql
select id,name,name from student where tid=(select .......);
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165618.png)



## 10.2、按照结果嵌套处理

```sql
<--s.id sid相当于给其起一个别名，也就相当于数据库里面的id变成了sid-->
<select id="getStudent2" resultMap="StudentTeacher2">
    select s.id sid,s.name sname,t.name tname
    from student s,teacher t
    where s.tid=t.id;
</select>

<resultMap id="StudentTeacher2" type="Student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    
    <--这里是直接根据结果来返回结果的类型，再来嵌套进行查找；即相当于从结果入手，反向查询-->
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165634.png)



回顾M以上情况多对一查询方式：

- 子查询
- 联表查询（即第二种方法）



# 11、一对多

比如：一个老师拥有多个学生

对于老师而言，就是一对多的关系

实体类：

```Java
@Data
public class Teacher {
    private int id;
    private String name;

    //一个老师拥有多个学生
    private List<Student> students;
}
```

```Java
public class Student {
    private int id;
    private String name;
    private int tid;
}
```



## 11.1、按照结果嵌套查询（推荐）

```xml
<select id="getTeacher" resultMap="TeacherStudent">
    select s.id sid,s.name sname ,t.name tname ,t.id tid
    from student s,teacher t
    where s.tid=t.id and t.id=#{tid}
</select>

<resultMap id="TeacherStudent" type="Teacher">
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>

    <!--集合中的泛型信息，我们使用ofType获取
        这里取出来的是每一个值，取出每一个值所对应的，所以我们采用ofType-->
    <collection property="students" ofType="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>
```



**`注意点`：一对多，对于按`结果`查询使用collection时，如果是取出的一个个值，则					需使用ofType来指定其类型，而不使用JavaType**

**而对于按`查询`嵌套使用collection时，我们取出的是一个集合，而不是一个个值，则需要使用JavaType来指定属性的对象类型，同时还得将泛型的信息使用ofType写上**



## 11.2、按照查询嵌套处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
    select * from Teacher where id=#{tid};
</select>

<resultMap id="TeacherStudent2" type="Teacher">
    <--这里是相同属性名的话可以省略不用写-->
    <result property="id" column="id"/>
    <result property="name" column="name"/>
    <!--这里所取出来的值就是一个集合，所以得写上其JavaType的类型，也就是集合的对象类型，并且里面的泛型信息也得写上-->
    <collection property="students" column="id" javaType="ArrayList" ofType="Student" select="getStudent"/>
</resultMap>

<select id="getStudent" resultType="Student">
    <--这个地方的#{id}中的id可以随意填，因为mybatis会自动去匹配，但是为了规范还是正规写-->
    select * from Student where tid=#{id};
</select>
```





## 11.3、小结

	1. 关联--assocation   【多对一】
	2. 集合--collection       【一对多】
	3. JavaType        &&        ofType
	   1. Java Type  用来指定实体类中属性的类型
	   2. ofType   用来指定映射到List或者集合中的pojo类型，即泛型中的约束类型！



注意点：

- 保证SQL的可读性，尽量保证通俗易懂
- 注意一对多和多对一中，属性名和字段的问题
- 如果问题不好排查错误，可以使用日志，建议使用Log4j



# 12、动态SQL

**什么是动态SQL：动态SQL就是根据不同的条件生成不同的语句**

 

## 12.1、搭建环境

~~~sql
CREATE TABLE `blog`(
`id` VARCHAR(50) NOT NULL COMMENT '博客id',
`title` VARCHAR(100) NOT NULL COMMENT '博客标题',
`author` VARCHAR(30) NOT NULL COMMENT '博客作者',
`create_time` DATETIME NOT NULL COMMENT '创建时间',
`views` INT(30) NOT NULL COMMENT '浏览量'
)ENGINE=INNODB DEFAULT CHARSET=utf8
~~~

创建一个基础工程

1. 导包

   ```xml
   <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.18</version>
   </dependency>
   ```

2. 编写配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <!--configuration核心配置文件-->
   <configuration>
       <!--引入外部配置文件-->
       <properties resource="db.properties">
           <property name="username" value="root"/>
           <property name="password" value="xn123456"/>
       </properties>
       
       <settings>
           <setting name="logImpl" value="STDOUT_LOGGING"/>
           <!--是否开启驼峰命名自动映射-->
           <setting name="mapUnderscoreToCamelCase" value="true"/>
       </settings>
   
       <!--可以给实体类起别名-->
       <typeAliases>
           <package name="com.kuang.pojo"/>
       </typeAliases>
   
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="${driver}"/>
                   <property name="url" value="${url}"/>
                   <property name="username" value="${username}"/>
                   <property name="password" value="${password}"/>
               </dataSource>
           </environment>
       </environments>
   
       <mappers>
           <mapper resource="com/kuang/dao/BlogMapper.xml"/>
       </mappers>
   
   </configuration>
   ```

3. 编写实体类

   ```Java
   @Data
   public class Blog {
       private String id;
       private String title;
       private String author;
       private Date createTime;//属性名和字段名不一致，可以开启Setting中的驼峰命名规则
       private int views;
   }
   ```

4. 编写实体类对应Mapper接口和Mapper.xml文件

   ```Java
   public interface BlogMapper {
       //插入数据
       int addBlog(Blog blog);
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.kuang.dao.BlogMapper">
       <insert id="addBlog" parameterType="blog">
           insert into mybatis.blog (id,title,author,create_time,views)
           values (#{id},#{title},#{author},#{createTime},#{views});
       </insert>
   
   </mapper>
   ```

5. 测试

   ```Java
   @Test
   public void addBlogTest() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
       Blog blog = new Blog();
       blog.setId(IDUtils.getId());
       blog.setTitle("Mybatis");
       blog.setAuthor("狂神说");
       blog.setCreateTime(new Date());
       blog.setViews(9999);
   
       mapper.addBlog(blog);
   
       blog.setId(IDUtils.getId());
       blog.setTitle("Java");
       mapper.addBlog(blog);
   
       blog.setId(IDUtils.getId());
       blog.setTitle("Spring");
       mapper.addBlog(blog);
   
       blog.setId(IDUtils.getId());
       blog.setTitle("微服务");
       mapper.addBlog(blog);
   
       sqlSession.close();
   }
   ```

 

## 12.2、IF

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
    select * from mybatis.blog where 1=1
    <if test="title != null">
         and title=#{title}
    </if>
    <if test="author != null">
         and author=#{author}
     </if>
</select>
```

```Java
@Test
public void queryBlogif(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);

    HashMap map = new HashMap();

    //map.put("title","Mybatis");

    map.put("author","狂神说");

    List<Blog> blogList = mapper.queryBlogIf(map);

    for (Blog blog : blogList) {
        System.out.println(blog) ;
    }

    sqlSession.close();
}
```



## 12.3、choose（when,otherwise）

```xml
<select id="queryBlogChoose" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <choose>
            <when test="title != null">
                title=#{title}
            </when>
            <when test="author != null">
                and author=#{author}
            </when>
            <otherwise>
                and views=#{views}
            </otherwise>
        </choose>
    </where>
</select>
```

**注意点：**使用`where`的时候，当条件成立的时候where会自动去除`and`或者`or` 

使用`choose`时，当条件成立时，只会选择一个语句进行执行，其它语句则不执行，当所有语句都不成立时，则会使用`otherwise`中的语句；类似与switch-case，执行一个case后面有break，执行完就跳出



## 12.4、trim(where，set)

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <if test="title != null">
            and title=#{title}
        </if>
        <if test="author != null">
            and author=#{author}
        </if>
    </where>
</select>
```

```xml
<update id="updateBlog" parameterType="map">
    update mybatis.blog
    <set>
        <if test="title != null">
            title=#{title},
        </if>
        <if test="author != null">
            author=#{author}
        </if>
    </set>
    where id =#{id}
</update>
```

`set元素会动态前置SET关键字，同时也会删掉无关的逗号`



**所谓的动态SQL，本质还是SQL语句，只是我们可以在SQL层面，去执行一个逻辑代码**



## 12.5、SQL片段

有时候，我们可能会将一些功能的部分抽取出来，方便复用

1. 使用SQL标签抽取公共的部分

   ```sql
   <sql id="if-title-author">
       <if test="title != null">
           and title=#{title}
       </if>
       <if test="author != null">
           and author=#{author}
       </if>
   </sql>
   ```

2. 在需要使用的地方使用`include`标签引用即可

   ```sql
   <select id="queryBlogIf" parameterType="map" resultType="blog">
       select * from mybatis.blog
       <where>
           <include refid="if-title-author"></include>
       </where>
   </select>
   ```



注意事项：

- 最好基于单表来定义SQL片段（如果是多表查询会让其效率变低，公共部分的拼接就会很复杂）
- 不要存在where标签



## 12.6、Foreach

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165710.png)

```sql
<!--
    这个地方我们在使用foreach时，是遍历的一个集合，而我们传递的参数为map，所有我们的map中可以存在一个集合
    item：从集合中遍历出来的值,也就是传到#{}中的值
    separator：中间的分隔符
-->
<select id="queryBlogForeach" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <foreach collection="ids" item="id" open="and (" close=")" separator="or">
            id=#{id}
        </foreach>
    </where>
</select>
```



**动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式，去排列组合就可以了**

建议：

- 先在Mysql中写出完整的SQL，再对应的去修改成为我们的动态SQL实现通用即可



 

# 13、缓存（了解）

## 13.1、简介

~~~shell
查询--->连接数据库，耗资源
		一次查询的结果，给它暂存到一个可以直接取值的地方！-->内存：缓存
		
我们再次查询相同数据的时候，直接走缓存，就不用走数据库了
~~~



1. 什么是缓存
   - 存在内存中的临时数据
   - 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上（关系型数据库文件）查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题
2. 为什么使用缓存？
   - 减少和数据库的交互次数，减少系统开销，提高系统效率
3. 什么样的数据能使用缓存
   - 经常查询并且不经常改变的数据【可以使用缓存】
   - 不经常查询并且经常改变的数据【不可以使用缓存】





## 13.2、Mybatis缓存

- Mybatis包含一个非常强大的查询缓存特性，它可以非常方便的定制和配置缓存。缓存可以极大的提升查询效率
- Mybatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**
  - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，它是基于namespace级别的缓存。
  - 为了提高扩展性，Mybatis定于了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存



## 13.3、一级缓存

一级缓存也叫本地缓存：SqlSession就是一个一级缓存

- 与数据库同一次会话期间查询到的数据会放在本地缓存中。
- 以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库



测试步骤：

1. 开启日志

2. 测试在一个Sqlsession中查询两次相同的记录

   ```Java
   @Test
   public void test(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       User user = mapper.queryUserById(1);
       System.out.println(user);
   
       System.out.println("================");
       User user1 = mapper.queryUserById(1);
       System.out.println(user1);
   
       System.out.println(user==user1);
   
       sqlSession.close();
   }
   ```

3. 查看日志输出

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165730.png)



缓存失效的情况：

1. 查询不同的东西

2. 增删改操作，可能改变原来的数据，所以必定会刷新缓存

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165744.png)

3. 查询不同的Mapper.xml

4. 手动清理缓存

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165757.png)



小结：一级缓存默认是开启的，只在一次Sqlsession中有效，也就是拿到连接到关闭连接这个区间段！

**一级缓存就是一个map**



## 13.4、二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存
- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存
- 工作机制
  - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中
  - 新的会话查询信息，就可以从二级缓存中获取内容
  - 不同的mapper查出的数据会存放在自己对应的缓存（map）中



步骤：

1. 开启全局缓存

   ```xml
   <!--开启全局缓存-->
   <setting name="cacheEnabled" value="true"/>
   ```

2. 在要使用二级缓存的Mapper中开启

   ```xml
   <!--开启二级缓存-->
   <cache/>
   ```

   也可以自定义参数

   ```xml
   <!--在当前Mapper.xml中使用二级缓存-->
   <cache eviction="FIFO"
          flushInterval="60000"
          size="512"
          readOnly="true"/>
   ```

3. 测试

   - 问题：当我们没有设置策略时，我们需要将实体类实例化！否则就会报错

     ~~~Java
     Caused by: java.io.NotSerializableException: com.kuang.pojo.User
     ~~~

     



小结：

- 只要开启了二级缓存，在同一个Mapper下就有效
- 所有的数据都会先放在一级缓存中
- 只有当会话提交，或者关闭的时候，才会提交到二级缓存中！



## 13.5、缓存原理
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601165812.png)



**缓存顺序：**

1. 先看二级缓存中有没有
2. 再看一级缓存中有没有
3. 两个都没有，再去查数据库 



## 13.6、自定义缓存-ehcache

Ehcache是一种广泛使用的开源Java分布式缓存。主要面向通用缓存



要在程序中使用Ehcache步骤

1. 先导包

   ~~~xml
   <!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
   <dependency>
       <groupId>org.mybatis.caches</groupId>
       <artifactId>mybatis-ehcache</artifactId>
       <version>1.2.1</version>
   </dependency>
   
   ~~~

   在mapper中指定使用我们的ahcache缓存实现

   ```xml
   <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
   ```

ehcache.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    
    <diskStore path="./tmpdir/Tmp_EhCache"/>

    <defaultCache
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>

    <cache
            name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"/>
</ehcache>
~~~
