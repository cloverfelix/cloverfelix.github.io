# Spring5


## 1、Spring框架概述

1. Spring 是**轻量级**的**开源**的 JavaEE**框架**
2. Spring 可以解决企业应用开发的复杂性 
3. Spring 有两个核心部分：**IOC**和 **Aop** 
	- IOC：控制反转，把创建对象过程交给 Spring 进行管理
	- AOP：面向切面，不修改源代码进行功能增强
4. Spring 特点
	- 方便解耦，简化开发
	- AOP编程支持
	- 方便程序测试
	- 方便和其它框架进行整合
	- 方便进行事务操作
	- 降低API开发难度

## 2、入门案例

1、创建一个Java工程

2、导入相关的依赖包
	
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211029201059.png)
	
3、创建普通类
~~~Java
package com.clover.spring5;

public class User {
    public void add()
    {
        System.out.println("Add.............");
    }
}
~~~

4、创建Spring配置文件，在配置文件中配置创建的对象

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置User对象创建-->
    <bean id="user" class="com.clover.spring5.User" />
</beans>
~~~

5、进行测试代码编写

~~~Java
package com.clover.spring5.Test;

import com.clover.spring5.User;
import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class demo {
    @Test
    public void testAdd()
    {
        //1.加载配置文件
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");

        //2.获取配置创建的对象
        User user = context.getBean("user", User.class);

        System.out.println(user);
        user.add();

    }
}
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211029201350.png)


## 3、IOC(概念和原理)

### 3.1、什么是IOC

1. 控制反转，把对象的创建和对象之间的调用过程，交给Spring进行管理
2. 使用IOC的目的：为了耦合度降低


### 3.2、IOC底层原理

- xml解析、工厂模式、反射

- 反射：通道得到该类的字节码文件，然后可以操作该类中的所有内容

- 原始方式

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211029203925.png)

- 工厂模式

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211029204220.png)

- IOC模式

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211029203548.png)


## 4、IOC(BeanFactory 接口)

1. **IOC思想基于IOC容器完成，IOC容器底层就是对象工厂**

2. Spring提供IOC容器实现的**两种方法**：（两个接口）

	- `BeanFactory`：IOC容器的基本实现，是Spring内部的使用接口，不提供给开发人员进行使用
		- **与ApplicationContext的区别**：它在加载配置文件的时候不会去创建对象，在获取(使用)对象的时候才去创建对象
	- `ApplicationContext`：BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员进行使用
		- **与BeanFacotory的区别**：它在加载配置文件的时候就会把在配置文件中配置的对象进行创建

	-  上述两种方式那种更好呢？
	-  有人说：第一种方式好，因为只有在用的时候才去使用，节约内存空间
	-  有人说：第二种方式好，因为不需要在使用的时候=去创建，因为那会耗费很多时间和资源
	-  综上所述：第二种方式更好，我们将这些耗费时间和资源的事情全部交给服务器去做，在启动的时候就全部做完，我们直接使用即可

3. ApplicationContext接口有实现类

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211029210834.png)

## 5、IOC的操作

### 5.1、Bean管理

**IOC的操作实质上就是对Bean进行操作，因为IOC容器就是管理着一个个Bean，而Bean有两个操作：1、创建对象，2、注入属性**

1. 什么是Bean管理
	- Bean管理指的是两个操作
		- Spring创建对象
		- Spring注入属性

2. 怎样实现Bean管理操作
	1. 基于xml配置文件方式实现
		- 基于该方法实现属性注入时有**两种方法**
			- `set方法注入`
			- `有参构造函数注入`
	2. 基于注解方式实现

### 5.2、Bean管理操作实现

1、基于xml 方式创建对象

~~~xml
<!--配置User对象创建-->
<bean id="user" class="com.clover.spring5.User" />
~~~
- 在spring配置文件中，使用bean标签，标签里面添加对应的属性，就可以实现对象创建
- 在bean标签里面有很多属性，介绍常用属性
	- `id属性`：类似于对象的一个别名或者标识，并不是代表对象，是通过该别名去获取真实的对象，要有唯一性
	- `class属性`：类的全路径，即该对象所对应的类路径

2、基于xml方式注入属性
- DI：依赖注入，就是属性注入
- **`面试时问`：IOC与DI的区别**
	- DI是IOC中的一种具体实现，它表示属性注入，属性注入是在`对象创建好的基础之上`去执行的

３、第一种注入方式：**使用set方法进行注入**
- 创建类，定义属性和set方法

~~~Java
package com.clover.spring5;

/**
 * 演示使用set方法进行注入属性
 */
public class Book {
    // 创建属性
    private String name;
    private String author;

    // 创建属性对应的set方法
    public void setName(String name) {
        this.name = name;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public void test(){
        System.out.println(name+"::::::"+author);
    }
}
~~~

- 在spring配置文件中配置对象创建，配置属性注入

~~~xml
<!--set方法注入属性-->
<bean id="book" class="com.clover.spring5.Book">
	<!--
		使用 property 完成属性注入 
		name：类里面属性名称 
		value：向属性注入的值 
	-->
	<property name="name" value="易筋经"/>
	<property name="author" value="达摩老祖"/>
</bean>
~~~

4、第二种注入方式：**使用有参构造函数进行注入**
- 创建类，定义属性，创建属性对应有参数构造方法


~~~Java
//有参构造函数
public Book(String name, String author) {
	this.name = name;
	this.author = author;
}
~~~

- 在 spring 配置文件中进行配置

~~~xml
<!--使用有参构造函数进行注入-->
<bean id="book" class="com.clover.spring5.Book">
	<!--
		使用 constructor-arg 完成属性注入
		name：类里面属性名称
		value：向属性注入的值
	-->
	<constructor-arg name="name" value="易筋经"/>
	<constructor-arg name="author" value="达摩老祖"/>
</bean>
~~~

5、p名称空间注入（了解）
- **p名称空间注入底层还是使用了Set方法注入**
- 使用p名称空间注入，可以简化基于xml配置方式
- 第一步 添加p名称空间在配置文件中
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
~~~
- 第二步 进行属性注入，在bean标签里面进行操作

~~~xml
<!--p命名空间注入属性-->
<bean id="book" class="com.clover.spring5.Book" p:name="九阳神功" p:author="无名氏"/>	
~~~

### 5.3、IOC 操作 Bean 管理(xml 注入其他类型属性)

#### 5.3.1、字面量

1、空值(NULL值)
~~~xml
<!--给属性设null值-->
<property name="address">
	<null/>
</property>
~~~

2、属性值包含特殊符号
~~~xml
<!--属性值包含特殊符号
	  1、把<>进行转义  &lt;&gt; 代表大于号、小于号
	  2、把带特殊符号内容写道CDATA中
-->
<property name="address">
	<value><![CDATA[<<赤壁>>]]></value>
</property>
~~~

#### 5.3.2、注入属性-外部Bean

1、创建两个类service类和dao类

2、在 service 调用 dao 里面的方法

3、在 spring配置文件中进行配置

~~~xml
<!--1 service和dao对象创建-->
<bean id="userService" class="com.clover.spring5.service.UserService">
	<!--注入UserDao对象，外部Bean
		name属性：类里面属性名称
		ref属性：创建UserDao对象bean标签ID值
	-->
	<property name="userDao" ref="userDao"/>
</bean>

<!--因为接口不能创建对象，所以class路径指向接口实现类-->
<bean id="userDao" class="com.clover.spring5.dao.UserDaoImpl"/>
~~~

~~~Java
package com.clover.spring5.service;

import com.clover.spring5.dao.UserDao;

public class UserService {
    // 创建UserDao类型属性，生成set方法进行属性注入
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void add() {
        System.out.println("UserService add .............");
        userDao.update();
    }
}
~~~

#### 5.3.3、注入属性-内部Bean
1、一对多关系：部门和员工
一个部门有多个员工，一个员工属于一个部门
部门是一，员工是多

2、在实体类之间表示一对多关系，员工表示所属部门，使用对象类型进行表示

~~~Java
package com.clover.spring5.bean;

//部门类
public class Dept {

    private String dname;

    public void setDname(String dname) {
        this.dname = dname;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "dname='" + dname + '\'' +
                '}';
    }
}
~~~

~~~Java
package com.clover.spring5.bean;

// 员工类
public class Emp {
    private String ename;
    private String gender;

    //员工属于某一个部门，使用对象形式表示
    private Dept dept;

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    public void setEname(String ename) {
        this.ename = ename;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public void test()
    {
        System.out.println(ename+":::"+gender+":::"+dept);
    }
}
~~~

3、在spring配置文件中进行配置
~~~xml
<!--内部bean-->
<bean id="emp" class="com.clover.spring5.bean.Emp">
	<!--设置两个普通属性-->
	<property name="ename" value="clover"/>
	<property name="gender" value="男"/>
	<!--设置对象类型属性-->
	<property name="dept">
		<bean id="dept" class="com.clover.spring5.bean.Dept">
			<property name="dname" value="技术部"/>
		</bean>
	</property>
</bean>
~~~

#### 5.3.4、注入属性-级联赋值

##### 5.3.4.1、第一种写法

~~~xml
<!--级联赋值-->
<bean id="emp" class="com.clover.spring5.bean.Emp">
	<!--设置两个普通属性-->
	<property name="ename" value="clover"/>
	<property name="gender" value="男"/>
	<!--级联赋值-->
	<property name="dept" ref="dept"/>
</bean>

<bean id="dept" class="com.clover.spring5.bean.Dept">
	<property name="dname" value="安保部"/>
</bean>
~~~

##### 5.3.4.2、第二种写法

~~~xml
 <!--级联赋值-->
<bean id="emp" class="com.clover.spring5.bean.Emp">
	<!--设置两个普通属性-->
	<property name="ename" value="clover"/>
	<property name="gender" value="男"/>
	<!--级联赋值-->
	<property name="dept" ref="dept"/>
	<property name="dept.dname" value="财务部"/>
</bean>

<bean id="dept" class="com.clover.spring5.bean.Dept">
	<property name="dname" value="安保部"/>
</bean>
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211030172853.png)

	注意：使用级联赋值时，如果你property中的name=对象.属性名，那么你需要在该property对应Bean的实体类中加入get
	方法，因为你不先拿到该对象时无法对该对象进行操作的
	同时，你在使用第二种方式的同时，第一种方法的写法也得保存下来，要不然会报错，说你对应的属性值为null
	
**自己小总结：**
1. IOC的底层就是基于xml、工厂模式和反射
2. 先配置好xml，然后工厂模式对xml进行解析，最后通过反射创建对象
3. IOC基于容器实现，容器的底层又是工厂模式
4. 工厂模式有两种实现方式：
	1. Application
	2. BeanFactory
5. 使用set方法进行注入时，是因为工厂模式在解析xml的时候，xml中的property设置的值就会去调用set方法设置对应属性的值，否则会报null错误 **(这点为自我总结，还未查阅资料，不知正确与否)**
6. 使用构造器进行注入时，与使用set方法注入是一样的作用

### 5.4、IOC 操作 Bean 管理(xml 注入集合属性)
1、注入数组类型属性

2、注入 List 集合类型属性

3、注入Map集合类型属性

- 创建类，定义数组、list、map、set类型属性，生成对应 set方法
~~~Java
package com.clover.spring5.collectiontype;

import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Set;

public class Stu {
    //1 数组类型属性
    private String[] courses;

    //2 list集合类型属性
    private List<String> lists;

    //3 map集合类型属性
    private Map<String,String> maps;

    //4 set集合类型属性
    private Set<String> sets;

    public void setCourses(String[] courses) {
        this.courses = courses;
    }

    public void setLists(List<String> lists) {
        this.lists = lists;
    }

    public void setMaps(Map<String, String> maps) {
        this.maps = maps;
    }

    public void setSets(Set<String> sets) {
        this.sets = sets;
    }

    public void test()
    {
        System.out.println(Arrays.toString(courses));
        System.out.println(lists);
        System.out.println(maps);
        System.out.println(sets);
    }
}
~~~

- 在 spring配置文件进行配置

~~~xml
<!--集合类型属性注入-->
<bean id="stu" class="com.clover.spring5.collectiontype.Stu">
	<!--数组类型属性注入-->
	<property name="courses">
		<array>
			<value>Java 课程</value>
			<value>数据库课程</value>
		</array>
	</property>

	<!--list类型属性注入-->
	<property name="lists">
		<list>
			<value>李四</value>
			<value>小李子</value>
		</list>
	</property>

	<!--map类型属性注入-->
	<property name="maps">
		<map>
			<entry key="JAVA" value="java"/>
			<entry key="PHP" value="php"/>
		</map>
	</property>

	<!--set类型属性注入-->
	<property name="sets">
		<set>
			<value>Mysql</value>
			<value>Redis</value>
		</set>
	</property>
</bean>
~~~

#### 5.4.1、xml注入集合属性两个小细节

1、在集合里面设置对象类型值

~~~xml
<!--注入list集合类型，值是对象-->
<property name="courseList">
	<list>
		<ref bean="course1"/>
		<ref bean="course2"/>
	</list>
</property>
<!--创建多个course对象-->
<bean id="course1" class="com.clover.spring5.collectiontype.Courses">
	<property name="cname" value="spring"/>
</bean>
<bean id="course2" class="com.clover.spring5.collectiontype.Courses">
	<property name="cname" value="mybatis"/>
</bean>
~~~


2、把集合注入部分提取出来
- 在 spring 配置文件中引入名称空间 util
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">


</beans>
~~~

- 使用 util 标签完成 list 集合注入提取

~~~xml
<!--1 提取list集合类型属性注入-->
<util:list id="bookList">
	<value>易筋经</value>
	<value>九阴真经</value>
	<value>九阳神功</value>
</util:list>

<!--2 提取list集合类型属性注入使用-->
<bean id="book" class="com.clover.spring5.collectiontype.Book">
	<property name="list" ref="bookList"/>
</bean>
~~~

### 5.5、IOC 操作 Bean 管理(FactoryBean)

1、**Spring有两种类型bean，一种普通bean，另外一种叫工厂bean(FactoryBean)**

2、`普通 bean`：在配置文件中定义的 bean类型就是返回类型

3、`工厂bean`：在配置文件定义的 bean类型可以和返回类型不一样

- 第一步 创建类，让这个类作为工厂 bean，实现接口 FactoryBean
- 第二步 实现接口里面的方法，在实现的方法中定义返回的bean类型

~~~Java
package com.clover.spring5.factorybean;

import com.clover.spring5.collectiontype.Courses;
import org.springframework.beans.factory.FactoryBean;

// 工厂Bean，可以让返回对象类型和class中定义的不一样
public class MyBean implements FactoryBean<Courses> {
    
    //定义返回bean的类型
    @Override
    public Courses getObject() throws Exception {
        Courses courses = new Courses();
        courses.setCname("clover");
        return courses;
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
}
~~~

~~~xml
<bean id="myBean" class="com.clover.spring5.factorybean.MyBean"></bean>
~~~

~~~Java
@Test
public void test3()
{
	// 1.加载配置文件
	ApplicationContext context = new ClassPathXmlApplicationContext("bean3.xml");

	// 2.获取配置文件创建的对象
	Courses courses = context.getBean("myBean", Courses.class);

	System.out.println(courses);
}
~~~

### 5.6、IOC 操作 Bean 管理(bean 作用域)

1. 在 Spring里面，设置创建 bean实例是单实例还是多实例
2. 在 Spring里面，默认情况下，**bean 是单实例对象**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211031101923.png)

3. 如何设置单实例还是多实例
	1. 在 spring配置文件 bean标签里面有属性（scope）用于设置单实例还是多实例
	2. scope 属性值
		- 第一个值  默认值，singleton，表示是单实例对象 
		- 第二个值  prototype，表示是多实例对象
		
		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211031101947.png)
		
		- **singleton 和 prototype 区别 **
			1. 第一  singleton 单实例，prototype 多实例
			2. 第二 设置 scope 值是 singleton时候，加载 spring 配置文件时候就会创建单实例对象 
			3. 设置 scope 值是 prototype 时候，不是在加载 spring 配置文件时候创建 对象，在调用 getBean方法时候创建多实例对象


### 5.7、IOC 操作 Bean 管理(bean 生命周期)
1. 生命周期
	1. 从对象创建到对象销毁的过程
2. **bean 生命周期**
	1. 通过构造器创建bean实例  (调用无参构造函数)
	2. 为bean的属性设置值和对其它bean引用  (调用set方法)
	3. 调用bean的初始化方法  (需要进行配置初始化的方法)
	4. bean可以使用了  (对象获取到了)
	5. 当容器关闭的时候，调用bean的销毁方法  (需要进行配置销毁的方法)
3. 演示 bean 生命周期

~~~Java
package com.clover.spring5.bean;

public class Orders {
    //无参数构造
    public Orders(){
        System.out.println("第一步 执行无参数构造创建 bean 实例");
    }

    private String oname;
    public void setOname(String oname) {
        this.oname = oname;
        System.out.println("第二步 调用 set 方法设置属性值");
    }
    //创建执行的初始化的方法
    public void initMethod() {
        System.out.println("第三步 执行初始化的方法");
    }
    //创建执行的销毁的方法
    public void destroyMethod() {
        System.out.println("第五步 执行销毁的方法");
    }

}
~~~

~~~xml
<bean id="orders" class="com.clover.spring5.bean.Orders" init-method="initMethod" destroy-method="destroyMethod">
	<property name="oname" value="adas"/>
</bean>
~~~

~~~Java
@Test
public void test4()
{
	// 1.加载配置文件
	ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean4.xml");

	// 2.获取配置文件创建的对象
	Orders orders = context.getBean("orders", Orders.class);

	System.out.println("第四步 获取创建 bean 实例对象");
	System.out.println(orders);

	//手动让bean实例销毁
	context.close();
}
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211031104706.png)

4. **bean的后置处理器，bean生命周期有七步**
	1. 通过构造器创建bean实例  (调用无参构造函数)
	2. 为bean的属性设置值和对其它bean引用  (调用set方法)
	3. 把 bean 实例传递 bean 后置处理器的方法 postProcessBeforeInitialization 
	4. 调用bean的初始化方法  (需要进行配置初始化的方法)
	5. 把 bean 实例传递 bean 后置处理器的方法 postProcessAfterInitialization
	6. bean可以使用了  (对象获取到了)
	7. 当容器关闭的时候，调用bean的销毁方法  (需要进行配置销毁的方法)
5. 演示添加后置处理器效果
	1. 创建类，实现接口 BeanPostProcessor，创建后置处理器
~~~Java
package com.clover.spring5.bean;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.lang.Nullable;

public class MyBeanPost implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("在初始化之前执行的方法");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("在初始化之后执行的方法");
        return bean;
    }
}
~~~

~~~xml
<!--配置后置处理器-->
<bean id="myPostBean" class="com.clover.spring5.bean.MyBeanPost"/>
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211031105108.png)

**问题、疑惑：**

	自己的一个问题，但还没有查找资料解决
	创建bean实例通过调用无参构造函数来指向，但是当你使用了有参构造函数进行属性注入时，无参构造函数没有了，bean
	实例如何创建的
	
### 5.8、IOC 操作 Bean 管理(xml 自动装配)
1. 什么是自动装配
	- 根据指定装配规则（属性名称或者属性类型），Spring自动将匹配的属性值进行注入
2. 演示自动装配过程
	- 根据属性名称自动注入
		~~~xml
		<!--实现自动装配
				bean标签属性autowire，配置自动装配
				autowire属性常用两个值：
					byName根据属性名称注入 ，注入值bean的id值和类属性名称一样
					byType根据属性类型注入 ，但是只能每次同一个对象只能有一个，多个就不知道该调用哪一个了
			-->
			<bean id="emp" class="com.clover.spring5.autowire.Emp" autowire="byName">
		<!--        <property name="dept" ref="dept"/>-->
			</bean>

			<bean id="dept" class="com.clover.spring5.autowire.Dept"/>
		~~~
	- 根据属性类型自动注入
		~~~xml
		<!--实现自动装配
				bean标签属性autowire，配置自动装配
				autowire属性常用两个值：
					byName根据属性名称注入 ，注入值bean的id值和类属性名称一样
					byType根据属性类型注入 ，但是只能每次同一个对象只能有一个，多个就不知道该调用哪一个了
			-->
			<bean id="emp" class="com.clover.spring5.autowire.Emp" autowire="byType">
		<!--        <property name="dept" ref="dept"/>-->
			</bean>

			<bean id="dept" class="com.clover.spring5.autowire.Dept"/>
		~~~
		
### 5.9、IOC 操作 Bean 管理(外部属性文件)

1. 直接配置数据库信息
	1. 配置Druid连接池
	2. 引入Druid连接池依赖 jar包
~~~xml
<!--直接配置连接池-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
	<property name="driverClassName" value="com.mysql.jdbc.Driver"/>
	<property name="url" value="jdbc:mysql://localhost:3306/userDb"/>
	<property name="username" value="root"/>
	<property name="password" value="root"/>
</bean>
~~~
2. 引入外部属性文件配置数据库连接池
	- 创建外部属性文件，properties格式文件，写数据库信息
		~~~xml
		prop.driverClass= com.mysql.jdbc.Driver
		prop.url= jdbc:mysql://localhost:3306/userDb
		prop.userName=root
		prop.password=root|
		~~~
	- 把外部 properties 属性文件引入到 spring 配置文件中：**引入 context 名称空间**

		~~~xml
		<?xml version="1.0" encoding="UTF-8"?>
		<beans xmlns="http://www.springframework.org/schema/beans"
			   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			   xmlns:p="http://www.springframework.org/schema/p"
			   xmlns:util="http://www.springframework.org/schema/util"
			   xmlns:context="http://www.springframework.org/schema/context"
			   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
								   http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd
								   http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

		</beans>
		~~~
	- 在spring 配置文件使用标签引入外部属性文件

		~~~xml
		<!--引入外部属性文件-->
		<context:property-placeholder location="classpath:jdbc.properties"/>
		<!--配置连接池-->
		<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
			<property name="driverClassName" value="${prop.driverClass}"/>
			<property name="url" value="${prop.url}"/>
			<property name="username" value="${prop.userName}"/>
			<property name="password" value="${prop.password}"/>
		</bean>
		~~~
		
### 5.10、IOC 操作 Bean 管理(基于注解方式)
1. 什么是注解
	- 注解是代码特殊标记，格式：@注解名称(属性名称=属性值, 属性名称=属性值..)
	- 使用注解，注解作用在类上面，方法上面，属性上面
	- 使用注解目的：简化 xml配置
2. Spring 针对 Bean 管理中创建对象提供注解
	- @Component
	- @Service
	- @Controller
	- @Repository
	- **上面四个注解功能是一样的，都可以用来创建bean实例**
3. 基于注解方式实现对象创建
	- 第一步 引入AOP依赖
	- 第二步 开启组件扫描
		~~~xml
		<!--开启组件扫描
				1 如果扫描多个包，多个包使用逗号隔开
				2 扫描包上层目录
		-->
		<context:component-scan base-package="com.clover"/>
		~~~
	- 第三步 创建类，在类上面添加创建对象注解

		~~~Java
		package com.clover.spring5.bean;

		import org.springframework.stereotype.Service;

		//在注解里面value属性值可以省略不写，
		//默认值是类名称，首字母小写
		@Service  // 该注解就等价于<bean id="userService" class="..."/>
		public class UserService {
			public void add() {
				System.out.println("service add.......");
			}
		}
		~~~
4. 开启组件扫描细节配置
~~~xml
<!--示例1
	use-default-filters="false" 表示现在不使用默认filter，自己配置filter 
	context:include-filter ，设置扫描哪些内容
-->
<context:component-scan base-package="com.clover" use-default-filters="false">
	<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>

<!--示例2
	下面配置扫描包所有内容
	context:exclude-filter： 设置哪些内容不进行扫描
-->
<context:component-scan base-package="com.clover">
	<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
~~~

5. 基于注解方式实现属性注入
	1. `@Autowired`：根据属性类型进行自动装配
		- 第一步 把 service 和 dao 对象创建，在 service 和 dao 类添加创建对象注解
		- 第二步 在 service 注入 dao 对象，在 service 类添加 dao类型属性，在属性上面使用注解
			~~~Java
			//在注解里面value属性值可以省略不写，
			//默认值是类名称，首字母小写
			@Service  // 该注解就等价于<bean id="userService" class="..."/>
			public class UserService {
				//定义dao类型属性
				//不需要添加set方法
				//添加注入属性注解
				@Autowired      //根据类型进行注入
				@Qualifier(value = "userDaoImpl1") //根据名称进行注入
				private UserDao userDao;

				public void add() {
					System.out.println("service add.......");
					userDao.add();
				}
			}
			~~~
	2. `@Qualifier`：根据名称进行注入;**这个@Qualifier 注解的使用，和上面@Autowired 一起使用**
		~~~Java
		//定义dao类型属性
		//不需要添加set方法
		//添加注入属性注解
		@Autowired      //根据类型进行注入
		@Qualifier(value = "userDaoImpl1") //根据名称进行注入
		private UserDao userDao;
		~~~
	3. `@Resource`：可以根据类型注入，也可以根据名称注入
		~~~Java
			@Resource  //根据类型进行注入
		//    @Resource(name = "userDaoImpl1")  //根据名称进行注入
			private UserDao userDao;
		~~~
	4. `@Value`：注入普通类型属性
		~~~Java
		@Value(value = "abc") 
		private String name;
		~~~
		
			注意：前面三个注解是针对对象类型注入，而不是普通属性注入
						使用了注解不用写set方法，因为注解里面帮我们封装好了


### 5.11、完全注解开发
1、创建配置类，替代xml配置文件

~~~Java
package com.clover.spring5.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = {"com.clover"})
public class SpringConfig {
}
~~~

2、编写测试类

~~~Java
@Test
public void test2()
{
	//加载配置类
	ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);

	UserService userService = context.getBean("userService", UserService.class);

	System.out.println(userService);
	userService.add();
}
~~~

## 6、AOP（概念）

### 6.1、什么是AOP
1. 面向切面（方面）编程，**利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低**，提高程序的可重用性，同时提高了开发的效率
2. 通俗描述：不通过修改源代码方式，在主干功能里面添加新功能
3. 使用登录例子说明AOP

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211031160654.png)

### 6.2、AOP(底层原理)

动态代理
1. 传入的目标对象
2. 利用反射机制返回一个代理对象
3. 通过代理对象调用目标对象方法触发`事情处理器方法`


1. AOP底层使用动态代理
	1. 有两种情况动态代理
		1. 第一种 有接口情况，使用JDK动态代理
			- 创建接口实现类代理对象，增强类的方法

		![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211031162049.png)

		2. 第二种 没有接口情况，使用CGLIB动态代理
		- 创建子类的代理对象，增强类的方法

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211031162347.png)

### 6.3、AOP(JDK 动态代理)

- **想对哪个类方法进行加强，就传入哪个类对象，创建目标对象可以用接口变量形式接收**
	- **利用反射机制返回一个代理对象**
		- `需要准备3个参数`
		1. `当前目标对象使用的类加载器`
		2. `目标对象实现的接口类型`
		3. `创建代理对象`
	- **通过代理对象调用方法后，就会实现功能增强；调用目标对象方法后，会触发事情处理器方法，会把当前执行的目标对象方法作为参数传入**

~~~Java
package com.clover.spring5;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.Arrays;

public class JDKProxy {
    public static void main(String[] args) {
        //创建目标对象实现的接口类型
        Class[] interfaces = {UserDao.class};
        //创建目标对象
        UserDao target = new UserDaoImpl();
        //给目标对象创建代理对象
        //返回指定接口的代理类的实例，该接口将方法调用分派给指定的调用处理程序
        /**
         * ClassLoader loader: 指定当前目标对象使用的类加载器，获取加载器的方法固定
         * Class<?>[] interfaces: 目标对象实现的接口类型，使用泛型方式确认
         * InvocationHandler h: 事情处理，执行目标对象的方法，会触发事情处理器方法，会把当前执行的目标对象方法作为参数传入
         */
        UserDao dao = (UserDao) Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(target));
        //System.out.println((Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao))) instanceof UserDao);
        int result = dao.add(1, 2);
        System.out.println("result:"+result);
    }
}

// 创建代理对象
class UserDaoProxy implements InvocationHandler{
    //1 把创建的是谁的代理对象，就把谁传递过来
    //有参数构造传递
    private Object obj;
    public UserDaoProxy(Object obj) {
        this.obj = obj;
    }

    //增强功能的逻辑
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //方法之前
        System.out.println("方法之前执行...."+method.getName()+" :传递的参数..."+ Arrays.toString(args));
        //被增强的方法执行
        //通过反射机制调用目标对象的方法
        Object res = method.invoke(obj, args);
        //System.out.println(res);
        //方法之后
        System.out.println("方法之后执行...."+obj);
        return res;
    }
}
~~~

~~~Java
package com.clover.spring5;

public class UserDaoImpl implements UserDao {
    @Override
    public int add(int a, int b) {
        System.out.println("1111");
        return a+b;
    }

    @Override
    public String update(String id) {
        return id;
    }
}
~~~

~~~interface
package com.clover.spring5;

public interface UserDao {
    public int add(int a,int b);
    public String update(String id);
}
~~~

### 6.4、AOP术语

**1、连接点**
- 类里面哪些方法可以被增强

**2、切入点**
- 实际被真正增强的方法，称为切入点

**3、通知(增强)**
- 实际增强的逻辑部分称为通知(增加)
- 通知有多种类型
	- `前置通知`：在某一个方法之前执行
	- `后置通知`：在某一方法之后执行
	- `环绕通知`：在某一方法之前之后都执行
	- `异常通知`：在某一方法出现异常时执行
	- `最终通知`：类似于finally，不管怎么样最后都会执行

**4、切面**
- 是动作，把通知应用到切入点的过程

### 6.5、AOP操作(准备工作)

1. Spring 框架一般都是基于 AspectJ 实现 AOP 操作
	- AspectJ 不是 Spring 组成部分，是独立 AOP 框架，一般把 AspectJ 和 Spirng 框架一起使用，进行AOP操作
2. 基于 AspectJ 实现 AOP 操作
	- **基于xml配置文件实现**
	- **基于注解方式实现（使用）**
3. 在项目工程里面引入AOP相关依赖
4. 切入点表达式
	- 切入点表达式作用：`知道对哪个类里面的哪个方法进行增强`
	- 语法结构： execution([权限修饰符] [返回类型] [类全路径] [方法名称]([参数列表]))
	- **使用两个`..`代表参数列表**
	- 举例 1：对 com.atguigu.dao.BookDao 类里面的 add 进行增强
	- `execution(*  com.atguigu.dao.BookDao.add(..))`
	- 举例 2：对 com.atguigu.dao.BookDao 类里面的所有的方法进行增强 
	- `execution(*  com.atguigu.dao.BookDao.* (..))`
	- 举例 3：对 com.atguigu.dao包里面所有类，类里面所有方法进行增强 
	- `execution(*  com.atguigu.dao.*.* (..))`

### 6.6、AOP 操作(AspectJ 注解)

1、创建类，在类里面定义方法

~~~Java
@Component
public class User {
    public void add()
    {
        System.out.println("add.................");
    }
}
~~~

2、创建增强类（编写增强逻辑）

~~~Java
//增强的类
@Component
@Aspect//生成代理对象 
public class UserProxy {

    public void before() {//前置通知
        System.out.println("before......");
    }
}
~~~

3、进行通知的配置
- 在 spring配置文件中，开启注解扫描

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" 
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
    <!-- 开启注解扫描 -->
    <context:component-scan base-package="com.clover.spring5.aopannotation"/>

	<!-- 开启Aspect生成代理对象-->
	<aop:aspectj-autoproxy/>
    
</beans>
~~~

- 使用注解创建 User 和 UserProxy 对象
- **在增强类上面添加注解 @Aspect**
- **在 spring配置文件中开启生成代理对象**

4、配置不同类型的通知
- 在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置

~~~Java
//增强的类
@Component
@Aspect//生成代理对象
public class UserProxy {

    @Before(value = "execution(* com.clover.spring5.aopannotation.User.add(..))")
    public void before() {//前置通知
        System.out.println("before......");
    }

    //后置通知（返回通知）(方法有异常时不会执行)
    @AfterReturning(value = "execution(* com.clover.spring5.aopannotation.User.add(..))")
    public void afterReturning() {
        System.out.println("afterReturning.........");
    }

    //最终通知(方法有异常时也会执行)
    @After(value = "execution(* com.clover.spring5.aopannotation.User.add(..))")
    public void after() {
        System.out.println("after.........");
    }

    //异常通知
    @AfterThrowing(value = "execution(* com.clover.spring5.aopannotation.User.add(..))")
    public void afterThrowing() {
        System.out.println("afterThrowing.........");
    }

    //环绕通知
    @Around(value = "execution(* com.clover.spring5.aopannotation.User.add(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕之前.........");
        //被增强的方法执行
        proceedingJoinPoint.proceed();
        System.out.println("环绕之后.........");
    }
}
~~~

5、相同的切入点抽取
~~~java 
//相同切入点抽取
@Pointcut(value = "execution(* com.clover.spring5.aopannotation.User.add(..))")
public void pointcut()
{

}

@Before(value = "pointcut()")
public void before() {//前置通知
	System.out.println("before......");
}
~~~

6、有多个增强类多同一个方法进行增强，设置增强类优先级
- 在增强类上面添加注解 @Order(数字类型值)，数字类型值越小优先级越高

~~~Java
@Component
@Aspect
@Order(1)
public class PersonProxy 
~~~ 

7、完全使用注解开发
- 创建配置类，不需要创建xml配置文件

~~~Java
@Configuration
@ComponentScan(basePackages = {"com.clover"})// 开启注解扫描
@EnableAspectJAutoProxy(proxyTargetClass = true)// 开启Aspect生成代理对象
public class configAop {
}
~~~

### 6.7、AOP 操作（AspectJ 配置文件）

1、创建两个类，增强类和被增强类，创建方法

2、在 spring配置文件中创建两个类对象

~~~xml
<!--创建对象-->
<bean id="book" class="com.clover.spring5.aopxml.Book"/>
<bean id="bookProxy" class="com.clover.spring5.aopxml.BookProxy"/>
~~~

3、在 spring 配置文件中配置切入点

~~~xml
<!--配置aop增强-->
<aop:config>
	<!--配置切入点-->
	<aop:pointcut id="p" expression="execution(* com.clover.spring5.aopxml.Book.buy(..))"/>

	<!--配置切面-->
	<aop:aspect ref="bookProxy">
		<!--增强作用在具体的方法上-->
		<aop:before method="before" pointcut-ref="p"/>
	</aop:aspect>
</aop:config>
~~~

## 7、JdbcTemplate(概念和准备)
1、什么是 JdbcTemplate
- Spring 框架对 JDBC 进行封装，使用 JdbcTemplate 方便实现对数据库操作

2、准备工作
- 引入相关 jar包

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211101214655.png)

- 在 spring配置文件配置数据库连接池
	~~~xml
	<!-- 数据库连接池 -->
	<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
		<property name="url" value="jdbc:mysql:///user_db" />
		<property name="username" value="root"/>
		<property name="password" value="xn123456"/>
		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
	</bean>
	~~~
- 配置 JdbcTemplate 对象，注入 DataSource

	~~~xml
	<!-- JdbcTemplate对象 -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<!--注入dataSource-->
		<property name="dataSource" ref="dataSource"/>
	</bean>

	<!-- 组件扫描 -->
    <context:component-scan base-package="com.clover"/>
	~~~
- 创建 service 类，创建 dao 类，在 dao 注入 jdbcTemplate 对象
	~~~Java
	@Service
	public class BookService {

		//注入dao
		@Autowired
		private BookDao bookDao;
	}
	~~~
	
	~~~Java
	@Repository
	public class BookDaoImpl implements BookDao {
		//注入JdbcTemplate
		@Autowired
		private JdbcTemplate jdbcTemplate;
	}
	~~~
	
### 7.1、JdbcTemplate 操作数据库（添加）

1、对应数据库创建实体类

~~~Java
public class Book {
    private int userId;
    private String username;
    private String userStatus;

    public int getUserId() {
        return userId;
    }

    public void setUserId(int userId) {
        this.userId = userId;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getUserStatus() {
        return userStatus;
    }

    public void setUserStatus(String userStatus) {
        this.userStatus = userStatus;
    }
}
~~~

2、编写 service 和 dao
- 在dao进行数据库添加操作
- 调用 JdbcTemplate 对象里面 update 方法实现添加操作
- 有两个参数
	- 第一个参数：sql 语句
	- 第二个参数：可变参数，设置sql语句值

		~~~Java
		@Repository
		public class BookDaoImpl implements BookDao {
			//注入JdbcTemplate
			@Autowired
			private JdbcTemplate jdbcTemplate;
			//添加的方法
			@Override
			public void add(Book book) {
				//1 创建sql语句
				String sql = "insert into t_book values(?,?,?)";
				//2 调用方法实现
				Object[] args = {book.getUserId(), book.getUsername(), book.getUserStatus()};
				int update = jdbcTemplate.update(sql,args);
				System.out.println(update);
			}
		}
		~~~

3、测试类

~~~Java
@Test
public void testJdbcTemplate() {
	ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
	BookService bookService = context.getBean("bookService", BookService.class);
	Book book = new Book();
	book.setUserId("1");
	book.setUsername("java");
	book.setUstatus("a");
	bookService.addBook(book);
}
~~~


### 7.2、JdbcTemplate 操作数据库（修改和删除）

~~~Java
//修改
@Override
public void update(Book book) {
	String sql = "update t_book set username=?,ustatus=? where user_id=?";
	Object[] args = {book.getUsername(), book.getUserStatus(),book.getUserId()};
	int update = jdbcTemplate.update(sql, args);
	System.out.println(update);
}

//删除
@Override
public void deleteBook(String id) {
	String sql = "delete from t_book where user_id=?";
	int update = jdbcTemplate.update(sql, id);
	System.out.println(update);
}
~~~

### 7.3、JdbcTemplate 操作数据库（查询返回某个值）
1、查询表里面有多少条记录，返回是某个值

2、使用 JdbcTemplate 实现查询返回某个值代码

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211101222134.png)
- 有两个参数
	- 第一个参数：sql 语句
	- 第二个参数：返回类型 Class

~~~Java
//查询表记录数
@Override
public int selectCount() {
	String sql = "select count(*) from t_book";
	Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
	return count;
}
~~~

### 7.4、JdbcTemplate 操作数据库（查询返回对象）
1、场景：查询图书详情

2、JdbcTemplate 实现查询返回对象

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102131616.png)

- 有三个参数
	- 第一个参数：sql 语句 
	- 第二个参数：RowMapper 是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装
	- 第三个参数：sql 语句值

~~~Java
//查询返回对象
@Override
public Book findBookInfo(String id) {
	String sql = "select * from t_book where user_id=?";
	//调用方法
	Book book = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>(Book.class), id);
	return book;
}
~~~

### 7.5、JdbcTemplate 操作数据库（查询返回集合）

1、场景：查询图书详情

2、JdbcTemplate 实现查询返回对象

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102132940.png)

- 有三个参数
	- 第一个参数：sql 语句 
	- 第二个参数：RowMapper 是接口，针对返回不同类型数据，使用这个接口里面实现类完成 数据封装
	- 第三个参数：sql 语句值

~~~Java
//查询返回集合
@Override
public List<Book> findAllBook() {
	String sql = "select * from t_book";
	//调用方法
	List<Book> bookList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
	return bookList;
}
~~~

### 7.6、JdbcTemplate 操作数据库（批量操作）

1、批量操作：操作表里面多条记录

2、JdbcTemplate 实现批量添加操作

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102133529.png)

- 有两个参数
	- 第一个参数：sql 语句 
	- 第二个参数：List集合，添加多条记录数据

~~~Java
//批量添加
@Override
public void batchAddBook(List<Object[]> batchArgs) {
	String sql = "insert into t_book values(?,?,?)";
	int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
	System.out.println(Arrays.toString(ints)); 
}

//批量添加测试
List<Object[]> batchArgs = new ArrayList<>(); 
Object[] o1 = {"3","java","a"}; 
Object[] o2 = {"4","c++","b"}; 
Object[] o3 = {"5","MySQL","c"}; 
batchArgs.add(o1); batchArgs.add(o2); batchArgs.add(o3);
//调用批量添加
bookService.batchAdd(batchArgs);
~~~

3、JdbcTemplate 实现批量修改操作

~~~Java
//批量修改
@Override
public void batchUpdateBook(List<Object[]> batchArgs) {
	String sql = "update t_book set username=?,ustatus=? where user_id=?";
	int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
	System.out.println(Arrays.toString(ints)); 
}
//批量修改
List<Object[]> batchArgs = new ArrayList<>(); 
Object[] o1 = {"java0909","a3","3"}; 
Object[] o2 = {"c++1010","b4","4"}; 
Object[] o3 = {"MySQL1111","c5","5"}; 
batchArgs.add(o1); batchArgs.add(o2); batchArgs.add(o3);
//调用方法实现批量修改
bookService.batchUpdate(batchArgs);
~~~

4、JdbcTemplate 实现批量删除操作

~~~Java
//批量删除
@Override
public void batchDeleteBook(List<Object[]> batchArgs) {
	String sql = "delete from t_book where user_id=?";
	int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
	System.out.println(Arrays.toString(ints)); 
}
//批量删除
List<Object[]> batchArgs = new ArrayList<>();
Object[] o1 = {"3"}; 
Object[] o2 = {"4"}; 
batchArgs.add(o1); batchArgs.add(o2);
//调用方法实现批量删除
bookService.batchDelete(batchArgs);
~~~ 


## 8、事务概念

1、什么事务
- 事务是数据库操作最基本单元，逻辑上一组操作，要么都成功，如果有一个失败所有操作都失败
- 典型场景：银行转账
- lucy 转账 100 元 给 mary 
- lucy 少 100，mary 多 100

**2、事务四个特性（ACID）**
- `原子性`：要么都成功，要么都失败
- `一致性`：操作前后总量是一致的
- `隔离性`：多事务之间的操作无影响，比如两个人操作同一记录，过程间是不会产生影响的
- `持久性`：数据操作完成后表中数据是真正改变了

### 8.1、事务操作（搭建事务操作环境）

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102154956.png)

1、创建数据库表，添加记录

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102155023.png)

2、创建 service，搭建 dao，完成对象创建和注入关系

- service 注入 dao，在 dao 注入 JdbcTemplate，在 JdbcTemplate 注入 DataSource

~~~Java
@Service
public class UserService {
	//注入dao
	@Autowired
	private UserDao userDao;
}
@Repository
public class UserDaoImpl implements UserDao {
	@Autowired
	private JdbcTemplate jdbcTemplate;
}
~~~

3、在 dao创建两个方法：多钱和少钱的方法，在 service创建方法（转账的方法）

~~~Java
package com.clover.spring5.dao;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class UserDaoImpl implements UserDao{

    @Autowired
    private JdbcTemplate jdbcTemplate;

    //lucy转账100给mary
    // 少钱
    @Override
    public void reduceMoney() {
        String sql = "update t_money set money=money-? where username=?";
        jdbcTemplate.update(sql,100,"lucy");
    }

    //多钱
    @Override
    public void addMoney() {
        String sql = "update t_money set money=money+? where username=?";
        jdbcTemplate.update(sql,100,"mary");
    }
}

@Service
public class UserService {

    @Autowired
    private UserDao userDao;

    //转账的方法
    public void accountMoney() {
        //lucy少100
        userDao.reduceMoney();
        //mary多100
        userDao.addMoney();
    }
}
~~~

4、上面代码，如果正常执行没有问题的，但是如果代码执行过程中出现异常，有问题

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102160253.png)

- 上面问题如何解决呢？
	- **使用事务进行解决**
- 事务操作过程

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102160533.png)

### 8.2、事务操作（Spring 事务管理介绍）

1、事务添加到 JavaEE 三层结构里面 Service 层（业务逻辑层）

2、在 Spring 进行事务管理操作
- 有两种方式
	- **编程式事务管理**
	- **声明式事务管理（常用）**

3、声明式事务管理
- **基于注解方式（常用）**
- 基于xml配置文件方式 

4、在 Spring进行声明式事务管理，**底层使用 AOP原理**

5、Spring 事务管理 API
- 提供一个接口，代表事务管理器，这个接口针对不同的框架提供不同的实现类

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102161040.png)

### 8.3、事务操作（注解声明式事务管理）

1、在 spring配置文件配置事务管理器

~~~xml
<!--创建事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<!--注入数据源-->
	<property name="dataSource" ref="dataSource"/>
</bean>
~~~


2、在 spring 配置文件，开启事务注解 
- 在 spring配置文件引入名称空间 tx

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/tx http://www.springframework.org/schema/aop/spring-tx.xsd">
~~~
- 开启事务注解

~~~xml
<!--开启事务注解-->
<tx:annotation-driven transaction-manager="transactionManager"/>
~~~

3、在 service 类上面（或者 service 类里面方法上面）添加事务注解 
- `@Transactional`，这个注解添加到类上面，也可以添加方法上面 
- 如果把这个注解添加类上面，这个类里面所有的方法都添加事务
- 如果把这个注解添加方法上面，为这个方法添加事务

### 8.4、事务操作（声明式事务管理参数配置）

1、在 service 类上面添加注解@Transactional，在这个注解里面可以配置事务相关参数

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102163629.png)

**2、propagation：事务传播行为**
- 多事务方法之间进行调用，这个过程中事务是如何进行管理的

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102163350.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102163417.png)

**3、ioslation：事务隔离级别**
- 事务有特性成为隔离性，多事务操作之间不会产生影响。不考虑隔离性产生很多问题
- 有三个读问题：**脏读、不可重复读、虚（幻）读**
- `脏读`：一个未提交事务读取到另一个未提交事务的数据
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102164658.png)
- `不可重复读`：一个未提交事务读取到另一提交事务`修改`数据

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102165350.png)

- `虚读`：一个未提交事务读取到另一提交事务`添加`数据
- 解决：**通过设置事务隔离级别**，解决读问题

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102165444.png)

**4、timeout：超时时间**

- 事务需要在一定时间内进行提交，如果不提交进行回滚
- 默认值是 -1 ，设置时间以`秒单位`进行计算

**5、readOnly：是否只读**
- 读：查询操作，写：添加修改删除操作
- readOnly 默认值 false，表示可以查询，也可以添加修改删除操作 
- 设置 readOnly 值是 true，设置成 true 之后，只能查询

**6、rollbackFor：回滚**
- 设置出现哪些异常进行事务回滚

**7、noRollbackFor：不回滚**

- 设置出现哪些异常不进行事务回滚

### 8.5、事务操作（XML声明式事务管理）

1、在 spring 配置文件中进行配置 
- 第一步 配置事务管理器
- 第二步 配置通知
- 第三步 配置切入点和切面

~~~xml
<!--1 创建事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<!--注入数据源-->
	<property name="dataSource" ref="dataSource"/>
</bean>

<!--2 配置通知-->
<tx:advice id="txadvice">
	<!--配置事务参数-->
	<tx:attributes>
		<!--指定哪种规则在方法上面添加事务-->
		<tx:method name="accountMoney"/>
	</tx:attributes>
</tx:advice>

<!--3 配置切入点和切面-->
<aop:config>
	<!--配置切入点-->
	<aop:pointcut id="pt" expression="execution(* com.clover.spring5.service.UserService.*(..))"/>

	<!--配置切面-->
	<aop:advisor advice-ref="txadvice" pointcut-ref="pt"/>
</aop:config>
~~~

### 8.6、事务操作（完全注解声明式事务管理）

1、创建配置类，使用配置类替代xml配置文件

~~~Java
@Configuration//配置类
@ComponentScan(basePackages = "com.clover")//组件扫描
@EnableTransactionManagement //开启事务
public class TxConfig {

    //创建数据库连接池
    @Bean
    public DruidDataSource getDruidDataSource()
    {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl("jdbc:mysql:///user_db");
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUsername("root");
        dataSource.setPassword("xn123456");
        return dataSource;
    }

    //创建JdbcTemplate对象
    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource)
    {
        //到ioc容器中根据类型找到dataSource
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        //注入dataSource
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

    //创建事务管理器
    @Bean
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource)
    {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
}
~~~

## 9、Spring5 框架新功能

1、整个 Spring5 框架的代码基于 Java8，运行时兼容 JDK9，许多不建议使用的类和方 法在代码库中删除

2、Spring 5.0 框架自带了通用的日志封装 
- Spring5 已经移除 Log4jConfigListener，官方建议使用 Log4j2 
- Spring5 框架整合 Log4j2 
	- 第一步 引入jar包
	- 第二步 创建 log4j2.xml 配置文件
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->
<!--Configuration后面的status用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，可以看到log4j2内部各种详细输出-->
<configuration status="INFO">
    <!--先定义所有的appender-->
    <appenders>
        <!--输出日志信息到控制台-->
        <console name="Console" target="SYSTEM_OUT">
            <!--控制日志输出的格式-->
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </console>
    </appenders>
    <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
    <!--root：用于指定项目的根日志，如果没有单独指定Logger，则会使用root作为默认的日志输出-->
    <loggers>
        <root level="info">
            <appender-ref ref="Console"/>
        </root>
    </loggers>
</configuration>
~~~

3、Spring5 框架核心容器支持@Nullable 注解 
- `@Nullable 注解`可以使用在方法上面，属性上面，参数上面，表示方法返回可以为空，属性值可以为空，参数值可以为空
- 注解用在方法上面，方法返回值可以为空
- 注解使用在方法参数里面，方法参数可以为空
- 注解使用在属性上面，属性值可以为空

4、Spring5 核心容器支持函数式风格 GenericApplicationContext

~~~Java
//函数式风格创建对象，交给spring进行管理
@Test
public void testGenericApplicationContext() {
	//1 创建GenericApplicationContext对象
	GenericApplicationContext context = new GenericApplicationContext();
	//2 调用context的方法对象注册
	context.refresh(); context.registerBean("user1",User.class,() -> new User());
	//3 获取在spring注册的对象
//        User user = (User)context.getBean("com.clover.spring5.Test.User");
	User user = (User)context.getBean("user1");
	System.out.println(user);
}
~~~

5、Spring5 支持整合 JUnit5 
- 整合 JUnit4
	- 第一步 引入 Spring相关针对测试依赖
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102183207.png)
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102183224.png)
	- 第二步 创建测试类，使用注解方式完成
		 ~~~Java
		@RunWith(SpringJUnit4ClassRunner.class)//指定单元测试框架
		@ContextConfiguration("classpath:bean1.xml") //加载配置文件
		public class JTest4 {

			@Autowired
			private UserService userService;

			@Test
			public void test1() {
				userService.accountMoney();
			}
		}
		~~~

- Spring5 整合 JUnit5 
	- 第一步 引入 JUnit5 的 jar 包
	- 第二步 创建测试类，使用注解完成

		~~~Java
		import com.clover.spring5.service.UserService;
		import org.junit.jupiter.api.Test;
		import org.junit.jupiter.api.extension.ExtendWith;
		import org.springframework.beans.factory.annotation.Autowired;
		import org.springframework.test.context.ContextConfiguration;
		import org.springframework.test.context.junit.jupiter.SpringExtension;

		@ExtendWith(SpringExtension.class)
		@ContextConfiguration("classpath:bean1.xml")
		public class JTest5 {

			@Autowired
			private UserService userService;
			@Test
			public void test1() {
				userService.accountMoney();
			}
		}
		~~~
		
- 使用一个复合注解替代上面两个注解完成整合

~~~Java
@SpringJUnitConfig(locations = "classpath:bean1.xml")
public class JTest5 {
	@Autowired
	private UserService userService;
	@Test
	public void test1() {
		userService.accountMoney();
	}
}
~~~ 

## 10、Spring5 框架新功能（Webflux）

1、SpringWebflux 介绍 
- 是 Spring5 添加新的模块，用于 web 开发的，功能和 SpringMVC 类似的，Webflux 使用当前一种比较流行**响应式编程**出现的框架
- 使用传统 web 框架，比如 SpringMVC，这些基于 Servlet 容器，Webflux 是一种**异步非阻塞**的框架，异步非阻塞的框架在 Servlet3.1 以后才支持，**核心是基于 Reactor 的相关 API 实现的**
- 解释什么是异步非阻塞
	- 异步和同步；非阻塞和阻塞
	- 上面都是针对对象不一样
	- **异步和同步针对`调用者`**，调用者发送请求，如果等着对方回应之后才去做其他事情就是同步，如果发送请求之后不等着对方回应就去做其他事情就是异步
	- **阻塞和非阻塞针对`被调用者`**，被调用者收到请求之后，做完请求任务之后才给出反馈就是阻塞，受到请求之后马上给出反馈然后再去做事情就是非阻塞
- Webflux 特点
	- 第一 异步非阻塞式：在有限资源下，提高系统吞吐量和伸缩性，以Reactor为基础实现响应式编程
	- 第二 函数式编程：Spring5 框架基于 java8，Webflux 使用 Java8函数式编程方式实现路由请求
- 比较 SpringMVC

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211102200845.png)

- 第一 两个框架都可以使用注解方式，都运行在Tomcat等容器中
- 第二 SpringMVC采用命令式编程，Webflux采用异步响应式编程 

2、响应式编程（Java 实现） 
- 什么是响应式编程
	- 响应式编程是一种面向数据流和变化传播的编程范式。这意味着可以在编程语言中很方便地表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播
	- 电子表格程序就是响应式编程的一个例子。单元格可以包含字面值或类似"=B1+C1"的公式，而包含公式的单元格的值会依据其他单元格的值的变化而变化
- Java8及其之前版本
	- 提供的**观察者模式**两个类 `Observer` 和 `Observable`

~~~Java
public class ObserverDemo extends Observable {
    public static void main(String[] args) {

        ObserverDemo observerDemo = new ObserverDemo();
        //添加观察者
        observerDemo.addObserver((o,arg)->{
            System.out.println("发生变化");
        });
        observerDemo.addObserver((o,arg)->{
            System.out.println("手动被观察者通知，准备改变");
        });
        observerDemo.setChanged(); //监控数据变化
        observerDemo.notifyObservers(); //通知

    }
}
~~~

3、响应式编程（Reactor 实现）
- 响应式编程操作中，Reactor 是满足 Reactive 规范框架 
- Reactor 有两个核心类，Mono 和 Flux，这两个类实现接口 Publisher，提供丰富操作符。Flux对象实现发布者，返回 N个元素；Mono实现发布者，返回 0或者1个元素 
- Flux和 Mono都是数据流的发布者，使用 Flux和 Mono 都可以发出三种数据信号： **元素值，错误信号，完成信号**，错误信号和完成信号都代表终止信号，终止信号用于告诉订阅者数据流结束了，错误信号终止数据流同时把错误信息传递给订阅者
- 代码演示 Flux 和 Mono
