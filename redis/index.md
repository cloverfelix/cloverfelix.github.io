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

1、[下载安装包](https://github.com/microsoftarchive/redis/releases/tag/win-3.2.100)

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

## Set（集合）

**set中的值是不能重复的！且插入值是随机分布的，并不是有序集合** 

~~~bash
127.0.0.1:6379> sadd myset "hello"					# sadd => set add set集合中添加元素
(integer) 1
127.0.0.1:6379> sadd myset "clover"
(integer) 1
127.0.0.1:6379> sadd myset "cloverfelix"
(integer) 1
127.0.0.1:6379> SMEMBERS myset					 # smembers => set members 查看指定set的所有值
1) "hello"
2) "clover"
3) "cloverfelix"
127.0.0.1:6379> sismember myset "felix"			# sismember => set exist member 判断某一个值是不是在set集合中
(integer) 0
127.0.0.1:6379> sismember myset "clover"	  # 存在集合中就返回0，不存在就返回值为1
(integer) 1

######################################
127.0.0.1:6379> scard myset								# 获取set集合中元素的个数
(integer) 3

#####################################
srem

127.0.0.1:6379> srem myset "hello"				   # 移除set集合中的指定的元素
(integer) 1
127.0.0.1:6379> scard myset
(integer) 2
127.0.0.1:6379> SMEMBERS myset
1) "clover"
2) "cloverfelix"

#####################################
set 是无序不重复集合。抽随机！

127.0.0.1:6379> SRANDMEMBER myset		# 随机抽选出一个元素
"felix"
127.0.0.1:6379> SRANDMEMBER myset
"clover"
127.0.0.1:6379> SRANDMEMBER myset
"test"
127.0.0.1:6379> SRANDMEMBER myset
"clover"
127.0.0.1:6379> SRANDMEMBER myset
"felix"	
127.0.0.1:6379> SRANDMEMBER myset 2		# 随机抽取指定个数的元素
1) "felix"
2) "test"
127.0.0.1:6379> SRANDMEMBER myset 2
1) "cloverfelix"
2) "test"
127.0.0.1:6379> SRANDMEMBER myset 2
1) "felix"
2) "test"

#####################################
删除指定的key，随机删除key

127.0.0.1:6379> SMEMBERS myset
1) "clover"
2) "test"
3) "felix"
4) "cloverfelix"
127.0.0.1:6379> spop myset					# 随机删除一些set集合中的元素
"test"
127.0.0.1:6379> spop myset
"clover"
127.0.0.1:6379> spop myset
"cloverfelix"
127.0.0.1:6379> SMEMBERS myset
1) "felix"

#####################################
将一个指定的值，移动到另外一个set集合中！
127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "world"
(integer) 1
127.0.0.1:6379> sadd myset "clover"
(integer) 1
127.0.0.1:6379> sadd myset2 "felix"
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "hello"
2) "clover"
3) "world"
127.0.0.1:6379> SMOVE myset myset2 "clover"    # 将一个指定的值，移动到另外一个set集合中！，第一个参数是源地址，第二个参数是目的地址
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "hello"
2) "world"
127.0.0.1:6379> SMEMBERS myset2
1) "clover"
2) "felix"
127.0.0.1:6379> 

#####################################
微博，B站，公共关注（并集）
数字集合类：
	- 差集
	- 交集
	- 并集
	
127.0.0.1:6379> sadd key1 a
(integer) 1
127.0.0.1:6379> sadd key1 b
(integer) 1
127.0.0.1:6379> sadd key1 c
(integer) 1
127.0.0.1:6379> sadd key2 c
(integer) 1
127.0.0.1:6379> sadd key2 d
(integer) 1
127.0.0.1:6379> sadd key2 e
(integer) 1
127.0.0.1:6379> SDIFF key1 key2						# 差集
1) "a"
2) "b"
127.0.0.1:6379> SINTER key1 key2				  # 交集
1) "c"
127.0.0.1:6379> SUNION key1 key2			    # 并集
1) "c"
2) "b"
3) "a"
4) "d"
5) "e"
127.0.0.1:6379> 
~~~

## Hash（哈希）

**Hash插入值是顺序插入，并且是可重复的！**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210708104832.png)

可以将其想象成一个Map集合！之前我们用的都是`Key-Value`，而我们这里是将`Value`换成`Map`，也就是

`key-map`的形式，Map中存放的又是一个`key-value`，所以这个时候这个值是一个`Map`集合

Hash本质和String类型没有太大区别，还是一个简单的key-value

~~~bash
127.0.0.1:6379> hset myhash test1 felix							# set一个具体的key-value
OK
127.0.0.1:6379> hset myhash test2 ryan
OK
127.0.0.1:6379> hget myhash test1								# 获取一个字段值
"felix"
127.0.0.1:6379> hget myhash test2
"ryan"
127.0.0.1:6379> hmset myhash test1 clover test2 cosy		# set多个具体的key-value
OK
127.0.0.1:6379> hmget myhash test1 test2							# 获取多个字段值
1) "clover"
2) "cosy"
127.0.0.1:6379> hgetall myhash									# 获取全部数据，展示是key-value形式
1) "test1"
2) "clover"
3) "test2"
4) "cosy"
127.0.0.1:6379> hdel myhash test2							# 删除指定key字段！对应的value值也就消失了！
(integer) 1
127.0.0.1:6379> hgetall myhash
1) "test1"
2) "clover"
127.0.0.1:6379>
########################################

hlen  获取hash的长度
127.0.0.1:6379> hmset myhash test2 felix
OK
127.0.0.1:6379> HGETALL myhash
1) "test1"
2) "clover"
3) "test2"
4) "felix"
127.0.0.1:6379> HLEN myhash				# 获取hash表的字段数量
(integer) 2
127.0.0.1:6379> 

########################################

127.0.0.1:6379> HEXISTS myhash test0			# 判断hash中指定字段是否存在！
(integer) 1
127.0.0.1:6379> HEXISTS myhash test4
(integer) 0
127.0.0.1:6379> 

########################################
# 只获得所有filed
# 只获得所有value
127.0.0.1:6379> hkeys myhash
1) "test1"
2) "test2"
3) "test3"
4) "test0"
127.0.0.1:6379> hvals myhash					# 这个地方使用的是简写
1) "clover"
2) "felix"
3) "clover"
4) "clover"
127.0.0.1:6379> 

######################################## 
incr 自增	  decr	自减

127.0.0.1:6379> hset myhash test4 5
(integer) 1
127.0.0.1:6379> HINCRBY myhash test4 1			# 自增加1
(integer) 6
127.0.0.1:6379> HINCRBY myhash test4 -1			# 这就相当于执行自减了
(integer) 5
127.0.0.1:6379> hsetnx myhash test5 clover		# 判断某一个值是否存在，不存在就创建一个，存在就返回0且不能设置
(integer) 1
127.0.0.1:6379> hsetnx myhash test5 felix
(integer) 0
127.0.0.1:6379> 
~~~

hash用来保存变更的数据，尤其是用户信息之类的，经常变更的信息！hash更适合于对象的存储，String

更加适合字符串存储！
 
 ## Zset（有序集合）
 
 在set基础上，增加了一个值
 
 ~~~bash
127.0.0.1:6379> zadd myset 1 one						# 添加一个值
(integer) 1
127.0.0.1:6379> zadd myset 2 two 3 three			# 添加多个值
(integer) 2
127.0.0.1:6379> ZRANGE myset 0 -1
1) "one"
2) "two"
3) "three"
127.0.0.1:6379> 

##########################################
排序是如何实现的呢

127.0.0.1:6379> zadd salary 2500 xiaohong			# 添加三个用户	
(integer) 1
127.0.0.1:6379> zadd salary 5000 zhangsan
(integer) 1
127.0.0.1:6379> zadd salary 500 clover
(integer) 1
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf		# 显示全部的用户！  从小到大排序
1) "clover"
2) "xiaohong"
3) "zhangsan"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf withscores		# 显示全部用户并且附带成绩
1) "clover"
2) "500"
3) "xiaohong"
4) "2500"
5) "zhangsan"
6) "5000"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf 2500 withscores		# 显示工资小于2500员工的升序排序
1) "clover"
2) "500"
3) "xiaohong"
4) "2500"

# 此处 狂神将得由问题：RANGE命令是指从小到大，不能用于去执行从大到小，要执行从大到小，得使用ZREVRANGE
127.0.0.1:6379> ZREVRANGE salary 0 -1
1) "zhangsan"
2) "xiaohong"
3) "clover"
127.0.0.1:6379> 

########################################
# 移除指定的元素rem

127.0.0.1:6379> zrange salary 0 -1
1) "clover"
2) "xiaohong"
3) "zhangsan"
127.0.0.1:6379> zrem salary xiaohong			# 移除有序集合中指定的元素
(integer) 1
127.0.0.1:6379> zrange salary 0 -1
1) "clover"
2) "zhangsan"
127.0.0.1:6379> zcard salary					# 获取有序集合中的个数
(integer) 2

#######################################
 127.0.0.1:6379> zadd myset 1 one 2 two 3 three
(integer) 3
127.0.0.1:6379> ZCOUNT myset 1 3				# 获取指定区间的成员数量
(integer) 3
127.0.0.1:6379> ZCOUNT myset 1 2
(integer) 2
127.0.0.1:6379> 
~~~

# 三种特殊数据类型

## geospatial地理位置

Redis的Geo在Redis3.2版本就推出了！这个功能可以推算地理位置的信息，两地之间的距离，方圆几里的人！

只有六个命令

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210709110013.png)

>getadd

~~~bash
# getadd 添加地理位置
# 规则：两级无法直接添加，我们一般会下载城市数据，直接通过java程序一次性导入！
# 有效的经度从-180度到180度。
# 有效的纬度从-85.05112878度到85.05112878度。
# 当坐标位置超出上述指定范围时，该命令将会返回一个错误。
    # 127.0.0.1:6379> geoadd china:city 39.90 116.40 beijin
(error) ERR invalid longitude,latitude pair 39.900000,116.400000
# 参数 key  值（）
127.0.0.1:6379> geoadd china:city 116.40 39.90 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqing 114.05 22.52 shengzhen
(integer) 2
127.0.0.1:6379> geoadd china:city 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 2
~~~

>getpos

获得当前定位：一定是一个坐标值！

~~~bash
127.0.0.1:6379> GEOPOS china:city beijing  # 获取指定的城市的经度和纬度！
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
127.0.0.1:6379> GEOPOS china:city beijing chongqi
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
2) 1) "106.49999767541885376"
   2) "29.52999957900659211"
~~~

>GEODIST

两人之间的距离！

单位：
- m 表示单位为米。
- km 表示单位为千米。
- mi 表示单位为英里。
- ft 表示单位为英尺。

~~~bash
127.0.0.1:6379> GEODIST china:city beijing shanghai km  # 查看上海到北京的直线距离
"1067.3788"
127.0.0.1:6379> GEODIST china:city beijing chongqi km   # 查看重庆到北京的直线距离
"1464.0708"
~~~

>georadius以给定的经纬度为中心， 找出某一半径内的元素

我附近的人？ （获得所有附近的人的地址，定位！）通过半径来查询！

获得指定数量的人，200

所有数据应该都录入：china:city ，才会让结果更加清晰！

~~~bash
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km  # 以110，30 这个经纬度为中心，寻
找方圆1000km内的城市
1) "chongqi"
2) "xian"
3) "shengzhen"
4) "hangzhou"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km
1) "chongqi"
2) "xian"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist  # 显示到中间距离的位置
1) 1) "chongqi"
   2) "341.9374"
2) 1) "xian"
   2) "483.8340"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withcoord  # 显示他人的定位信息
1) 1) "chongqi"
   2) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) 1) "108.96000176668167114"
      2) "34.25999964418929977"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist withcoord count 1  # 筛选出指定的结果！
1) 1) "chongqi"
   2) "341.9374"
   3) 1) "106.49999767541885376"
      2) "29.52999957900659211"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist withcoord count 2
1) 1) "chongqi"
   2) "341.9374"
   3) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167114"
      2) "34.25999964418929977"
~~~

>GEORADIUSBYMEMBER 

~~~bash
# 找出位于指定元素周围的其他元素！
127.0.0.1:6379> GEORADIUSBYMEMBER china:city beijing 1000 km
1) "beijing"
2) "xian"
127.0.0.1:6379> GEORADIUSBYMEMBER china:city shanghai 400 km
1) "hangzhou"
2) "shanghai"
~~~

>GEOHASH 命令 - 返回一个或多个位置元素的 Geohash 表示

该命令将返回11个字符的Geohash字符串!

~~~bash
# 将二维的经纬度转换为一维的字符串，如果两个字符串越接近，那么则距离越近！
127.0.0.1:6379> geohash china:city beijing chongqi
1) "wx4fbxxfke0"
2) "wm5xzrybty0"
~~~

>GEO 底层的实现原理其实就是 `Zset`！我们可以使用`Zset`命令来操作geo！

~~~bash
127.0.0.1:6379> ZRANGE china:city 0 -1  # 查看地图中全部的元素
1) "chongqi"
2) "xian"
3) "shengzhen"
4) "hangzhou"
5) "shanghai"
6) "beijing"
127.0.0.1:6379> zrem china:city beijing  # 移除指定元素！
(integer) 1
127.0.0.1:6379> ZRANGE china:city 0 -1
1) "chongqi"
2) "xian"
3) "shengzhen"
4) "hangzhou"
5) "shanghai"
~~~

## Hyperloglog

>什么是基数？

A {1,3,5,7,8,7} 

B{1,3,5,7,8}

基数（是指一个集合中不重复的元素）= 5 可以接受误差！

>简介

Redis 2.8.9 版本就更新了 Hyperloglog 数据结构！

Redis Hyperloglog  基数统计的算法！

优点：占用的内存是固定，2^64 不同的元素的基数，只需要废 12KB内存！如果要从内存角度来比较的

话 Hyperloglog 首选！

**网页的 UV （一个人访问一个网站多次，但是还是算作一个人！）**

传统的方式， set 保存用户的id，然后就可以统计 set 中的元素数量作为标准判断 !

这个方式如果保存大量的用户id，就会比较麻烦！我们的目的是为了计数，而不是保存用户id；

0.81% 错误率！ 统计UV任务，可以忽略不计的！

>测试使用

~~~bash
127.0.0.1:6379> PFadd mykey a b c d e f g h i j   # 创建第一组元素 mykey
(integer) 1
127.0.0.1:6379> PFCOUNT mykey  # 统计 mykey 元素的基数数量
(integer) 10
127.0.0.1:6379> PFadd mykey2 i j z x c v b n m   # 创建第二组元素 mykey2
(integer) 1
127.0.0.1:6379> PFCOUNT mykey2
(integer) 9
127.0.0.1:6379> PFMERGE mykey3 mykey mykey2  # 合并两组 mykey mykey2 => mykey3 并集
OK
127.0.0.1:6379> PFCOUNT mykey3  # 看并集的数量！
(integer) 15
~~~

如果允许容错，那么一定可以使用 Hyperloglog  ！

如果不允许容错，就使用 set 或者自己的数据类型即可！

## Bitmap

为什么其他教程都不喜欢讲这些？这些在生活中或者开发中，都有十分多的应用场景，学习了，就是就是

多一个思路！

技多不压身！

>位存储

统计用户信息，活跃，不活跃！  登录 、 未登录！ 打卡，365打卡！ 两个状态的，都可以使用 
Bitmap！

Bitmap 位图，数据结构！ 都是操作二进制位来进行记录，就只有0 和 1 两个状态！
365 天 = 365 bit   

1字节 = 8bit    46 个字节左右！

>测试

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210709173121.png)

一个方框代表一天，来统计一个星期的打卡情况

使用bitmap 来记录 周一到周日的打卡！

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210709173206.png)

查看某一天是否打卡

~~~bash
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 6
(integer) 0
~~~

统计操作，统计 打卡的天数！

~~~bash
127.0.0.1:6379> bitcount sign  # 统计这周的打卡记录，就可以看到是否有全勤！
(integer) 3
~~~

# 事务

	要么同时成功，要么同时失败，原子性！

Redis事务本质：一组命令的集合！一个事务中的所有命令都会被序列化，在事务执行过程中，会按照顺序

执行！

一次性（队列中的命令一次执行完）、顺序性、排他性（不允许别人干扰）！执行一系列命令！

~~~bash
-----------------  队列  set（命令）set（命令） set（命令）执行----------------------

命令执行的过程中是不允许被打断的，出现了错误会去执行处理错误的机制
~~~

**Redis事务没有隔离级别的概念**

所有的命令在事务中，并没有直接被执行！只有发起执行命令的时候才会执行！Exec（执行命令）

**Redis单条命令是保存原子性的，但是redis事务不保证原子性！**

redis的事务：

- 开启事务（multi）

- 命令入队（）

- 执行命令（exec）

>正常执行事务！

~~~bash
127.0.0.1:6379> multi    # 开启事务
OK
# 命令入队
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> exec  # 执行事务
1) OK
2) OK
3) "v2"
4) OK
127.0.0.1:6379> multi    # 开启事务
OK
# 命令入队
127.0.0.1:6379> set k1 v1

~~~

> 放弃事务！

~~~bash
127.0.0.1:6379> multi   # 开启事务
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> DISCARD   # 取消事务
OK
127.0.0.1:6379> get k4  # 事务队列中命令都不会被执行！
(nil)
~~~

>编译型异常（代码有问题！ 命令有错！） ，事务中所有的命令都不会被执行！

~~~bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> getset k3    # 错误的命令
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> set k5 v5
QUEUED
127.0.0.1:6379> exec  # 执行事务报错！
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k5  # 所有的命令都不会被执行！
(nil)
~~~

>运行时异常（1/0）， 如果事务队列中存在语法性错误，那么执行命令的时候，其他命令是可以正常执行的，错误命令抛出异常！

~~~bash
127.0.0.1:6379> set k1 "v1"
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> incr k1  # 会执行的时候失败！
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> get k3
QUEUED
127.0.0.1:6379> exec
1) (error) ERR value is not an integer or out of range  # 虽然第一条命令报错了，但是依旧正常执行成功了！
2) OK
3) OK
4) "v3"
127.0.0.1:6379> get k2
"v2"
127.0.0.1:6379> get k3
"v3"
~~~

>监控！ Watch  （面试常问！）

悲观锁：

- 很悲观，认为什么时候都会出问题，无论做什么都会加锁！

乐观锁：

- 很乐观，认为什么时候都不会出问题，所以不会上锁！ 更新数据的时候去判断一下，在此期间是否
有人修改过这个数据，

- 获取version

- 更新的时候比较 version

>Redis测监视测试

正常执行成功！

~~~bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money   # 监视 money 对象
OK
127.0.0.1:6379> multi     
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY out 20
QUEUED
127.0.0.1:6379> exec		# 事务正常结束，数据期间没有发生变动，这个时候就正常执行成功！
1) (integer) 80
2) (integer) 20
~~~

测试多线程修改值 , 使用watch 可以当做redis的乐观锁操作！

	Watch的生命周期，只是和事务关联的，一个事务执行完毕（执行了exec命令），相应的Watch的生命周期即将结束

~~~bash
127.0.0.1:6379> watch money   # 监视  money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 10
QUEUED
127.0.0.1:6379> INCRBY out 10
QUEUED
127.0.0.1:6379> exec  # 执行之前，另外一个线程，修改了我们的值，这个时候，就会导致事务执行失败！
(nil)


################################
这是第二条线程，在还没执行exec时，第二条线程插队修改了值
127.0.0.1:6379> get money
"80"
127.0.0.1:6379> set money 1000
OK
~~~

如果修改失败，获取最新的值就好

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210709214828.png)

# Jedis

我们要使用 Java 来操作 Redis

>什么是Jedis  是 Redis 官方推荐的 java连接开发工具！ 使用Java 操作Redis 中间件！如果你要使用 
java操作redis，那么一定要对Jedis 十分的熟悉！

>测试

1、导入对应的依赖

~~~xml
<!--导入jedis包-->
<dependencies>
	<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
	<dependency>
		<groupId>redis.clients</groupId>
		<artifactId>jedis</artifactId>
		<version>3.3.0</version>
	</dependency>
	<!--fastjson-->
	<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
	<dependency>
		<groupId>com.alibaba</groupId>
		<artifactId>fastjson</artifactId>
		<version>1.2.75</version>
	</dependency>
</dependencies>
~~~

2、编码测试：

-连接数据库

- 操作命令

- 断开连接！

~~~java
package com.clover;

import redis.clients.jedis.Jedis;

public class TestPing {
    public static void main(String[] args) {
        //1、new Jedis()对象即可
        Jedis jedis = new Jedis("127.0.0.1",6379);
        //jedis所有的命令就是我们之前学习的所有指令！所以之前的指令学习很重要

        System.out.println(jedis.ping());
    }
}
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210709222245.png)

## 常用的API

String

List

Set

Hash

Zset

>事务

~~~java
package com.clover;

import com.alibaba.fastjson.JSONObject;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

public class TestTX {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("127.0.0.4", 6379);

        //每次执行前清空数据库
        jedis.flushDB();

        JSONObject jsonObject = new JSONObject();
        jsonObject.put("hello","world");
        jsonObject.put("name","clover");

        //开启事务
        Transaction multi = jedis.multi();
        String result = jsonObject.toJSONString();

        try {
            multi.set("user1",result);
            multi.set("user2",result);
            //int i = 1/0;//代码抛出异常，执行失败
            multi.exec();//执行事务
        } catch (Exception e) {
            multi.discard();//放弃事务
            e.printStackTrace();
        } finally {
            System.out.println(jedis.get("user1"));
            System.out.println(jedis.get("user2"));
            jedis.close();//关闭连接
        }

    }
}
~~~

# SpringBoot整合

SpringBoot操作数据：都是集中在spring-data中操作

SpringData和SpringBoot齐名的项目！

	说明：在SpringBoot2.x之后，原来使用的Jedis被替换成了lettuce
	
**jedis：采用的直连，多个线程操作的话，是不安全的，如果想要避免不安全的，使用jedis pool连接池！更像 BIO模式**

**lettuce：采用netty，实例可以在多个线程中进行共享，不存在线程不安全的情况！可以减少线程数量，更像 NIO模式**

源码分析

~~~Java
@Bean
@ConditionalOnMissingBean(name = "redisTemplate") // 我们可以自己定义一个redis Template来替换这个默认的！ 
public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory)
    throws UnknownHostException {
    // 默认的 Redis Template 没有过多的设置，redis 对象都是需要序列化！
    // 两个泛型都是 Object, Object 的类型，我们后使用需要强制转换 <String, Object>
    RedisTemplate<Object, Object> template = new RedisTemplate<>();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
}
@Bean
@ConditionalOnMissingBean  // 由于 String 是redis中最常使用的类型，所以说单独提出来了一个bean！
public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory)
    throws UnknownHostException {
    StringRedisTemplate template = new StringRedisTemplate();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
}
~~~

>整合测试

1、导入redis依赖

~~~xml
<!--操作redis-->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

2、编写配置文件

~~~xml
# SpringBoot 所有的配置类，都会由一个自动配置类
# 自动配置类都会绑定一个 properties 配置文件

# 配置redis
spring.redis.host=127.0.0.1
spring.redis.port=6379

# 我们以后使用集群部署时尽量使用最新的lettuce，因为Jedis中有一部分配置类不存在，所以不生效
# 而lettuce中所有的类默认都是生效的
~~~

	我们以后使用集群部署时尽量使用最新的lettuce，因为Jedis中有一部分配置类不存在，所以不生效
	而lettuce中所有的类默认都是生效的

3、测试！

~~~java
package com.clover.redis02springboot;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.RedisTemplate;

@SpringBootTest
class Redis02SpringbootApplicationTests {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {

        // redisTemplate  操作不同的数据类型，api和我们的指令是一样的
        // opsForValue  操作字符串 类似String
        // opsForList   操作List 类似List
        // opsForSet
        // opsForHash
        // opsForZSet
        // opsForGeo
        // opsForHyperLogLog
        // 除了基本的操作，我们常用的方法都可以直接通过redisTemplate操作，比如事务，和基本的CRUD
        // 获取redis的连接对象
        // RedisConnection connection =redisTemplate.getConnectionFactory().getConnection();
        // connection.flushDb();
        // connection.flushAll();

        redisTemplate.opsForValue().set("mykey","clover");
        System.out.println(redisTemplate.opsForValue().get("mykey"));
    }

}
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210710215204.png)

>关于对象的保存时出现的问题

1、使用了序列化时的结果
~~~Java
@Test
public void test() throws JsonProcessingException {
	// 真实的开发中，我们一般使用json来传递对象
	User user = new User("clover", 3);
	// 这个是我们创建SpringBoot项目中所存在的，也就是jackson对象
	// 序列化该对象
	String obj = new ObjectMapper().writeValueAsString(user);
	redisTemplate.opsForValue().set("user",obj);
	System.out.println(redisTemplate.opsForValue().get("user"));
}
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210719105913.png)

2、未使用序列化时的结果
~~~Java
@Test
public void test() throws JsonProcessingException {
	// 真实的开发中，我们一般使用json来传递对象
	User user = new User("clover", 3);
	// 这个是我们创建SpringBoot项目中所存在的，也就是jackson对象
	// 序列化该对象
	//String obj = new ObjectMapper().writeValueAsString(user);
	redisTemplate.opsForValue().set("user",user);
	System.out.println(redisTemplate.opsForValue().get("user"));
}
~~~
产生的报错结果如下图：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210719105714.png)

我们编写一个自己的Tempalte

~~~Java
package com.clover.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;
@Configuration
public class RedisConfig {
    // 这是我给大家写好的一个固定模板，大家在企业中，拿去就可以直接使用！
    // 自己定义了一个 RedisTemplate
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        // 我们为了自己开发方便，一般直接使用 <String, Object>
        RedisTemplate<String, Object> template = new RedisTemplate<String,Object>();
        template.setConnectionFactory(factory);
        // Json序列化配置
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        // String 的序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        // key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的value序列化方式采用jackson
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }

}
~~~

所有的redis操作，其实对于Java操作人员来说，十分的简单，更重要的是去理解redis的思想和每一种数据结构的用处和作用场景！

# Redis.conf详解

启动的时候，就通过配置文件来启动!

>单位

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210719145819.png)

1、配置文件unit单位，对大小写不敏感

>包含

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210719150023.png)

可以将多个配置文件导入进来，合并成一个配置文件

> 网络

~~~bash
bind 127.0.0.1					 # 绑定的ip
protected-mode yes		  # 保护进程
port 6379						  # 端口设置
~~~

>通用GENERAL

~~~bash
daemonize yes			# 以守护进程的方式运行，默认是no，我们需要自己开启为yes！

pidfile /var/run/redis_6379.pid # 如果以后台的方式运行，我们就需要指定一个pid文件！

# 日志
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice				# 日志级别

logfile ""						# 日志文件的位置名，为空就是一个标准的输出

databases 16			  # 数据库的数量，默认是16个数据库

always-show-logo no		# 是否总是显示log，默认为false
~~~

>快照 SNAPSHOTTING

持久化，在规定的时间内，执行了多少次操作，则会持久化到文件 .rdb  .aof

redis是内存数据库，如果没有持久化操作，那么数据断电即失！

~~~bash
save 3600 1							# 如果3600s内，至少有一个key进行了修改，我们就进行持久化操作
save 300 100					   # 如果300s内，至少有100个key进行了修改，我们就进行持久化操作
save 60 10000					  # 如果60s内，至少有10000个key进行了修改，我们就进行持久化操作

stop-writes-on-bgsave-error yes			# 持久化如果出错，是否还要继续工作！

rdbcompression yes						# 是否压缩rdb文件，需要消耗一定cpu资源

rdbchecksum yes							# 保存rdb文件的时候，进行错误的校验检查

dir ./											# rdb 文件保存的目录
~~~ 

>REPLICATION复制，后面讲主从复制的时候详细讲解

~~~bash
~~~  

>SECURITY 安全

可以在这里设置密码，redis默认没有密码

~~~bash
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> config get requirepass				# 获取redis的密码
1) "requirepass"
2) ""	
127.0.0.1:6379> config set requirepass "clover"			# 设置redis的密码
OK
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "clover"
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> exit

# 这里设置密码后退出重新进入后才会生效，因为变向的改变了配置文件

[root@iZ8vb8plggk0dqs0rr5kzfZ ~]# redis-cli
127.0.0.1:6379> ping
(error) NOAUTH Authentication required.
127.0.0.1:6379> config get requirepass				# 发现命令没有权限了
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth clover							# 使用密码进行登录
OK
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "clover"
127.0.0.1:6379> 
~~~  

> 限制CLIENTS

~~~bash
maxclients 10000				# 设置能连接上redis的最大客户端的数量

maxmemory <bytes>			# redis配置最大的内存容量

maxmemory-policy noeviction			# 内存达到上限后的处理策略

	1、noeviction
	2、allkeys-lru
	3、volatile-lru
	4、allkeys-random
	5、volatile-random
	6、volatile-ttl:在设置了过期时间的所有键，抛弃存活时间最短的数据。

~~~  

>APPEND ONLY  模式 aof配置

~~~bash
appendonly no				# 默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分所有的情况下，rdb是够使用的

appendfilename "appendonly.aof"			# 持久化文件的名字

# appendfsync always			# 每次修改都会执行sync，速度会很慢，消耗性能
appendfsync everysec			# 每秒执行一次 sync，在这一秒宕机，可能会丢失这一秒的数据
# appendfsync no				  # 不执行 sync，这个时候操作系统自己同步数据，速度最快
~~~  

# Redis持久化

Redis是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会小时。所以Redis提供了持久化功能

## RDB（Redis Database）

在主从复制中，RDB是用来备份的，存在在从机中，不占用主机内存！

>什么是RDB

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721143706.png)

在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里。

Redis会单独创建一个(fork)一个紫禁城来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。这就确保了极高的性能。
如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。我们默认的就是RDB，一般情况下不需要修改这个配置！

**rdb保存的文件是 `dump.rdb`**  都是在我们的配置文件中的快照进行配置的！

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721145216.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721151859.png)

>触发机制

1、sava规则满足的情况下，会自动触发rdb规则

2、执行flushall命令，也会触发我们的rdb规则

3、退出redis，也会产生rdb文件

备份就会自动生成一个dump.rdb文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721152117.png)

	注意，使用sava命令保存时，也会产生一个edb文件
	
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721151342.png)

>如何恢复rdb文件

1、只需要将rdb文件放在我们redis启动目录就可以了，redis启动的时候会自动检查dump.rdb 恢复其中的数据

2、查看需要存放的位置
~~~bash
127.0.0.1:6379> config get dir
1) "dir"
2) "/usr/local/bin"
~~~  

>几乎它自己默认的配置就够用了，但是我们还是要去学习

**优点:**

	1、适合大规模的数据恢复！

	2、对数据的完整性要求不高！

**缺点：**

	1、需要一定的时间间隔进行操作！如果redis意外宕机了，这个最后一次修改的数据就没有了！

	2、fork进程的时候，会占用一定的内存空间!

## AOF(Append Only File)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721200204.png)

以日志的形式来记录每个写操作。将Redis执行过的所有指令记录下来（读操作不记录），只许追加文件但不可以改写文件，`redis启动之初会读取该文件重新构建数据`，换而言之，redis重启的化就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

AOF保存的是appendonly.aof文件

>append

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721201825.png)

默认是不开启的，我们需要手动进行配置！我们只需要将appendonly 改为yes就开启了aof！

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721201900.png)

重启，redis就可以生效了！

如果这个aof文件有错误，这时候redis是启动不起来的，我们需要修复这个aof文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721202304.png)

redis 给我们提供了一个工具`redis-check-aof --fix`

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721202405.png)

如果文件正常，重启就可以直接恢复了！

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721202520.png)

但是它对文件进行修复，也不是全部修复，是可能损坏部分值的！！！

>重写规则说明

aof默认就是文件的无限追加，文件会越来越大

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210721203830.png)

如果aof文件大于64M，太大了的话，就会fork一个新的进程来将我们的文件进行重写！

>优点和缺点

**优点：**

1、每一次修改都同步，文件的完整性会更好

2、每秒同步一次，可能会丢失一秒的数据

3、从不同步，效率是最高的！

**缺点：**

1、相对于数据文件来说，aof远远大于rdb，修复的速度也比rdb慢！

2、Aof运行效率也比rdb慢（因为其文件比rdb大），所以我们redis默认的配置就是rdb持久化！

**扩展：**

1、RDB 持久化方式能够在指定的时间间隔内对你的数据进行快照存储  

2、AOF 持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以Redis 协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。  

**3、只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化  **

4、同时开启两种持久化方式

-   在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整。
-   RDB 的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢？作者建议不要，因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，留着作为一个万一的手段。

5、性能建议

-   因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留 save 900 1 这条规则。
-   如果Enable AOF ，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了，代价一是带来了持续的IO，二是AOF rewrite 的最后将 rewrite 过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。
-   如果不Enable AOF ，仅靠 Master-Slave Repllcation 实现高可用性也可以，能省掉一大笔IO，也减少了rewrite时带来的系统波动。代价是如果Master/Slave 同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个 Master/Slave 中的 RDB文件，载入较新的那个，微博就是这种架构。

# Redis发布订阅

Redis发布订阅（pub/sub）是一种消息通信模式：发送者（pub）发送消息，订阅者（sub）接受消息。

Redis客户端可以订阅任意数量的频道。

订阅/发布消息图：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723202318.png)

下图展示了频道channel1，以及订阅这个频道的三个客户端--client2、client5和client1之间的关系：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723202452.png)

当有新消息通过PUBLISH 命令发送给频道channel1时，这个消息就会被发送给订阅它的三个客户端：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723202622.png)

> 命令

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723200431.png)

>测试

订阅端：

~~~bash
127.0.0.1:6379> SUBSCRIBE clover
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "clover"
3) (integer) 1
# 等待读取推送的信息
1) "message"		# 接送的是什么，消息
2) "clover"			   # 哪个频道的消息
3) "hello,world"	# 消息的具体内容
1) "message"
2) "clover"
3) "hello,cloverfelix"
~~~  

发送端:

~~~bash
127.0.0.1:6379> PUBLISH clover "hello,world"			# 发布者发布消息到频道
(integer) 1
127.0.0.1:6379> PUBLISH clover "hello,cloverfelix"		# 发布者发布消息到频道
(integer) 1
127.0.0.1:6379> 
~~~  

**原理**

Redis是使用C实现的，通过分析 Redis 源码里的 pubsub.c 文件，了解发布和订阅机制的底层实现，籍此加深对 Redis 的理解。

Redis 通过 PUBLISH 、SUBSCRIBE 和 PSUBSCRIBE 等命令实现发布和订阅功能。

每个 Redis 服务器进程都维持着一个表示服务器状态的 redis.h/redisServer 结构， 结构的 pubsub_channels 属性是一个字典， 这个字典就用于保存订阅频道的信息，其中，字典的键为正在被订阅的频道， 而字典的值则是一个链表， 链表中保存了所有订阅这个频道的客户端。

客户端订阅，就被链接到对应频道的链表的尾部，退订则就是将客户端节点从链表中移除。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723202921.png)

**缺点**

1.  如果一个客户端订阅了频道，但自己读取消息的速度却不够快的话，那么不断积压的消息会使redis输出缓冲区的体积变得越来越大，这可能使得redis本身的速度变慢，甚至直接崩溃。
2.  这和数据传输可靠性有关，如果在订阅方断线，那么他将会丢失所有在短线期间发布者发布的消息。

**应用**

1.  消息订阅：公众号订阅，微博关注等等（起始更多是使用消息队列来进行实现）
2.  多人在线聊天室。

稍微复杂的场景，我们就会使用消息中间件MQ处理。

# Redis主从复制

## 概念

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点（Master/Leader）,后者称为从节点（Slave/Follower）， **数据的复制是`单向的`！只能由主节点复制到从节点**（主节点以写为主、从节点以读为主）。

**默认情况下，每台Redis服务器都是主节点**

一个主节点可以有0个或者多个从节点，但每个从节点只能由一个主节点。

## 作用

1.  数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余的方式。

2.  故障恢复：当主节点故障时，从节点可以暂时替代主节点提供服务，是一种服务冗余的方式

3.  负载均衡：在主从复制的基础上，配合读写分离，由主节点进行写操作，从节点进行读操作，分担服务器的负载；尤其是在多读少写的场景下，通过多个从节点分担负载，提高并发量。

4.  高可用（集群）基石：主从复制还是哨兵和集群能够实施的基础。

## 为什么使用集群

一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的（宕机），原因如下：

1、从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大；

2、从容量上，单个Redis服务器内存容量有限，就算一台Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，**单台Redis最大使用内存不应该超过20G。**

电商网站上的商品，一般都是一次上传，无数次浏览的，说专业点也就是"多读少写"。

对于这种场景，我们可以使如下这种架构：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723204337.png)

# 环境配置

只用配置从库，不用配置主库，因为每个Redis是默认自己是主库

~~~bash
127.0.0.1:6379> info replication						# 查看当前库的信息
# Replication
role:master						# 角色   master
connected_slaves:0		  # 没有从机
master_failover_state:no-failover
master_replid:ce9e2da75c46b69ccaf4bea614598be642658691
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
~~~  

复制3个配置文件，然后修改对应的信息

1、端口

2、pid 名字

3、log文件名字

4、dump.rdb 名字

修改完毕后，启动我们3个redis服务，可以通过进程信息查看

## 一主二从

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723210738.png)

~~~bash
127.0.0.1:6380> SLAVEOF 127.0.0.1 6379
OK
127.0.0.1:6380> info replication
# Replication
role:slave				# 当前角色是从机
master_host:127.0.0.1  # 可以查看到主机的信息
master_port:6379
master_link_status:up
master_last_io_seconds_ago:3
master_sync_in_progress:0
slave_repl_offset:14
slave_priority:100
slave_read_only:1
replica_announced:1
connected_slaves:0
master_failover_state:no-failover
master_replid:80653bedaad4360f908ff72a68a47a4bd1045f0f
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:14
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:14
127.0.0.1:6380> 


# 在主机中查看信息
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:2			# 多了从机的信息，可以查看从机的信息
slave0:ip=127.0.0.1,port=6380,state=online,offset=56,lag=1
slave1:ip=127.0.0.1,port=6381,state=online,offset=56,lag=1
master_failover_state:no-failover
master_replid:80653bedaad4360f908ff72a68a47a4bd1045f0f
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:56
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:56
127.0.0.1:6379> 
~~~  

如果两个都配置完了，就会显示两个从机

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723212114.png)

真实的主从配置应该在配置文件中配置，这样的话是永久的，我们这里使用的是命令，只是暂时的！

## 使用规则

1.从机只能读，不能写，主机可读可写但是多用于写。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723213646.png)

2.从机只能读取内容，如果写入内容会报错

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723213742.png)


>测试

主机断开连接，从机依旧连接到主机的，但是没有写操作，这个时候，主机如果回来了，从机依旧可以直接获取到主机写的信息

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723213924.png)

如果是使用命令行来配置的从机，这个时候如果重启了，就会变回主机！只要变为从机，立马就会从主机中获取值！

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723214210.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723214303.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723214442.png)

>复制原理

Slave 启动成功连接到 master 后会发送一个`sync同步`命令

Master 接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，master将传送整个数据文件到slave，并完成一次完全同步。

**全量复制**：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。

**增量复制**：Master 继续将新的所有收集到的修改命令依次传给slave，完成同步

但是只要是重新连接master，一次完全同步（全量复制）将被自动执行！ 我们的数据一定可以在从机中看到！

>层层链路

上一个Master链接下一个Slave

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210723214841.png)

这样也可以完成我们的主从复制！

>如果没有老大了，这个时候能不能选择一个老大出来呢？哨兵模式没出来之前是：手动选择

如果主机断开了连接，我们可以使用`SLAVEOF no one`让自己变成主机！其它的节点就可以手动连接到这个最新的主节点（手动）如果这个时候老大修复了，那就只有重新配置连接了！

## 哨兵模式

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  

~~~bash
~~~  
