# Spring


## 1.Spring

### 1.1 简介

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601170509.png)

- 下载spring所有版本信息的地址入口：<https://docs.spring.io/spring-framework/docs/4.3.9.RELEASE/spring-framework-reference/>

- 下载spring所有版本信息的地址：<http://repo.spring.io/release/org/springframework/spring>

- github：<https://github.com/spring-projects/spring-framework>

- 导入spring-webmvc，可以快速一次性导入所有java包）

  ~~~shell
  <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.0.RELEASE</version>
  </dependency>
  ~~~

- 导入spring web jdbc

	~~~shell
	<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.3.3</version>
  </dependency>
	~~~

### 1.2 优点

- 开源的免费容器
- 轻量级，非入侵式的框架（引入不会对原来的代码产生任何影响）
- 控制反转(IOC)，面向切面编程(AOP)
- 支持事务的处理，对框架整合的支持

spring就是一个轻量级的控制反转和面向切面编程的框架

### 1.3 组成

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601170742.png)

### 1.4 学习路线

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601170757.png)

SpringBoot

- 一个快速开发的脚手架
- 基于SpringBoot可以快速的开发单个微服务
- 约定大于配置
- 要先学Spring和SpringMVC

SpringCloud

- 基于SpringBoot实现的

## 2.坑

- 多态父类实现子类独有的方法需要向下强转

- ```java
  public static void main(String[] args){}
  ```
  
  可以由快捷键psvm加回车打出来

- 无参构造函数，只要一new就会执行

- 只要有new一定要写在main方法里

## 3.IOC DI


### 3.1 理论推导（依赖注入：set注入）

```java
//利用set进行动态实现值的注入
private UserDao userDao;
public void setUserDao(UserDao userDao) {
    this.userDao = userDao;
}
```

- 之前程序是主动创建对象！控制权在程序员手上
- 使用set注入后，程序不再具有主动性，而是变成了被动的接受对象
- 这种思想从本质上解决了问题，我们程序员不用再去管理对象的创建了

### 3.2 IOC本质

是一种通过描述（XML或注解）并通过第三方生产或获取特定对象的方式，在spring中实现控制反转的是Ioc容器，其实现方法是依赖注入（DI）

### 3.3 IOC容器(控制反转)

- 配置文件

  - xml中的bean标签就是一个对象，把类用bean标签添加在配置文件中，就相当于：类名 变量名=new 类名()，而标签中的id就是变量名，class就等于new的对象。所以myHello就是一个Hello类对象。这里的对象是由spring创建的，而不是java new出来的。
  - property相当于给对象中的基本数据类型的属性设置一个值（**注意！！！，类中一定要有：set属性名(参数类型 参数值){this.属性名=参数值}   这个方法**  

  ```java
  <bean id="myHello" class="com.kua.pojo.Hello">
      <property name="str" value="spring"/>
  </bean>
  ```

  - property如果要给对象中的对象数据设置值，那就是```<property name="userDao" ref="mySql"/>```其中name表示对象属性名，表示引用spring中创建好的对象

  ```java
  <bean id="myServiceImpl" class="com.kuang.service.UserServiceImpl">
      <property name="userDao" ref="mySql"/>
  </bean>
  ```

- 初始化时，先读取配置文件，获得spring的上下文对象（固定语句不可变）

  ```java
  ApplicationContext context=new ClassPathXmlApplicationContext("bean.xml");//这里的bean.xml是配置文件名
  ```

- 通过上下文对象context的getBean方法从Ioc容器中直接取出对象

  ```java
  Hello myHello=(Hello)context.getBean("hello");
  ```

### 3.4 IOC综合

- 控制：谁来控制对象的创建，传统的应用程序是由程序本身来控制创建的，使用Spring后，对象是由Spring创建的
- 反转：程序本身不创建对象，而变成被动的接收对象

- 依赖注入：利用set方法进行注入
- IOC编程思想：由主动编程，变成被动接收，对象由Spring来创建，管理，装配

### 3.5 IOC创建对象的方式（依赖注入：构造器注入）

使用构造器方式注入可以不写set

- 使用无参构造创建对象（默认的）

- 如果要用有参构造函数创建对象

  方式一：**下标**赋值

  ```java
  <bean id="user" class="com.kuang.pojo.User">
      <constructor-arg index="0" value="renzuoweishihanhan"/>
  </bean>
  ```

  index表示参数的下标，value表示给参数赋值

  方式二：匹配**参数类型**（不建议使用）

  ```java
  <bean id="user" class="com.kuang.pojo.User">
      <constructor-arg type="java.lang.String" value="renzuoweishihanhan"/>
  </bean>
  ```

**注意：基本类型可以直接写，但是引用类型如：String得写全名**

  方式三：直接通过参数名

  ```java
  <bean id="user" class="com.kuang.pojo.User">
      <constructor-arg name="name" value="renzuoweishihanhan"/>
  </bean>
  ```

### 3.6 IOC初始化对象的时间

在new ClassPathXmlApplicationContext，即加载完配置文件后就已经初始化好了**所有bean**对象，然后你需要getBean谁就取谁

## 4.Spring配置

### 4.1 Spring配置

- alias，给bean对象取别名（一对一）

  给容器中的东西取别名

  ```xml
  <alias name="user" alias="user2"/>
  ```

- bean配置

  - id：bean的唯一标识符

  - class：bean对象所对应的全限定名（包+类）

  - name：给bean对象取别名，可以同时取多个别名，中间可以用逗号、空格、分号隔开

  - import：一般用于团队开发使用，他可以将多个文件导入合并为一个

    用import可以导入这个模块的其他bean文件，合并为一个总的，然后通过总的xml文件可以取到所有导入的bean里面的东西，如果不同bean中有相同的配置，但是配置的值不同，则下面的xml文件会覆盖上面的

    <import resource="bean4.xml"/>

## 5.DI依赖注入之set注入

### 5.1、依赖注入之set注入

依赖：bean对象的创建依赖于容器

注入：bean对象的所有属性值由容器注入



复杂类型：

```java
public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

真实测试对象：

```java
public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbies;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;
}
```

bean4.xml

```xml
<bean id="adress" class="com.kuang.pojo.Address"/>
    <bean id="student" class="com.kuang.pojo.Student">
<!--        普通值注入,value-->
        <property name="name" value="yzq"/>
<!--        bean注入，ref-->
        <property name="address" ref="adress"/>
<!--        数组注入-->
        <property name="books">
            <array>
                <value>西游记</value>
                <value>水浒传</value>
                <value>红楼梦</value>
            </array>
        </property>
<!--        List注入-->
        <property name="hobbies">
            <list>
                <value>打篮球</value>
                <value>看剧</value>
                <value>敲代码</value>
            </list>
        </property>
<!--        Map注入-->
        <property name="card">
            <map>
                <entry key="学号" value="2918378"/>
                <entry key="身份证" value="2917321"/>
            </map>
        </property>
<!--        Set注入-->
        <property name="games">
            <set>
                <value>lol</value>
                <value>找你妹</value>
            </set>
        </property>
<!--        null-->
        <property name="wife">
            <null/>
        </property>
<!--        Properties配置类型-->
        <property name="info">
            <props>
                <prop key="学号">21342</prop>
                <prop key="性别">女</prop>
            </props>
        </property>
    </bean>
```

测试类：

```java
ApplicationContext context=new ClassPathXmlApplicationContext("bean4.xml");
Student student=(Student) context.getBean("student");
System.out.println(student.toString());
```

## 6.导入test测试包（junit）

### 6.1、导入test测试包

```xml
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
    <version>5.7.1</version>
    <scope>test</scope>
</dependency>
```

然后在测试类中@Test，这样在测试类中写出的方法，会自动被执行

## 7.c命名和p命名空间注入

### 7.1、命名空间

p命名空间的注入，可以直接注入属性的值：property（本质还是set注入）

1. 前提：
   - 导入约束xmlns:p="http://www.springframework.org/schema/p"
   - 要有set方法

2. 
   ~~~   xml
   <bean id="user" class="com.kuang.pojo.User" p:age="18" p:name="yzq"/>
   ~~~
   
   可以直接在bean标签内注入属性的值

   如果是注入对象则：
   
   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601170944.png)
   
3. 导入junit，并在测试文件中@Test（插曲）

   c命名空间（本质是构造器注入）


4. 前提：
   - 导入约束
~~~xml
xmlns:c="http://www.springframework.org/schema/c"
~~~
   - 要有set方法
   - **必须要有无参构造函数和有参构造函数两种**

5. 
   ~~~xml
   <bean id="user" class="com.kuang.pojo.User" c:age="18" c:name="nihao"/>
   ~~~

6. 测试：

   ```java
   @Test
   public void test2(){
       ApplicationContext context=new ClassPathXmlApplicationContext("beans4.xml");
       User user=context.getBean("user",User.class);
       System.out.println(user);
   };
   ```

## 8.Bean Scopes（bean作用域）

### 8.1、bean作用域

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171233.png)

- 单例模式singleton（Spring默认机制）

 ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171248.png)

  对象只有一个，不管从相同的bean对象中getBean多少个对象，都是表示同一个对象，如下：输出true

  用法：在bean的后面加上scope="singleton"，也可以不加，因为这种作用域是spring默认的

  ```xml
  <bean id="user" class="com.kuang.pojo.User" c:age="18" c:name="nihao" scope="singleton"/>
  ```

  ```java
  User user=context.getBean("user",User.class);
  User user1=context.getBean("user",User.class);
  System.out.println(user==user1);
  ```

- 原型模式prototype

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171320.png)

  用法：scope="prototype"

  每次从容器中get时都会产生新对象，每个对象都不一样

  ```java
  User user=context.getBean("user",User.class);
  User user1=context.getBean("user",User.class);
  System.out.println(user==user1);
  ```

  输入false

- 其他四个只能在web开发中使用

## 9.bean的自动装配（Autowired）

### 9.1、bean自动装配

Spring会在上下文中自动寻找，并自动给bean装配属性，**只针对bean对象**

在Spring中有三种装配方式：

1. 在xml中显示的配置
2. 在java中显示的配置
3. 隐式的自动装配bean（重要）

 

自动装配就是：可以不用在bean中自己**手动装备bean对象的对象属性**，**也就是不用property来为对象中的对象引用赋值，但是只针对对象属性**

用法：**用autowired=""实现**

People.java

```java
public void setName() {
        this.name = name;
    }
public void setCat(Cat cat) {
    this.cat = cat;
}
public void setDog(Dog dog) {
        this.dog = dog;
    }
```

bean5.xml

```xml
<bean id="cat" class="com.kuang.pojo.Cat"/>
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="people" class="com.kuang.pojo.People" autowire="byName">
    <property name="name" value="yzq"/>
</bean>
```

	byName表示在容器上下文中查找和自己set方法后面的值对应的beanid，弊端：名字必须相同

	byType表示在容器上下文中查找和自己set方法中对象类型相同的对应的beanid，都可以省略id。弊
	
	端：容器中所有bean的class唯一，即只有一个对象，如下就会报错

```xml
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="dog1" class="com.kuang.pojo.Dog"/>
<bean id="people" class="com.kuang.pojo.People" autowire="byType">
```

## 10.使用注解实现自动装配

### 10.1 @Autowired

***可以删掉set方法，但是需要get方法！！！***

前提：**只针对bean对象**，通过byType方式实现，如果有多个对象，就通过byName方式实现

- 导入约束

  ```xml
  xmlns:context="http://www.springframework.org/schema/context"
  ```

- 配置注解的支持

  ```xml
  <!--所有注解驱动-->
  <context:annotation-config/>
  ```

- 容器中和.java中的名字要相同

使用：

	在配置文件中注入所有你要自动装配的对象，然后直接在属性上面或者set方法上面使用@Autowired

bean5.xml

```xml
<bean id="cat" class="com.kuang.pojo.Cat"/>
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="people" class="com.kuang.pojo.People"/>
```

People.java

```java
@Autowired
private Cat cat;
@Autowired
private Dog dog;
```

也可以放在set方法上面

### 10.2 @Nullable(拓展)

如果显示定义@Autowired的require属性为false，那么那个对象的属性值为null也不会报错（但是完全没有定义，只有声明，就会报错）

与这有相同效果的是@Nullable，用法：在构造函数的形参前面 

```java
public People(@Nullable String name) {
    this.name = name;
}
```

### 10.3 @Qualifier

当容器中和.java中的对象名字和类型都不匹配时，这时@Autowired就没办法用，但如果又想要自动匹配，就可以使用@Qualifier(value="")实现名字匹配

```xml
<bean id="dog2" class="com.kuang.pojo.Dog"/>
<bean id="dog1" class="com.kuang.pojo.Dog"/>
```

```java
@Autowired
@Qualifier(value="dog1")
private Dog dog;
```

但是可能在运行前要去maven里clean一下，再运行

### 10.4 @Resource

以上三种都是Spring注解，这一个是java注解

@Resource与@Autowired用法一样，但是@Resource先通过byName，再通过byType

当名字和类型都不匹配时：加一个name属性可以指定名字匹配

@Resource(name="")

### 10.5 @Component

见11的第2条的方法二

### 10.6 @Scope

见11的第5条

## 11.使用注解开发

- 在Spring4之后，要使用注解开发，必须保证aop的包导入，所以出错时检查这个包有没有问题
- 导入约束
- 增加支持



1. bean

2. 属性如何注入（开启注解的支持）

   方法一：

   注解驱动

   ```xml
   <context:annotation-config/>
   ```

   方法二：

   指定包中的全部注解都生效

   ```xml
   <context:component-scan base-package="com.kuang.pojo"/>
   ```

   用@Component注解：

   放在类上面，表示 这个类被Spring管理了，就是一个bean，而@Value就相当于bean中的property，给属性（**这里的属性，不只是对象，还包含基本数据类型的属性**）注入值，@Value也可以放在set方法上面

   ```java
   //相当于在配置文件中
   //<bean id="user" class="com.kuang.pojo.User" />
   @Component
   public class User {
       @Value("yzq")
       private String name;
   }
   ```

3. 衍生的注解

   @Component有几个衍生注解，在web开发中，会按照mvc三层架构分层

   - poji层：@Component

   - dao层：@Repository

   - service层：@Service

   - controller层：@Controller

     这四个注解功能一样，都是讲某个类注入到容器中

4. 自动装配置

   以上所有注解

5. 作用域

   也可以使用注解：@Scope("作用域类型")，也是放在类上面的

   ```java
   @Component
   @Scope("prototype")
   public class User{
       
   }
   ```

6. 小结

	xml和注解：

	- xml更加万能，适用于任何场所|维护简单方便
	- 注解：不是自己的类使用不了，维护复杂，因为每个不同的类的注解是独立的

	xml与注解最佳实践

	- xml用来管理bean
	- 注解只负责完成属性的注入
	- 注意：一定要开启注解的支持

## 12.使用java方式配置Spring

### 12.1 用@Configuration配置过程

- 写一个类作为配置类，将@Configuration放在类上面，@Bean放在一个返回类的方法里面

  	@Configuration代表最这是一个配置类，和之前的bean.xml是一样的，本身也会被spring容器托管，注册到容器中，因为他本来就是一个@Component

  	@Bean代表bean，哪个类的bean就写一个返回类的方法，然后bean的id就是方法的方法名

  配置类：

  ```java
  @Configuration
  public class myConfig {
      @Bean
      public User getUser(){
          //返回要注入到bean中的对象
          return new User();
      }
  }
  ```

- 给bean中的属性注入值

  ```java
  @Value("yzq")
  private String name;
  
  public String getName() {
      return name;
  }
  ```

- 测试类：注意是用的AnnotationConfigApplicationContext实现类，因为没有配置文件xml了，完全是用java+注解配置的，实参是写的配置类，bean的id名是配置类中的方法名

  ```java
  ApplicationContext context = new AnnotationConfigApplicationContext(myConfig.class);
  User getUser=(User) context.getBean("getUser");
  System.out.println(getUser.getName());
  ```

### 12.2 @Configuration扫描

- 也可以显示的扫描包

```java
@Configuration
//包中所有注解都生效
@ComponentScan("com.kuang.pojo")
public class myConfig {
    @Bean
    public User getUser(){
        return new User();
    }
}
```

- 因为肯定有多个bean，所以就根据几个bean创建几个java配置类，然后每个类上面都加上@Configuration，代表这是一个配置类但是要把这么多bean整合到一起，就要使用@Import(其他配置类名.class)

## 13.代理模式

### 13.1 代理模式

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171534.png)

​	为什么学习代理模式？

​	因为这是SpringAOP的底层

### 13.2 静态代理

角色分析：

- 抽象角色（租房）：一般会使用接口或者抽象类来解决

  ```java
  public interface rent {
      public void rent();
  }
  ```

- 真实角色（房东）：被代理的角色

  ```java
  public void rent(){
      System.out.println("房东要出租房子");
  };
  ```

- 代理角色（中介）：代理真实角色，代理真实角色后，一般会做一些附属操作（代理角色自己的方法）

  ```java
  public class Proxy {
      public Host host;
  
      public Proxy() {
      }
  
      public Proxy(Host host) {
          this.host = host;
      }
      public void rent(){
          host.rent();
          hetong();
          see();
          fee();
      }
      public void hetong(){
          System.out.println("中介带你签合同");
      }
      public void see(){
          System.out.println("中介带你看房");
      }
      public void fee(){
          System.out.println("中介带你付费");
      }
  }
  ```

- 客户（我）：访问代理对象的人

  ```java
  public class Client {
      public static void main(String[] args) {
          //房东只租房子
          Host host =new Host();
          //代理出来租房子还有一些附属操作
          Proxy proxy=new Proxy(host);
          //不用面对房东，直接找中介即可
          proxy.rent();
      }
  }
  ```

好处：

- 可以使真实角色操作更纯粹，不用去关注一些公共的业务
- 公共也就交给代理角色！实现了业务的分工
- 公共业务发生拓展时，方便集中管理

缺点：

- 一个真实角色就会产生一个代理角色，代码量会翻倍，开发效率低

### 13.3 静态代理再理解

- 抽象角色

  ```java
  public interface UserService {
      public void add();
      public void delete();
      public void update();
      public void query();
  }
  ```

- 真实角色，实现增删改查四个方法

  ```
  public class UserServiceImpl implements UserService{
      public void add(){
          System.out.println("增");
      };
      public void delete(){
          System.out.println("删");
      };
      public void update(){
          System.out.println("改");
      };
      public void query(){
          System.out.println("查");
      };
  }
  ```

- 代理角色：现有客户需要在每次实现什么方法的时候，能够输出实现了什么方法，称为日志功能，这个时候如果去在真实角色的代码里面添加，就违反了”**改动原有代码，在公司中是大忌**“，所以这时候就需要代理去实现

  ```
  public class UserServiceProxy {
      private UserServiceImpl userServiceImpl;
  
      public void setUserServiceImpl(UserServiceImpl userServiceImpl) {
          this.userServiceImpl = userServiceImpl;
      }
  
      public void add(){
          log("add");
          userServiceImpl.add();
      }
      public void delete(){
          log("delete");
          userServiceImpl.delete();
      }
      public void update(){
          log("update");
          userServiceImpl.update();
      }
      public void query(){
          log("query");
          userServiceImpl.query();
      }
      public void log(String a){
          System.out.println("实现了"+a+"方法");
      }
  }
  ```

- 客户：只需要跟代理角色接触，让代理角色去调用真实角色的各种方法

  ```java
  public class Client {
      public static void main(String[] args) {
          UserServiceImpl userServiceImpl=new UserServiceImpl();
          UserServiceProxy userServiceProxy=new UserServiceProxy();
          userServiceProxy.setUserServiceImpl(userServiceImpl);
          userServiceProxy.add();
      }
  }
  ```

### 13.4 聊聊AOP

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171607.png)

AOP实现机制，就是不改变原有代码，增加代理角色，面向切面编程

### 13.5 动态代理详解

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是我们直接写好的
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - 基于接口：JDK的动态代理
  - 基于类：cglib
  - java字节码实现（现在用的多的）：javassist，javassist是一个开源的分析、编辑和创建java字节码的类库

前提：了解两个类

**Proxy类**：生成动态代理实例的，提供动态代理类和实例的静态方法

**InvocationHandle**：调用处理程序并返回一个结果的（调用处理程序实现的接口）



接口：

```java
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();
}
```

真实角色：

```java
public class UserServiceImpl implements UserService{
    public void add(){
        System.out.println("增");
    };
    public void delete(){
        System.out.println("删");
    };
    public void update(){
        System.out.println("改");
    };
    public void query(){
        System.out.println("查");
    };
}
```

创建代理对象的类，ProxyInvocationHandle.java：

```java
//使用这个类动态生成代理类，这个类本身不是代理类，只是处理代理过程的一个类
public class ProxyInvocationHandle implements InvocationHandler {
    //设置被代理的接口
    //实际上就是设置实现接口的真实角色对象
    public Object target;
    public void setTarget(Object target) {
        this.target = target;
    }

    //生成得到代理类
    public Object getProxy(){
        //参数1：实现接口的真实类的位置
        //参数2：真实类
        //参数3：通过什么调用的
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),target.getClass().getInterfaces(),this);
    }

    @Override
    //处理代理实例，并返回结果，这个invoke方法是通过反射自动调用的，在getProxy时传了一个this，里面调用了invoke。每次当代理对象调用方法时都会经过这个方法，这个方法包含了所有代理对象需要处理的业务
    //参数1：动态生成的代理对象的实例
    //参数2：接口的方法，和真实角色接口方法一致
    //代表调用接口方法对应的Object参数数组
    public Object invoke(Object o, Method method, Object[] objects) throws Throwable {
        //因为method是接口的方法，所以getName就可以得到方法名
        log(method.getName());
        Object result=method.invoke(target,objects);
        return result;
    }
    //如果想要增加代理类自己的方法，就直接在下面写，下面是一个实现日志功能的方法
    public void log(String msg){
        System.out.println("实现了"+msg+"方法");
    }
}
```

客户：

```java
public class Client {
    public static void main(String[] args) {
        //真实角色
        UserServiceImpl userService=new UserServiceImpl();
        //处理和生成代理角色的类
        ProxyInvocationHandle pih=new ProxyInvocationHandle();
        //设置要代理的对象，实际上就是真实角色对象
        pih.setTarget(userService);
        //生成代理类,要强转成接口类型，因为动态代理的是接口
        UserService proxy=(UserService) pih.getProxy();
        //现在就可以让代理类处理业务了
        proxy.add();
    }
}
```

输出：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171638.png)



好处：

- 静态代理的优点全有
- 一个动态代理类代理的是一个接口，一般就是对应的一些业务
- 一个动态代理类可以代理多个类，只要实现了同一个接口就可以

## 14.AOP

### 14.1 了解

面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的技术，在不改变原来代码的基础上实现动态的增强

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171655.png)

### 14.2 使用Spring实现Aop

前提：使用AOP织入，需要导入一个依赖包

```xml
<dependencies>
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>
</dependencies>
```

#### 14.2.1 方法一：使用原生Spring API接口（JDK）默认

- 此方法特点是，切入一个方法就要对应一个类

UserService接口

```java
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void search();
}
```

UserServiceImpl实现类

```java
public class UserServiceImpl implements UserService{
    public void add(){
        System.out.println("增");
    }
    public void delete(){
        System.out.println("删");
    }
    public void update(){
        System.out.println("改");
    }
    public void search(){
        System.out.println("查");
    }
}
```

Log类：实现日志功能

```java
public class Log implements MethodBeforeAdvice {
    @Override
    //参数method：要执行的目标对象的方法
    //参数args：参数
    //参数target：目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"类,实现了"+method.getName()+"方法");
    }
}
```

AfterLog类：实现方法执行后，得知返回值功能

```java
public class AfterLog implements AfterReturningAdvice {

    @Override
    //参数returnValue:返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了"+method.getName()+"方法，返回结果为："+returnValue);
    }
}
```

bean9.xml配置类

切入点表示要切入到哪里去，即实现接口的类中的方法要在哪里执行

执行环绕：表示把自定义类与切入点连接起来，对应起来，表示自定义类中的方法在对应的切入点处执行

```xml
<!--    注册bean-->
    <bean id="log" class="com.kuang.log.Log"/>
    <bean id="afterLog" class="com.kuang.log.AfterLog"/>
    <bean id="userServiceImpl" class="com.kuang.service.UserServiceImpl"/>


<!--    方法一:使用原生Spring API接口-->

<!--    配置aop：要在上面导入aop约束-->
    <aop:config>
<!--        切入点:expression="execution(修饰符，返回值，类名，方法名，参数)"-->
<!--        *  代表任意修饰符返回值-->
<!--        com.kuang.service.UserServiceImpl.*  表示这个类下的所有方法-->
<!--        com.kuang.service.UserServiceImpl.*(..)  表示所有方法，(..)代表方法的任意参数-->
        <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>

<!--        执行环绕增加:表示把log,afterLog类切入到上面切入点的所有方法中-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>
```

测试类Test9.java

```java
public class Test9 {
    public static void main(String[] args) {
        ApplicationContext context=new ClassPathXmlApplicationContext("bean9.xml");
//        因为动态代理代理的是接口
        UserService userService=(UserService) context.getBean("userServiceImpl");
        userService.add();
    }
}
```

#### 14.2.2 方法二：使用自定义类（CGLIB）

- 此方法的特点是：切入的所有方法都存在于一个类中，切入 多少个方法就对应多少个函数

切面表示引用哪个类，即哪个类里面有自定义的特定功能的方法，它就是一个类。切面中还要还要定义切入点（切面要引用到哪里去）和通知（是要切入切面类中的哪个方法，且切入到哪里去）

自定义切面类

```java
public class DivPointCut {
    public void before(){
        System.out.println("函数执行之前");
    }
    public void after(){
        System.out.println("函数执行之后");
    }
}
```

bean9.xml

```xml
<!--    方法二:使用自定义类-->
<!--    注入自定义切面类-->
    <bean id="divPointCut" class="com.kuang.divPointCut.DivPointCut"/>
    
    <aop:config>
<!--        自定义切面,ref要引用的类-->
        <aop:aspect ref="divPointCut">
            <!--        切入点-->
            <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
<!--            通知:即自定义切面中的方法,且要指定在哪里调用-->
            <aop:before method="before" pointcut-ref="pointcut"/>
            <aop:after method="after" pointcut-ref="pointcut"/>
        </aop:aspect>
    </aop:config>
```

#### 14.2.3 方法三：注解实现

与方法二原理一模一样，只是将xml换成注解



AnnotationPointCut自定义切面类：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171732.png)

注意选的是上面那个Before注解

```java
//此注解表示这个类是一个切面
@Aspect
public class AnnotationPointCut {
    //此注解表示这个方法是一个通知,切入点就写在@Before内
    @Before("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("函数执行前");
    }
    
    
    @After("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("函数执行后");
    }

    
    @Around("execution(* com.kuang.service.UserServiceImpl.*(..))")
    //这里的joinPoint是指连接点,和切入点类似,但是它可以从切入地方拿东西，是必要的参数
    //记得一定要抛出异常
    public void around(ProceedingJoinPoint jp) throws Throwable{
        System.out.println("环绕前");
        //代表执行方法
        Object proceed=jp.proceed();
        System.out.println("环绕后");

        //连接点还可以获得签名(实际上就是执行了哪个方法)等信息
        Signature signature=jp.getSignature();
        System.out.println("签名:"+signature);
    }
}
```

配置文件

```xml
<!--    方法三:使用注解-->
<!--    注入自定义切面-->
    <bean id="annotationPointCut" class="com.kuang.divPointCut.AnnotationPointCut"/>
<!--    注解支持-->
    <aop:aspectj-autoproxy/>
```

运行：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171748.png)

#### 14.2.4 小结

后面设置为false，使用JDK支持，这也是默认的

设置为true，使用cglib支持

```xml
<!--    注解支持-->
<apo:aspectj-autoproxy proxy-target-class="false"/>
```

## 15.整合mybatis

### 15.1 回顾Mybatis

Mybatis过程：

1. 导入相关jar包

- junit

  ```xml
  <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
  </dependency>
  ```

- mysql

  ```xml
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
  </dependency>
  ```

- mybatis

  ```xml
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
  </dependency>
  ```

- spring-webmvc

  ```xml
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.9.RELEASE</version>
  </dependency>
  ```

- spring-jdbc

  ```xml
  <!--        spring操作数据库的话,需要一个spring-jdbc-->
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-jdbc</artifactId>
              <version>5.1.9.RELEASE</version>
          </dependency>
  ```

- aspectjweaver：aop必导的包

  ```xml
  <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.8.13</version>
  </dependency>
  ```

- mybatis-spring

  ```xml
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.2</version>
  </dependency>
  ```

2. 编写配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://127.0.0.1:3306/nfmall?useUnicode=true&amp;characterEncoding=UTF-8"/>
                   <property name="username" value="root"/>
                   <property name="password" value="uchr@123"/>
               </dataSource>
           </environment>
       </environments>
   </configuration>
   ```

3. 测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210601171826.png)

看到视频23的15:00

## 16.声明式事务

### 16.1 回顾事务

- 把一组业务当成一个业务来做，要么都成功要么都失败
- 涉及到数据的一致性问题
- 确保完整性和一致性



事务ACID原则

- 原子性：确保事务要么都成功要么都失败
- 一致性：事务完成后，要么都提交，要么就都不行，资源状态都要保持一致性
- 隔离性：多个业务操作同一个资源，防止数据损坏
- 持久性：事务一旦提交，无论系统发生什么问题，结果都不会被影响，被持久化的写到存储器中

