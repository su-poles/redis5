# 一 Redis简介
## 1.0 Redis简介

### 什么是Redis

Redis是完全开源免费的，遵守**BSD**协议，是一个**高性能**（NoSQL）的key-value数据库，Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可以持久化的日志型、key-value数据库，并提供多种语言的API。

> BSD是"Berkeley Software Distribution"的缩写，意思是"伯克利软件发行版"。
>
> BSD开源协议是一个给予使用者很大自由的协议。可以自由的使用，修改源代码，也可以将修改后的代码作为你开源或者专有软件再发布。BSD协议鼓励代码共享，但需要尊重代码作者的著作权。
>
>   
>
> BSD由于允许使用者修改和重新发布代码，也允许使用或在BSD代码上开发商业软件发布和销售，因此是对商业集成很友好的协议。



## 1.1 NoSQL

```css
NoSQL，泛指非关系型的数据库，NoSQL即Not-Only SQL, 它可以作为关系型数据库的良好补充。随着互联网web2.0网站的兴起，非关系型的数据库现在成了一个及其热门的新领域，非关系型数据库产品的发展非常迅速。
```

而传统的关系型数据库在应付web2.0网站，特别是**超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题**，例如：

1、High performance - <font color=#FF0000>对数据库高并发读写的需求</font>

web2.0，网站要根据用户个性化信息来实时生成动态页面和提供动态信息，所以基本上无法使用动态页面静态化技术，因此数据库并发负载非常高，往往要达到每秒上万次读写请求。关系数据库应付上万次SQL查询还勉强顶得住，但是应付上万次SQL写数据请求，硬盘IO就已经无法承受了，其实对于普通的BBS网站，往往也存在对高并发写请求的需求，例如网站的实时统计在线用户状态，记录热门帖子的点击次数，投票计数等，因此这是一个相当普遍的需求。

2、Huge Storage - <font color=#FF0000>对海量数据的高效率存储和访问的需求</font>

类似Facebook、Twitter，Friendfeed这样的SNS网站，每天用户产生海量的用户动态，以Friendfeed为例，一个月就达到了2.5亿条用户动态，对于关系型数据库来说，在一张2.5亿条记录的表里面进行SQL查询，效率是极其低下乃至不可忍受的。再例如大型web网站的用户登录系统，例如腾讯、盛大等动辄以亿计的账户，关系型数据库也很难应付。

3、High Scalability &&  High Avalilability - <font color=#FF0000>对数据库的高可扩展性和高可用性的需求</font>

在基于web的架构中，数据库是最难进行横向扩展的，当一个应用系统的用户量和访问量与日俱增的时候，你的数据库却没有办法像web server和app server那样，简单的通过添加更多的硬件和服务节点，来扩展性能和负载能力，对于很多需要提供24小时不间断服务的网站来说，对数据库系统进行升级和扩展是非常痛苦的事情，往往需要停机维护和数据迁移，为什么数据库不能通过不断的添加服务器节点来实现扩展呢？

**NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。**



## 1.2 NoSQL的类别

#### 键值（key-value）存储数据库

```css
这一类数据库主要会使用到一个哈希表，这个表中有一个特定的键和一个指针指向特定的数据。Key/Value模型对于IT系统来说，优势在于简单、易部署。但是如果DBA值对部分值进行查询或更新的时候，Key/Value就显得效率低下了。

相关产品：Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
典型应用：内容缓存，主要用于处理大量数据的高访问负载。
数据模型：一系列键值对
优势：快速查询
劣势：存储的数据缺少结构化
```

#### 列存储数据库

```css
这部分数据库通常用来应对分布式存储的海量数据。键仍然存在，但是他们的特点是指向了多个列。这些列是由列家族来安排的。

相关产品：Cassandra、HBase、Riak
典型应用：分布式的文件系统
数据模型：以列簇式存储，将同一列数据存在一起
优势：查找速度快，可扩展性强，更容易进行分布式扩展
劣势：功能相对局限
```

#### 文档型数据库

```css
文档型数据库的灵感是来自于Lotus Notes办公软件，它同第一种键值存储相类似。该类型的数据模型是版本化的文档，半结构化的文档以特定的格式存储，比如JSON。文档型数据库可以看作是键值数据库的升级版，允许之间嵌套键值。而且文档数据库比键值数据库的查询效率更高。

相关产品：CouchDB、MongoDB
典型应用：Web应用（与Key-Value类似，Value是结构化的）
数据模型：一系列键值对
优势：数据结构要求不严格
劣势：查询性能不高，而且缺乏统一的查询语法
```

#### 图像(Graph)数据库

```css
图形结构的数据库同其它行列以及刚性结构的SQL数据库不同，它是使用灵活的图形模型，并且能够扩展到多个服务器上。NoSQL数据库没有标准的查询语言（SQL)，因此进行数据库查询需要制定数据模型。许多NoSQL数据库都是REST风格的数据接口或者查询API。

相关数据库：Neo4J、InfoGrid、Infinite Graph
典型应用：社交网络
数据模型：图结构
优势：利用图结构相关算法
劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案
```



### 总结

因此，我们总结NoSQL数据库在以下的这几种情况下比较适用：

1. 数据模型比较简单
2. 需要灵活性更强的IT系统
3. 对数据库性能要求较高
4. 不需要高度的数据一致性
5. 对于给定key，比较容易映射复杂值的环境



## 1.3 Redis历史

```
2008年由意大利的一家创业公司Merzia的创始人Salvatore Sanfilippo开发的。 2009年开发完成并开源。代码主要贡献者还有：Pieter Noordhuis.

2010年，VMware赞助Redis的开发，这俩货后来就去VMware全职做redis开发了。好好的老板不做，成了一个牛逼的打工者。当然作者还住在意大利卡塔尼亚，老家是西西里岛。

redis开源地址：antirez.com
redis作者的github地址：github.com/antirez
```



## 1.4 Redis特点

- **性能极高** - 读速度11万次/秒，写速度81000次/秒（参考百度百科）
- **丰富的数据类型** - 5种基本数据类型及一些高级数据类型
- **原子性** - 多个操作也支持事务，通过 **<font color=#FF0000>MULTI</font>** 和**<font color=#FF0000> EXEC </font>**指令包起来
- **丰富的特性** - 如publish/subscribe、键值过期等特性
- **高速读写** - redis使用自己实现的分离器，代码量很短，没有使用lock，因此效率非常高



## 1.5 Redis的应用场景

1. 缓存 - 键过期、 **<font color=#FF0000>键淘汰策略</font>** 

2. 排行榜 - Sorted Set

3. 计数器 - **<font color=#FF0000>incr</font>** (+1)、**<font color=#FF0000>incrby</font>** (+n)

4. 分布式会话 - 即session管理器。

   ```css
   在集群模式下，在应用不多的情况下一般使用容器自带的session复制功能就能满足，当应用增多变成相对复杂的系统时，一般都会搭建以redis等内存数据库为中心的session服务，session由session服务及内存数据库管理，而不再由容器来管理。
   ```

5. 分布式锁 - set、setnx (具体场景包括：全局ID、减库存、秒杀)

6. 社交网络 - 具体场景如：点赞、踩一下、关注(回粉)/取关、被关注。使用hash、set等数据结构可以方便的实现这些功能。

7. 最新列表 -  list

   ```css
   lpush插入一个内容ID，ltrim可以用来限制列表的数量，这样列表永远为N个ID。所以查询最新的N个记录时，直接根据ID取到对应的内容即可。
   ```

8. 消息系统 - 具体场景如：业务解耦、流量削峰、异步处理等。可以通过list或者发布订阅来实现简单的消息队列系统。 

 

## 1.6 Redis缺点

- **持久化**  - snapshot（备份慢）、aof（恢复慢）
- **耗内存** - 占用内存过高。 redis可以通过redis.config文件设置最大内存，超过该内存时，写操作被拒绝，读操作正常。



# 二 Redis的安装

## 2.1 安装前的准备

官网：[redis.io](http://redis.io)

(io为名国家域名，英属印度洋领地，即British Indian Ocean territory)

## 2.2 Redis源码编译安装

```assembly
打开https://redis.io/download，查看安装步骤。

如需安装gcc，则使用root执行：
yum -y install gcc automake autoconf libtool make

如果yum运行时出现/var/run/yum.pid已被锁定（PID为xxxx的另一个程序正在运行），则执行：
rm -f /var/run/yum.pid

如果出现：
cc: 错误: ../deps/lua/src/liblua.a: 没有哪个文件或目录
进入deps目录并执行命令：
cd /opt/redis-5.0.0/deps
make lua hiredis linenoise
```

### 指定安装目录

```assembly
make PREFIX=/usr/local/redis install
```



## 三 Redis启动

```
 ./bin/redis-server
```

**启动redis客户端连接redis服务**

```
redis-cli -h IP地址 -p 端口
```

退出客户端

```
Ctrl+c
```



## 四 Redis基本默认配置

```
1. Redis默认不是以守护线程方式运行，如若修改，配置如下：
   daemonize yes
```

```
2. 当redis以守护进程的方式运行时，Redis会默认把pid写入/var/run/redis.pid，如要指定pid文件，配置如下：
   pidfile /var/run/redis.pid
```

```
3. redis默认端口号为6379，对应手机按键MERZ，MERZ是意大利歌女Alessia Merz的名字
   port 6379
```

```
4. 绑定的主机地址
	 bind 127.0.0.1
```

```
5. 当客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能
   timeout 300
```

```
6. 指定日志爱记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
   loglevel verbose
```

```
7. 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式执行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null (/dev/null表示的是一个黑洞，通常用于丢弃不需要的数据输出，或者用于输入流的空文件)
	 logfile stdout
```

```
8. 设置数据库的数量，即默认有16个数据库，默认数据库为0，可以使用select <dbid> 命令切换数据库
   databases 16
```

```
9. 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件一起配合
	 save <seconds> <changes>
	 Redis默认配置文件中提供了三个条件
	 save 900 1
	 save 300 10
	 save 60 10000
	 分别表示900秒（15分钟）内有1个更改、300秒（5分钟）内由10个更改以及60秒内有10000个更改时同步数据文件。
```

```
10. 指定存储至本地数据库时是否压缩数据，默认为yes, Redis采用LZF压缩算法进行压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大。
		rdbcompression yes
```

```
11.
```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

