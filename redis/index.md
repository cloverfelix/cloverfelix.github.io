# Redis





# NoSQL概述

> 1、单机Mysql的年代

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210619113815.png)

90年代，一个基本的网站访问量不会太大，单个数据库完全足够

那个时候，更多的去使用静态网页HTML~ 服务器根本就没有太大的压力！

思考，这种情况下：整个网站的瓶颈是什么？

	1、数据量如果太大了，一个机器放不下了！

	2、数据的索引，一个机器内存也存放不下

	3、访问量（读写混合），一个服务器承受不了

只要开始出现以上的三种情况之一，那么你就必须要晋级！

> Memcached（缓存） + MySQL + 垂直拆分（读写分离）

网站80%的情况下都是在读，每次都要去查询数据库的话就十分的麻烦！所以说我们希望减轻数据的压力

我们可以使用缓存来保证效率。

发展过程：优化数据结构和索引--->文件缓存（IO）--->Memcached（当时最热门的技术！）

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210619114615.png)

> 3、分库分表 + 水平拆分 + MySQL集群

技术和业务在发展的同时，对人的要求也越来越高

**本质：数据库（读、写）**

早些年MyISAM：表锁（有100万条数据，查询`张三`就会将**整个表锁起来**，等查询完后才能进行下一条查

询，十分影响效率！高并发下就会出现严重的锁问题。

转战Innodb：行锁

慢慢的就开始使用分库分表来解决写的压力！MySQL在那个年代推出了表分区！这个并没有多少公司使用

MySQL的集群，很好满足那个年代的所有需求

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210619120330.png)

> 4、如今最近的年代

MySQL等关系型数据库就不够用了！数据量很多，变化很快！

MySQL有的时候它用来存放一些比较大的文件，博客，图片！数据库表很大，效率就低了！如果有一种数

据库来专门处理这种数据，MySQL压力就变得十分小（研究如何处理这些问题！）大数据的IO压力下，表

几乎没法更改

 > 目前一个基本的互联网项目

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210619120720.png)

> 为什么要用NoSQL！

用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志等等爆发式增长！

这时候我们就需要使用NoSQL数据库，NoSQL可以很好的处理以上的情况！

## 什么是NoSQL

> NoSQL

NoSQL = Not Only SQL （不仅仅是SQL）

关系型数据库：表格，行，列

泛指非关系型数据库的，随着web2.0互联网的诞生！传统的关系型数据库很难对付web2.0时代！尤其是

超大规模的高并发社区！暴露出来很多难以克服的问题，NoSQL在当今大数据环境下发展的十分迅速，

Redis是发展的最快的，而且是我们当下必须要掌握的一个技术！

很多的数据类型用户的个人信息，社交网络，地理位置。这些数据类型的存储不需要一个固定的格式！不

需要多余的操作就可以横向扩展！ Map<String，Object>**使用键值对来控制**

> NoSQL特点

解耦！

1、方便扩展（数据之间没有关系，很好扩展！）

2、大数据量高性能（Redis一秒写8万次，读取11万，NoSQL的缓存是**记录级**是一种细粒度的缓存，性能

会比较高）

3、数据类型是多样型的！（不需要事先设计数据库！随取随用！如果数据量十分大的表，很多人无法设计了）

4、传统的 RDBMS（关系型数据库） 和 NoSQL

~~~Java
传统的RDBMS
- 结构化组织
- SQL
- 数据和关系都存在单独的表中
- 数据操作，数据定义语言
- 严格的一致性
- 基础的事务
~~~

~~~Java
NoSQL
- 不仅仅是数据
- 没有固定的查询语言
- 键值对存储，列存储（HBase），文档存储（MongoDb），图形数据库（社交关系）
- 最终一致性
- CAP定理和BASE　　（异地存活！整个服务器宕机了，但是不影响其它的地方正常运行）
- 高新能，高可用，高可扩
~~~

>了解3V+3高

大数据时代的3V：主要是**描述问题的**

1、海量Volume

2、多样Variety

3、实时Velocity

大数据时代的3高：主要是**对程序的要求**

1、高并发

2、高可扩

3、高新能

真正在公司中的实践：NoSQL + RDBMS一起使用才是最强的

## NoSQL的四大分类

**KV键值对：** 

- 新浪：**Redis**

- 美团：Redis + Tair

-  阿里、百度：Redis + memcache

**文档型数据库（bson格式 和 json一样）**

- **MongoDB**（一般必须掌握）

	- MongoDB是一个基于分布式文件存储的数据库，C++编写，主要用来处理大量的文档！

	-  MongoDB是一个介于关系型数据库和非关系型数据库的中间产品！MongoDB是NoSQL中功能最丰富，最像关系型数据库的！

- ConchDB（不做了解、国外的）

**列存储数据库**

- **HBase**

- 分布式文件系统

**图关系数据库**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620164050.png)

- 它不是存图形的，放的是关系，比如：朋友圈社交网络，广告推荐！

- **Neo4j**，InfoGrid；

# Redis入门

## 概述

> Redis是什么？

Redis（**R**emote **D**ictionary **S**erver），即远程字典服务

是一个开源的使用ANSIC语言编写，支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，

并提供多种语言的API

redis会周期型的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且再次基础上实现了

master-slave（主从）同步。

> Redis能干什么？

1、 内存存储、持久化，内存中是断电即失，所以说持久化很重要。**（RDB，AOF）**

2、效率高，可以用于高速缓存

3、发布订阅系统

4、地图信息分析

5、计时器、计数器（浏览量！）

## Windows安装

1、下载安装包：https://github.com/microsoftarchive/redis/releases/tag/win-3.2.100

2、下载完毕得到压缩包

3、解压到自己环境目录下！Redis十分的小

4、开启Redis，双击运行即可！

5、使用redis客户端来连接redis

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620171315.png)

记住一句话，Windows下使用确实简单，但是Redis推荐我们使用Linux去使用

## Linux安装包

1、下载安装包

2、解压Redis安装包！程序一般放在/opt下

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620172602.png)

3、进入解压后的文件，可以看到redis的配置文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620172655.png)

4、基本的环境安装

~~~bash
yum install gcc-c++

make

make install
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620201639.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620201706.png)

5、Redis的默认安装路径`usr/local/bin`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620201838.png)

6、将redis配置文件。复制到我们当前的目录下

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620202440.png)

7、redis默认不是后台启动的，修改配置文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620202735.png)

8、启动redis服务

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620203217.png)

9、使用redis-cli进行连接测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620203512.png)

10、查看redis的进程是否开启

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620203640.png)

11、如何关闭redis服务`shutdown`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620203822.png)

12、再次查看进程是否存在

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620203850.png)

## 测试性能

**redis-benchmark**是一个压力测试工具！

官方自带的性能测试工具！

redis-benchmark +命令参数！

可选参数如下：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620204055.png)

~~~bash
# 测试：100个并发连接  100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620204812.png)

如何查看这些分析呢？

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620205143.png)

## 基础的知识

redis默认有16个数据库

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620205816.png)

默认使用的是第0个

可以使用select进行切换数据库！**从0-15**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620210052.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210620210257.png)

清除当前数据库`flushdb`

清除全部数据库的内容`flushall`

~~~bash
127.0.0.1:6379[3]> keys *          # 查看数据库所有的key
1) "name"

127.0.0.1:6379[3]> flushdb  	# 清除当前数据库
OK
127.0.0.1:6379[3]> keys *
(empty array)
~~~ 

> Redis是单线程的！

明白Redis是很快的，官方表示，Redis是基于内存操作的，CPU不是Redis性能瓶颈，Redis的瓶颈是根据

机器的内存和网络带宽，既然可以使用单线程来实现，就使用单线程了！所以就使用单线程了！

Redis是C语言写的，官方提供的数据为 100000+QPS，完全不必同样是使用key-value的Memecache差！

**Redis为什么是单线程还这么快？查文献巩固一下**

1、误区1：高性能的服务器一定是多线程的？

2、误区2：多线程（CPU上下文会切换）一定比单线程效率高！

核心：redis是将所有的数据据全部放在内存中的，所以说使用单线程去操作效率就是最高的,多线程（CPU

上下文会切换：这是一个耗时的操作！！！），对于内存系统来说，如果没有上下文切换效率就是最高

的！多次读写都是在一个CPU上的，在内存情况下，这个就是最佳的方案！！


# 五大数据类型 

~~~bash
127.0.0.1:6379> keys *					# 查看所有的key
1) "name"
127.0.0.1:6379> set age 1
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> exists name				# 判断当前key是否存在
(integer) 1
127.0.0.1:6379> exists name1
(integer) 0
127.0.0.1:6379> move name 1			# 移出当前key 
(integer) 1
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> set name felix
OK
127.0.0.1:6379> KEYS *
1) "age"
2) "name"
127.0.0.1:6379> get name
"felix"
127.0.0.1:6379> move name 10
(integer) 1
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> set name felix
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> expire name 10			# 设置key的过期时间，单位是秒
(integer) 1
127.0.0.1:6379> ttl name						# 查看当前key 的剩余时间
(integer) 6
127.0.0.1:6379> ttl name
(integer) 4
127.0.0.1:6379> ttl name
(integer) 4
127.0.0.1:6379> ttl name
(integer) 3
127.0.0.1:6379> ttl name
(integer) 2
127.0.0.1:6379> ttl name
(integer) 1
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> get name
(nil)
127.0.0.1:6379> type name					# 查看当前key的一个类型
string
127.0.0.1:6379> type age
string
~~~

## String（字符串）

~~~bash
127.0.0.1:6379> set key1 clover									# 设置值
OK
127.0.0.1:6379> get key1											# 获得值
"clover"
127.0.0.1:6379> keys *												# 获得所有的key
1) "key1"
127.0.0.1:6379> EXISTS key1									# 判断某一个key是否存在
(integer) 1
127.0.0.1:6379> APPEND key1 "felix"						# 追加字符串，如果当前key不存在，就相当于setkey
(integer) 11
127.0.0.1:6379> get key1
"cloverfelix"
127.0.0.1:6379> STRLEN key1									# 获取字符串的长度！
(integer) 11
127.0.0.1:6379> APPEND key1 ",wei"
(integer) 15
127.0.0.1:6379> get key1
"cloverfelix,wei"
127.0.0.1:6379> STRLEN key1
(integer) 15

127.0.0.1:6379> set views 0											# 初始浏览量为0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views											# 自增1   浏览量变为1
(integer) 1
127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> get views
"2"
127.0.0.1:6379> decr views											# 自减1 浏览量减一
(integer) 1
127.0.0.1:6379> decr views
(integer) 0
127.0.0.1:6379> decr views
(integer) -1
127.0.0.1:6379> get views
"-1"
127.0.0.1:6379> INCRBY views 10								# 可以设置步长，指定增量
(integer) 9
127.0.0.1:6379> DECRBY views 10
(integer) -1
127.0.0.1:6379> get views
"-1"
127.0.0.1:6379> 

# 字符串范围  range

127.0.0.1:6379> set key1 "hello,clover"					# 设置key1的值
OK
127.0.0.1:6379> get key1
"hello,clover"
127.0.0.1:6379> GETRANGE key1 0 4					# 截取字符串[0，4]
"hello"
127.0.0.1:6379> GETRANGE key1 0 -1					# 获取全部的字符串 和 get key是一样的
"hello,clover"

# 替换！	
127.0.0.1:6379> set key2 abcdefg
OK
127.0.0.1:6379> get key
(nil)
127.0.0.1:6379> get key2
"abcdefg"
127.0.0.1:6379> SETRANGE key2 1 xxx						# 替换指定位置开始的字符串
(integer) 7
127.0.0.1:6379> get key2
"axxxefg"

# setex （set with expire）  # 设置过期时间
# setnx （set if not exist）  # 不存在再设置（在分布式锁中会常常使用！）

127.0.0.1:6379> setex key3 30 cloverfelix				# 设置key3的值为 cloverfelix，30秒后过期
OK
127.0.0.1:6379> ttl key3
(integer) 21
127.0.0.1:6379> setnx mykey "redis"						# 如果mykey 不存在，创建mykey
(integer) 1
127.0.0.1:6379> keys *
1) "key3"
2) "key2"
3) "mykey"
4) "key1"
127.0.0.1:6379> ttl key3
(integer) -2
127.0.0.1:6379> setnx mykey "mongoDB"			  如果mykey 存在，创建失败！
(integer) 0
127.0.0.1:6379> get mykey
"redis"

# mset
# mget

127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3				# 同时设置多个值
OK
127.0.0.1:6379> keys *
1) "k1"
2) "k3"
3) "k2"
127.0.0.1:6379> mget k1 k2 k3							# 同时获取
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v2 k4 v4					# msetnx 是一个原子性的操作，要么一起成功，要么一起失败！
(integer) 0
127.0.0.1:6379> get k4
(nil)
127.0.0.1:6379> 

# 对象
set user:1 {name:zhangsan,age:3}

# 这里的key是一个巧妙的设计：user:{id}:{filed属性}， 如此设计在Redis中完全是OK的
# 那么在设置文章的浏览量的时候就可以指定多篇文章的浏览量

127.0.0.1:6379> mset user:1:name zhangsan user:1:age 3
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "3"

getset  # 先get然后在set
127.0.0.1:6379> getset db redis								# 如果不存在值，则返回为null
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db mongodb					# 如果存在值，获取原来的值，并设置新的值
"redis"
127.0.0.1:6379> get db
"mongodb"
~~~

String类似的使用场景：value除了是我们的字符串还可以是我们的数字！

## List

在redis乱码，我们可以把list玩成栈、队列、阻塞队列！

所有的list命令都是用`l`开头的

~~~bash
127.0.0.1:6379> LPUSH list one			# 将一个或多个值插入列表的头部（从左边插入进去）
(integer) 1
127.0.0.1:6379> LPUSH list two
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> keys *
1) "list"
127.0.0.1:6379> LRANGE list 0 -1	# 获取值是从前往后的，即先插入进来的值放在最后，最后插入的值放在最前面，下标为0
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> LRANGE list 0 1   # 通过区间获取具体的值
1) "three"
2) "two"
127.0.0.1:6379> RPUSH list right			# 将一个或多个值插入列表的头部（从右边插入进去）
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
4) "right"
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210621210034.png)

~~~bash
# LPOP
# RPOP
127.0.0.1:6379> lpop list 			# 移出list的第一个元素
"three"
127.0.0.1:6379> rpop list			# 移出list的最后一个元素
"right"
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"

# index

127.0.0.1:6379> lindex list 0		# 通过下标获得list中的某一个值！
"two"
127.0.0.1:6379> lindex list 1
"one"

# llen

127.0.0.1:6379> lpush list one
(integer) 1
127.0.0.1:6379> lpush list two
(integer) 2
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> llen list   			# 返回列表的长度
(integer) 3

# 移除指定的值！

127.0.0.1:6379> lrem list 1 three							# 移出list集合中指定个数的value，精确匹配
(integer) 1
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> lpush list three
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "three"
3) "two"
4) "one"
127.0.0.1:6379> lrem list 2 three
(integer) 2
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"

#  list截断

127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "hello1"
(integer) 2
127.0.0.1:6379> rpush mylist "hello2"
(integer) 3
127.0.0.1:6379> rpush mylist "hello3"
(integer) 4
127.0.0.1:6379> LRANGE mylist 0 -1
1) "hello"
2) "hello1"
3) "hello2"
4) "hello3"
127.0.0.1:6379> ltrim mylist 1 2						# 通过下标截取指定的长度，这个list已经被改变了，截断了只剩下截取的元素
OK
127.0.0.1:6379> LRANGE mylist 0 -1							
1) "hello1"
2) "hello2"
127.0.0.1:6379> 

# rpoplpush   移出列表的最后一个元素，将它移动到新的列表中！

127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "hello1"
(integer) 2
127.0.0.1:6379> rpush mylist "hello2"
(integer) 3
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "hello1"
3) "hello2"
127.0.0.1:6379> rpoplpush mylist myotherlist		# 移出右边的最后一个元素，加入到新的list集合中去
"hello2"
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "hello1"
127.0.0.1:6379> lrange myotherlist 0 -1
1) "hello2"
127.0.0.1:6379> 

# lset  将列表中指定下标的值替换为另外一个值，相当于更新操作

127.0.0.1:6379> exists list				# 判断这个表是否存在
(integer) 0		
127.0.0.1:6379> lset list 0 item			# 如果不存在列表我们去更新就会报错
(error) ERR no such key
127.0.0.1:6379> lpush list value1
(integer) 1
127.0.0.1:6379> lrange list 0 0
1) "value1"
127.0.0.1:6379> lset list 0 item			# 如果存在，更新当前下标的值
OK
127.0.0.1:6379> lrange list 0 0
1) "item"
127.0.0.1:6379> lset list 1 test			# 如果不存在，则会报错！
(error) ERR index out of range

# linsert  将某个具体的value插入到列表中的某个元素的前面或者后面！

127.0.0.1:6379> rpush mylist hello
(integer) 1
127.0.0.1:6379> rpush mylist world
(integer) 2
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "world"
127.0.0.1:6379> linsert mylist before world other
(integer) 3
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "other"
3) "world"
127.0.0.1:6379> linsert mylist after world new
(integer) 4
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "other"
3) "world"
4) "new"
~~~

> 小结

-  它实际上是一个链表，可以在节点前后插入，left，right都可以插入值

- 如果key不存在，创建新的链表

- 如果key存在，新增内容

- 如果移出了所有值，空链表，也代表不存在！

- 在两边插入或者改动值，效率最高！中间元素，相对来说效率会低一点！

可以当作**消息队列（Lpush，Rpop）**，也可以作为**栈（Lpush，Lpop）**
