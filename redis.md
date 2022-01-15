



# 01_NoSql概述



## 为什么要用NoSql

1. 单机MySQL的时代。
   ![单机mysql时代](/Users/dyq/Desktop/redis/pics/单机mysql时代.jpg)
   一个基本的网站访问量一般不会太大，单个数据库完全足够。
   那时候更多使用的静态网页html，服务器根本没有太大压力。
   这时候网站的瓶颈是什么？

   - 数据量如果太大，一个机器放不下。
   - 数据量太大需要建立数据的索引（B+ Tree），一个服务器内存放不下。
   - 访问量读写混合，一个服务器承受不了。

2. memcached缓存+MySQL+垂直拆分（读写分离）。
   ![memcached缓冲](/Users/dyq/Desktop/redis/pics/memcached缓冲.jpg)
   网站80%的情况都是在读，每次都要去查询数据库的话效率低，我们可以使用缓存来保证效率，减轻数据库的压力。
   发展过程：优化数据结构和索引->文件缓存（IO)->Memcached缓存(当时最热门的技术，中间件)

3. 分库分表+水平拆分+MySQL集群。

   主从复制

   M：master主节点
   S：slave从节点

   数据库的本质其实就是（读（前面的cache已经解决了读的问题），写）

   早些年MyISAM：表锁（100万，十分影响效率，高并发下会出现严重的锁问题）

   转战Innodb：行锁，效果比表锁好很多

   

   慢慢的，就开始使用分库分表来解决写的压力！MySQL在那个年代就推出了表分区！这个并没有多少公司使用！

   MySQL的集群，很好满足了那个年代的需求。

   

   

   ![水平拆分](/Users/dyq/Desktop/redis/pics/水平拆分.png)

   

   缓冲没有的话，去集群里面查，三个集群各有1/3的数据，通过集群的机制，查数据在哪个集群。

4. 如今最近的年代

   2010~2020十年之间，世界已经发生了翻天覆地的变化。（定位也是一种数据。音乐！热榜！）
   面对这些数据，MySQL等关系型数据库就不够用了，数据量很多，变化很快。

   于是就出现了一些新的数据库，例如json。

   MySQL有些用来存储比较大的文件、博客、图片，那么这样的话->数据库表很大->效率低下，如果有一种数据库来专门处理这种数据->MySQL压力就会变小（研究如何处理这些问题）。大数据的IO压力下，表几乎没法更大。

5. 目前的互联网项目

   ![目前的互联网项目](/Users/dyq/Desktop/redis/pics/目前的互联网项目.png)

6. 为什么要用NoSql

   用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志等等爆发式增长。
   这时候我们就需要使用NoSql数据库，NoSql可以很好的处理以上的情况。



## 什么是NoSql

Not Only SQL（不仅仅是SQL）泛指非关系型数据库。

关系型数据库：表格，行和列

随着web2.0互联网的诞生（音乐，视频，社区），传统的关系型数据库很难对付这个时代，尤其是超大规模的高并发社区。

现在NoSQL在当今大数据环境下已经是必备了，redis是我们必须掌握的一个技术！！！

很多的数据类型：用户的个人信息，社交网络，地理位置。这些数据类型的存储不需要一个固定的格式（行和列）！比如地图就是一个拓扑图，那么就不应该放在关系型数据库里，我们需要其动态发展。

不需要多余的操作就可以横向扩展的！Map<String，Object>使用键值对来控制。



## NoSql的特点

1. 方便拓展（数据之间没有关系，很好拓展！）

2. 大数据量高性能（官方介绍：redis一秒可以写8万次，读取11万，NoSql的缓存记录级，可以不停的被刷新，是一种细粒度的缓存，性能比较高）

3. 数据类型是多样型的（不需要事先设计数据库，随取随用。例如以前的话，如果是数据量十分大的表，很多人就无法设计了）

4. 传统的RDBMS和NoSql

   ```
    传统的RDBMS
    -结构化组织
    -SQL
    -数据和关系都存在单独的表中
    -操作，数据定义语言(DML,DDL)
    -严格的一致性
    -基础的事务
    -...
   ```

   ```
    NoSql
    -不仅仅是数据
    -没有固定的查询语言
    -四种存储类型：键值对存储，列存储，文档存储，图形数据库（社交关系）
    -最终一致性（只要最后一样就行）
    -CAP定理和BASE（异地多活，保证整个服务器不会宕机）初级架构师！（狂神理念：只要学不死，就往死里学！）
    -高性能，高可用，高可扩
    -...
   
   ```

## 大数据3V+3高

大数据时代的3V：主要是描述问题的
1.海量Volume
2.多样Variety
3.实时Velocity
大数据时代的3高：主要是对程序的要求
1.高并发
2.高可拓（**随时水平拆分，机器不够了，可以随时加一台服务器，扩展机器来解决** ）
3.高性能（保证用户体验和效率）



```
真正在公司中的实践一定是：NoSQL+RDBMS，一起使用才是最强的，阿里巴巴的架构演进。

技术没有高低之分，就看你如何使用！（提升思维，而不仅仅是redis）
```



## 阿里巴巴的演进分析

思考：一个网站这么多东西，例如1688，就放在一个数据库里面吗

最早的时候，阿里巴巴买的国外的PHP网站



敏捷开发概念 、极限编程概念！





如果你未来想当一个架构师：没什么是加一层解决不了的

```
1. 商品的基本信息
	名称，价格，商家信息
	关系型数据库就可以解决了，mysql/oracle （淘宝造就去IOE）
	淘宝内部的mysql（底层不一样了）和我们的mysql是不一样的
	
2. 商品的描述、评论（文字比较多）
	文档型数据库中，MongoDB
	
3. 图片
	分布式文件系统 FastDFS
		淘宝自己的 TFS
		Google的GFS
		阿里云的oss
		hadoop HDFS
		
4. 商品的关键字（搜索）
	搜索引擎， solr， elasticsearch
	Isearch是淘宝自己用的， 王坚，多隆（他是阿里的第一位程序员，Isearch是他自己开发的）
	
5. 商品的热门波段信息
	内存数据库
	redis Tair、 Memache
	
6. 商品的交易，外部的支付接口
	三方应用
	
```

所有牛逼的人都有一段苦逼的岁月，但是你只要像SB一样的去坚持，终将牛逼！



### 大型互联网应用问题：

· 数据类型太多了·

· 数据源繁多，经常重构

· 数据要改造，大面积改造？

### 解决问题：

UDSL的二级缓存



## NoSql的四大分类

```
KV键值对：
		不同的公司的不同实现
    - 新浪：redis
    - 美团：redis+Tair
    - 阿里、百度：redis+memcache
    Memcache(内存缓存)是一个高性能的分布式的内存对象缓存系统。通过在内存里维护一个巨大的hash表。
文档型数据库（bson格式和json一样）：
    - MongoDB(一般必须要掌握)
    - MongoDB是一个基于分布式文件存储的数据库，C++编写，主要用来处理大量的文档。
    - MongoDB是一个介于关系型数据库和非关系型数据中间的产品。MongDB是非关系型数据库中功能最丰富，最像关系型数据库的。
    - CouchDb(国外的另一种)
列存储数据库：
    - HBase
    - 分布式文件系统
图形关系型数据库
    - 他不是存图形，放的是关系，比如：朋友圈社交网络，广告推荐。
    - Neo4j，infoGrid

```

![nosql的四大分类](/Users/dyq/Desktop/redis/pics/nosql的四大分类.png)





# 02_Redis入门

## 概述

### Redis是什么

Redis（Remote Dictionary Server )，即远程字典服务。

是一个开源的使用ANSI  **C语言编写**、支持**网络**、可**基于内存亦可持久化**的日志型、**Key-Value**数据库，并提供**多种语言的APl**。

redis会**周期性**的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了**master-slave（主从）同步**。

免费和开源！是当下最热门的NoSQL技术之一！也被人们称之为**结构化数据库**！

### Redis能干嘛

`1. 内存存储、持久化，内存中数据是断电即失、所以说持久化很重要（rdb、aof）。`

`2. 效率高，可以用于高速缓存。`

`3. 发布订阅系统（简单的消息队列的东西）。`

`4. 地图信息分析。`

`5. 计时器、计数器（微信微博浏览量！incre，decre）。`

`6. ......`

### 特性

`1. 多样的数据类型`

`2. 持久化`

`3. 集群`

`4. 事务`

`5. 开源`

### 学习中更需要用的东西

1. 狂神的公众号：狂神说
2. 官网（英文）：https://redis.io/
3. 中文网：http://www.redis.cn/ （中英文网是一摸一样的对应的）
4. 注意：windows版本的redis在github上下载，停更很久了，因为官方不太建议在windows上用了，他们**推荐的是在Linux服务器上搭建**。



## Windows安装

1. github下载安装包
2. 下载得到压缩包（才5M，十分的小）
3. 解压到自己电脑上的环境目录

4. 开启Redis，运行服务即可，默认端口好6379

5. 使用redis客户端来链接redis，测试只要ping，就会出现PONG

   记住一句话，在windows下使用redis，确实简单，但是Redis推荐我们使用Linux开发使用

   



## Linux安装

1. 下载安装包

2. 解压Redis的安装包，最好放在/opt下面

3. 这就和windows下没什么区别了

4. 进入解压后的文件，可以看到redis的配置文件（我们一般会改，建议拷贝）

5. 基本环境安装：

   ```
   yum install gcc-c++
   ```

   redis是写的

   ```
   make
   ```

   make的坑

   安装redis的6.0以上版本需要升级gcc到5.3及以上,如下：升级到gcc 9.3：
   yum -y install centos-release-scl
   yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
   scl enable devtoolset-9 bash
   需要注意的是scl命令启用只是临时的，退出shell或重启就会恢复原系统gcc版本。
   如果要长期使用gcc 9.3的话：
   echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile
   这样退出shell重新打开就是新版的gcc了

   #### make

   make与makefile
   在linux系统中make是一个非常重要的编译命令，不管是自己进行项目开发还是安装应用软件，我们都经常要用到make或makeinstall。利用make工具，我们可以将大型的开发项目分解成为多个更易于管理的模块，一个工程中的源文件不计数，其按类型、功能、模块分别放在若干个目录中，makefile定义了一系列的规则来指定，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的功能操作，因为makefile就像一个Shell脚本一样，其中也可以执行操作系统的命令。makefile 带来的好处就是“自动化编译”，一旦写好，只需要一个make命令，整个工程完全自动编译，极大的提高了软件开发的效率。

   Make的工作原理
   当make 命令被执行时，它会扫描当前目录下Makefile或makefile文件找到目标以及其依赖。如果这些依赖自身也是目标，继续为这些依赖扫描Makefile 建立其依赖关系，然后编译它们。一旦主依赖编译之后，然后就编译主目标,假设你对某个源文件进行了修改，你再次执行make 命令，它将只编译与该源文件相关的目标文件，因此，编译完最终的可执行文件节省了大量的时间。
   ————————————————
   版权声明：本文为CSDN博主「y_fan」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
   原文链接：https://blog.csdn.net/qq_22182835/article/details/89467386

6. make以后就有了.server

7. make install 

   ```
   yum insatll gcc-c++
   make
   make install
   
   ```

8. redis的默认安装路径：'/usr'

   /usr/loca/bin

   ![make install](/Users/dyq/Desktop/redis/pics/make install.png)

9. 将redis配置文件拷贝

   我们以后就用这个配置文件进行启动，原文件不用动了就。

   ![dyqConfig](/Users/dyq/Desktop/redis/pics/dyqConfig.png)

10. redis默认不是后台启动的，修改配置文件！

    ## daemon  英  [ˈdiːmən] 美  [ˈdiːmən]

    - n. 守护进程；后台程序

    ![截屏2022-01-02 下午6.46.18](/Users/dyq/Desktop/截屏2022-01-02 下午6.46.18.png)

    改为yes，即后台启动

11. 通过指定的配置文件启动Redis服务

    ![启动redis](/Users/dyq/Desktop/redis/pics/启动redis.png)

12. 使用redis客户端进行连接测试

    ![使用redis客户端进行连接测试](/Users/dyq/Desktop/redis/pics/使用redis客户端进行连接测试.png)

13. 查看redis进程是否开启

    ![测试redis进程是否开启](/Users/dyq/Desktop/redis/pics/测试redis进程是否开启.png)

14. 如何关闭redis服务

    ![关闭redis](/Users/dyq/Desktop/redis/pics/关闭redis.png)

    再次查看进程是否存在，ps -ef|grep redis，server和client就都没有了

15. 后面我们要用单机多redis来启动集群测试



## 测试性能

redis-benchmark是一个压力测试工具

官方自带的性能测试工具，也在usr/local/bin

![redis-benchmark](/Users/dyq/Desktop/redis/pics/redis-benchmark.jpeg)



简单测试，找对应的参数就行!

```shell
#测试：100个并发连接，请求数
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

![benchmark分析](/Users/dyq/Desktop/redis/pics/benchmark分析.jpeg)



**如何查看这些分析呢？**

对我们的100000个请求进行写入测试

100个并发客户端

每次写入3个字节

只保证有一台服务器来处理这些请求，单机性能

所有请求在3ms完成，每秒处理59276个请求



## 基础知识

### redis有16个数据库

redis默认有十六个数据库，可以在配置文件里面看到

默认使用的是第0个数据库，可以使用select切换数据库

```shell
select 1  #切换1数据库
DBSIZE  #查看DB大小，大小写无所谓，只查看当前数据库的大小
keys *  #查看 当前数据库 所有key
flushall #清空所有数据库
flushdb #清空当前数据库
exists key #判断当前key是否存在
move key 1 #移除当前的key
expire key second #设置过期时间
ttl key  #查看key剩余过期时间
type key #查看key的类型

```



### 为什么redis是6379！

——Alessia Merz 是一位意大利舞女、女演员。 Redis 作者 Antirez 早年看电视节目，觉得 Merz 在节目中的一些话愚蠢可笑，Antirez 喜欢造“梗”用于平时和朋友们交流，于是造了一个词 “MERZ”，形容愚蠢，与 “stupid” 含义相同。
——后来 Antirez 重新定义了 “MERZ” ，形容”具有很高的技术价值，包含技艺、耐心和劳动，但仍然保持简单本质“。
——到了给 Redis 选择一个数字作为默认端口号时，Antirez 没有多想，把 “MERZ” 在手机键盘上对应的数字 6379 拿来用了。



### redis是单线程的

**redis6.0开始支持多线程了，关于io的部分，需要在配置文件中开启多线程**

官方表示，Redis是基于内存操作的，CPU不是Redis的性能瓶颈。Redis的瓶颈是根据机器的内存和网络带宽，既然可以使用单线程，就使用单线程了！

Redis是C语言写的，官方提供的数据为100000+的QPS(每秒查询率)，完全不比同样使用key-value的MemeCache差！

#### redis为什么那么快？

1. 误区1：高性能的服务器一定是多线程的
2. 误区2：多线程（CPU上下文会切换！）一定比单线程效率高

cpu>内存>硬盘

核心：redis将所有的数据全部放在内存中，所以单线程去操作效率最高，多线程（CPU上下文切换很耗时），对于内存系统来说，如果没有上下文切换效率就是最高的，多次读写都是在一个cpu上的。

**虽然断电即失，但是redis有持久化策略**





# 03_五大数据类型

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作

**数据库、缓存和消息中间件MQ**。

 它支持多种类型的数据结构，如 [字符串（strings）](http://redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://redis.cn/topics/lru-cache.html)，[事务（transactions）](http://redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。





```
所有命令都要记住，后面我们使用springboot，jedis，所有方法就是这些命令
单点登录
```



## Redis-Key





```shell
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> set name sywl
OK
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> set age 1
OK
127.0.0.1:6379> keys *
1) "name"
2) "age"
127.0.0.1:6379> exists name # 判断当前key是否存在
(integer) 1
127.0.0.1:6379> exists name1
(integer) 0
127.0.0.1:6379> move name 1 # 将name移动到数据库1
(integer) 1
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> set name sywl
OK
127.0.0.1:6379> keys *
1) "name"
2) "age"
127.0.0.1:6379> get name
"sywl"
127.0.0.1:6379> expire name 10 # 设置key的过期时间，单位是秒
(integer) 1
127.0.0.1:6379> ttl name # 查看当前key的剩余时间
(integer) 6
127.0.0.1:6379> ttl name # time to live 查看过期剩余时间
(integer) 3
127.0.0.1:6379> ttl name
(integer) 0
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> get name
(nil)
127.0.0.1:6379> type name
none
127.0.0.1:6379> set name sywl
OK
127.0.0.1:6379> type name # 查看当前key的一个类型
string
127.0.0.1:6379> type age
string
127.0.0.1:6379>

```



## String（字符串）

90%的java程序员就只会使用String在redis里面

```shell
##################################
127.0.0.1:6379> set key1 v1 # 设置值
OK
127.0.0.1:6379> get key1 # 获取值
"v1"
127.0.0.1:6379> append key1 "hello" # 追加字符串，如果当前key不存在，就相当于set key1 v1设置值
(integer) 7
127.0.0.1:6379> get key1
"v1hello"
127.0.0.1:6379> strlen key1 # 获取字符串的长度
(integer) 7
127.0.0.1:6379> append key1 ",sywl"
(integer) 12
127.0.0.1:6379> get key1
"v1hello,sywl"
##################################
127.0.0.1:6379> set views 0 # 设置初始浏览量为0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views # 自增1，浏览量变为1
(integer) 1
127.0.0.1:6379> get views
"1"
127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> get views
"2"
127.0.0.1:6379> decr views # 自减1，浏览量-1
(integer) 1
127.0.0.1:6379> decr views
(integer) 0
127.0.0.1:6379> decr views
(integer) -1
127.0.0.1:6379> get views
"-1"
127.0.0.1:6379> incrby views 10 # 可以设置步长，指定增量
(integer) 9
127.0.0.1:6379> incrby views 10
(integer) 19
127.0.0.1:6379> decrby views 10 # 可以设置步长，指定减量
(integer) 9
##################################
# 字符串范围
127.0.0.1:6379> set key1 hello,kuangshen # 设置key1的值
OK
127.0.0.1:6379> get key1
"hello,kuangshen"
127.0.0.1:6379> getrange key1 0 3 # 截取字符串[0,3]，前后闭区间
"hell"
127.0.0.1:6379> getrange key1 0 -1 # 获取全部字符串和get key是一样的
"hello,kuangshen"
# 替换
127.0.0.1:6379> set key2 abcdefg
OK
127.0.0.1:6379> get key2
"abcdefg"
127.0.0.1:6379> setrange key2 1 xx # 替换指定位置开始的字符串(从指定索引的这个位置开始替换)
(integer) 7
127.0.0.1:6379> get key2
"axxdefg"
##################################
# setex (set with expire) # 设置值并设置过期时间
# setnx (set if not exist) # 不存在时再设置（在分布锁中常常使用）
127.0.0.1:6379> setex key3 30 "hello" # 设置key3的值为hello，30秒后过期。如果key3存在则覆盖key3的值，并且30后过期
OK
127.0.0.1:6379> ttl key3
(integer) 24
127.0.0.1:6379> get key3
"hello"
127.0.0.1:6379> setnx mykey "redis" # 如果mykey不存在，创建mykey
(integer) 1
127.0.0.1:6379> keys *
1) "key2"
2) "mykey"
3) "key1"
127.0.0.1:6379> ttl key3
(integer) -2
127.0.0.1:6379> setnx mykey "MongoDB" # 如果mykey存在，则创建失败
(integer) 0
127.0.0.1:6379> get mykey
"redis"
##################################
# mset
# mget
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3 # 同时设置多个值
OK
127.0.0.1:6379> keys *
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> mget k1 k2 k3 # 同时获取多个值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4 # msetnx是一个原子性操作，要么一起成功，要么一起失败
(integer) 0
127.0.0.1:6379> get k4
(nil)
# 对象
set user:1 {name:zhangsan,age:3} # 设置一个user:1对象，值为json字符
# 这里的key是一个巧妙的设计：user:{id}:{field}，如此设计在redis中是完全ok的！

#设置新的key
127.0.0.1:6379> mset user:1:name zhangsan user:1:age 2
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "2"
##################################
getset # 先get然后再set
127.0.0.1:6379> getset db redis # 如果不存在值，则返回nil
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db mongdb # 如果存在值，获取原来的的值，并设置新的值
"redis"
127.0.0.1:6379> get db
"mongdb"

```



数据结构是相同的，未来我们会学习 Jedis，方法在java里和现在的名字是一样的

String的类似使用场景：value除了是字符串还可以是数字

· 计数器

· 统计多单位的数量 例如： uid:123123:follow 0

· 粉丝数

· 对象缓存存储



## List

redis中，可以将list用作栈、队列、阻塞队列的数据结构

所有list命令都是以l开头，不一定哦，比如rpoplpush

```shell
lpush key v1 v2 ...  #将一个值或多个值插入列表的头部(左)
rpush key v1 v2 ...  #将一个值或多个值插入列表的尾部(右)

# 不能get了
```

![list push](/Users/dyq/Desktop/redis/pics/list push.png) 



```shell
lrange key start end  #用过区间获取具体的值  （0 -1 区间获取全部值）
lpop key  #移除列表头部第一个值（左）
rpop key  #移除列表尾部第一个值（右）

lindex key index #通过索引获取值，0开始
llen key   #获取列表长度
lrem key count value  #移除list集合中指定个数的value  精确匹配
ltrim key start stop   #通过下标截取指定长度，list已经改变，只剩下截取后的元素，闭区间
rpoplpush key otherkey  #移除列表中最后一个元素，并将它插入另一个列表头部
lset key index value  #将列表中指定下标的值替换为另外一个值，更新操作 （如果列表或索引不存在  会报错）
linsert key before v1 v2  #在v1前插入v2
linsert key after v1 v2  #在v1后插入v2


```



小结：

- 他实际上是一个链表，before after left right 都可以插入值
- 如果key不存在，创建新的链表
- 如果key存在，新增内容
- **如果移除了所有值，空链表，也代表不存在**
- 在两边插入或改动值，效率最高！中间元素，相对来说效率会低一点！
- 消息排队 消息队列（Lpush Rpop） ，栈（Lpush Lpop）





## Set

set中的值不能重复

无序不可重复

```shell
sadd key value  #添加元素
smembers key   #查看指定set中所用元素
sismember key value  #判断某一个值在指定set中是否存在
scard key  #获取set中的内容元素个数
srem key value   #移除set中指定元素
srandmember key count  #随机选出指定个数的成员
spop key  #随机移除元素
smove oldkey  newkey member  #将一个指定的值，从一个set移动到另一个set
sdiff key...   #获取多个set差集
sinter key...  #获取多个set交集 （共同好友、共同关注）
sunion key...  #获取多个set并集

```

应用场景：微博，将用户所有关注放入一个set，粉丝放入一个set
-> 共同关注、二度好友、相互关注…..    

## Hash

Map集合 

key - map

key - <key,value>

这时候的值是一个map集合，本质和String类型没有太大区别，还是一个简单的key-value



set myhash field 

```shell
hset key field value  #存入一个具体键值对
hget key field   #获取一个字段值
hmset key field value field1 value1 ...  #存入多个具体键值对
hmget key field field1 ...  #获取多个字段值
hgetall key    #获取全部数据
hdel key field  #删除hash指定的key字段，对应value也就没有了
hlen key     #获取hash中字段数量
hexists key field   #判断hash中某个字段是否存在
hkeys key    #获取hash中全部key
hvals key    #获取hash中全部value
hincrby key field 1  #hash中指定key的value加1
hdecrby key field 1  #hash中指定key的value减1
hsetnx key field value   #如果hash中指定key不存在则创建，存在则创建失败

```

hash变更的数据，尤其用户信息，经常变动的信息，hash更适合于对象的存储，string只适合于字符串

例如  

```shell
hset user:1 name du age 11
```



## Zset (有序集合)

有序集合，在set的基础上增加了一个排序的值

```shell
zadd key score value  #添加元素，给score
zrange key 0 1   #通过索引区间返回有序集合指定区间内的成员   （0 -1）返回全部

zrangebyscore key min max   #排序并返回 从小到大  例如：zrangebyscore key1 -inf +inf    （-inf：负无穷   +inf：正无穷 ） withscores可选
ZREVRANGEBYSCORE myset +inf -inf #排序并返回 从小到大 
zrevrange key 0 -1     #排序并返回 从大到小 （按照索引）

zrem key value   #移除指定元素

zcard key        #获取有序集合中的数量
zcount key start stop   #获取指定score区间中的成员数量
```

剩下的API，如果有工作需要，这个时候可以查看官方文档。

案例思路：set 排序，存储班级成绩表，工资表排序

普通消息：1重要信息 2带权重进行判断

排行榜：topN



# 04_三种特殊数据类型

## geospatial 地理位置



地理位置 （定位、附近的人、打车距离……）


GEO在redis3.2就推出了，这个功能可以推算地理位置的信息，两地之间的距离，方圆几里之内的人

http://www.jsons.cn/lngcode/ 可以查一些数据

http://redis.cn/commands/geoadd.html

只有6个命令

```shell
#geoadd 添加地理位置
规则：两极无法直接加入，通常通过配置文件用java一次性导入  有效经度：-180到180  有效纬度：-85.05112878到85.05112878
geoadd china:city 121.47 31.23 shanghai
geoadd china:city 106.50 29.53 chongqing  114.05 22.52 shenzhen 120.16 30.24 hangzhou 108.96 34.26 xian
#geopos 获取指定成员的经度和纬度
GEOPOS china:city chongqing beijin
#geodist 查看成员间的的直线距离
GEODIST china:city beijin shanghai 默认是米
GEODIST china:city beijin shanghai km
#georadius 以给定经纬度为中心，找出某一半径内的元素
（附近的人）
GEORADIUS china:city 110 30 1000 km
GEORADIUS china:city 110 30 1000 km withdist withcoord count 2 （withdist 显示直线距离  withcoord 显示经纬度  count  显示几条）
#georadiusbymember 以给定成员为中心，找出某一半径内的元素
georadiusbymember china:city beijing 1000 km withdist withcoord count 2 （withdist 显示直线距离  withcoord 显示经纬度  count  显示几条）
#geohash 返回一个或多个位置元素的geohash表示  将二维的经纬度转换成一维的11位字符串 如果两个字符串越接近，则距离越近。
127.0.0.1:6379> geohash china:city shanghai
1) "wtw3sj5zbj0"
```

GEO底层就是Zset 可以用Zset命令操作Geo

```shell
127.0.0.1:6379> zrange china:city 0 -1
1) "shanghai"
2) "datong"
127.0.0.1:6379> zrem china:city shanghai
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "datong"
```



## Hyperloglogs (cardinal number)

基数 （不重复的元素个数） 可以接受误差 大概有0.81%的错误率

```
A{1,3,5,7.8.7}
B{1,3,5,7,8}
则A和B的基数=5，基数可以允许误差的
```

Redis hyperloglog 基数统计算法：
优点： 占用内存固定，存放2^64不同的元素的技术，只需要占用12KB内存，则从内存角度来比较的话，hyperloglog首选，但是要忍受81%的错误率。
**网页的UV （一个人访问一个网站多次，统计出还是一个人），user visit**
传统的方式：set集合保存用户id，统计set中用户数量。 但是相对消耗更多内存，我们的目的并不是保存用户id，目的只是计数。

```shell
PFadd key element  #创建一组元素（set，不重复）
PFcount key   #统计元素基数
pfmerge key3 key1 key2   #合并两组key1 key2 => key3  并集

127.0.0.1:6379> PFADD mykey a b c d e f
(integer) 1
127.0.0.1:6379> PFCOUNT myket
(integer) 0
127.0.0.1:6379> PFCOUNT mykey
(integer) 6
127.0.0.1:6379> PFADD mykey1 a b e b (set)
(integer) 1
127.0.0.1:6379> pfcount mykey1
(integer) 3
127.0.0.1:6379> PFMERGE key3 mykey1 mykey0
OK
127.0.0.1:6379> get key3
"HYLL\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x80q\xa6\x84I\x8c\x80Bm\x80BZ"
127.0.0.1:6379> smembers key3
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> pfcount key3
(integer) 3
```

如果不允许容错，就用set或者自己的数据类型就行。



## Bitmaps（位图）

**位存储**
统计用户信息 活跃 不活跃 登录 未登录 打卡

两个状态的 都可以使用bitmaps
bitmaps位图数据结构，都是操作二进制位来进行记录的，非0即1

非常的省空间

例如，统计疫情感染人数，用14亿个0来标识未感染。感染的话就是1。



```shell
setbit key offset value  #设置位图
getbit key offset        #获取指定位图的值
bitcount key     #统计数量
###################################################
例如 一周打卡   0为打卡 1打卡
127.0.0.1:6379> SETBIT sign 0 1 （星期一打卡了）
(integer) 0
127.0.0.1:6379> SETBIT sign 1 0 （星期二没打卡）
(integer) 0
127.0.0.1:6379> SETBIT sign 2 1
(integer) 0
127.0.0.1:6379> SETBIT sign 3 0
(integer) 0
127.0.0.1:6379> SETBIT sign 4 0
(integer) 0
127.0.0.1:6379> SETBIT sign 5 0
(integer) 0
127.0.0.1:6379> SETBIT sign 6 1
(integer) 0
127.0.0.1:6379> GETBIT sign 0
(integer) 1
127.0.0.1:6379> BITCOUNT sign
(integer) 3

```

