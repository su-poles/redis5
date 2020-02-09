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



# 三 Redis启动

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



# 四 Redis基本默认配置

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
	 分别表示900秒（15分钟）内有1个更改、300秒（5分钟）内有10个更改以及60秒内有10000个更改时同步数据文件。
```

```
10. 指定存储至本地数据库时是否压缩数据，默认为yes, Redis采用LZF压缩算法进行压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大。
		rdbcompression yes
```

```
11.指定本地数据库文件名，默认值为dump.rdb
	 dbfilename dump.rdb
```

```
12.指定本地数据库存放目录
	 dir ./
```

```
13.设置当本机为slave服务时，设置master服务器的IP地址及端口，在Redis启动时，它会自动从master进行数据同步
	 slaveof <masterip> <masterport>
```

```
14.当master服务设置了密码保护时，salve连接master的密码
	 masterauth	<master-password>
```

```
15.设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过auth <password>命令提供密码，默认是关闭的
	 redis的密码设置时要很长很复杂，因为一秒之内可以尝试大约150000个密码，所以尽可能复杂点
	 requirepass <password> 
```

```
16.设置同一时间最大客户端连接数，默认无限制。Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置maxclients 0，表示不作限制。当客户端连接数达到限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息
	 maxclients 128
```

```
17.指定Redis最大内存限制。Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清楚已到期或即将到期的Key，当此方法处理后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis的新的VM机制，会把Key存放在内存，Value值存放在swap区
	 maxmemory <bytes>
```

```
18.开启aof备份
	 appendonly yes   #默认为no
```

```
19.指定aof备份文件名
	 appendfilename appendonly.aof
```

```
20.指定aof备份的频率，有三种可选项：
	 appendfsync always      #每次收到写命令，就立即写入磁盘
	 appendfsync everysec    #每秒钟同步一次，从aof缓冲区写入磁盘日志文件
	 appendfsyne no          #redis不主动同步写入磁盘，依赖OS的写入策略，一般是30秒写入一次。如果发生意外，会有较长时间的数据丢失
	 
	 提问：如果1秒钟执行的命令太多，导致aof缓冲区溢出，怎么办？
```

```
21.指定是否启用虚拟内存机制，默认值为no，简单的介绍一下，VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中
	 vm-enabled no
```

```
22.虚拟内存文件路径，默认值为/tmp/redis.swap，不可以死多个Redis实例共享
	 vm-swap-file /tmp/redis.swap
```

```
23.将所有大于vm-max-memory的数据存入虚拟内存，无论vm-max-memory设置多小，所有索引数据都是内存存储的（Redis的索引数据，就是keys）,也就是说，当vm-max-memory设置为0的时候，其实是所有的value都存在于磁盘，默认值为0
	 vm-max-memory 0
```

```
24.Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的数据大小来设定的，作者建议如果存储很多小对象，page大小最好设置为32或者64bytes；如果存储很多大对象，则可以使用更大的page，如果不确定，就使用默认值
	vm-page-size 32
```

```
25.设置swap文件中的page数量，由于页表(一种表示页面空闲或使用的bitmap)是存放在内存中的，在磁盘上每8个pages将消耗1byte的内存
	 vm-pages 134217728
```

```
26.设置访问swap文件的线程数。最好不要超过机器的核数，如果设置为0，那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值是4
	 vm-max-threads 4
```

```
27.向客户端应答时i，是否把较小的包合并为一个包发送，默认为开启
	 glueoutputbuf yes
```

```
28.指定在超过一定数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法
	 hash-max-zipmap-entries 64
	 hash-max-zipmap-value 512
```

```
29.指定是否激活重置哈希。默认为开启
	 activerehasing yes
```

```
30.指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件
	 include /path/to/local.conf
```



# 五 Redis的内存维护策略

redis作为优秀的中间缓存件，时常会存储大量的数据，即使采用了集群部署来动态扩容，也应该及时的整理内存，维持系统性能。

**在Redis中有两种解决方案**

**1. 设置过期时间**

```
expire key <seconds>
pexpire key <
milliseconds>
setex(String key, int seconds, String value)

expireat key <timestamp>    #将键的过期时间设置为unix时间戳（10位）
pexpireat key <timestamp>   #将键的过期时间设置为13位的long类型时间戳

相关命令：
ttl					#剩余生存时间，以秒为单位返回
pttl				   #剩余生存时间，以毫秒为单位返回
persist				#删除过期时间 
```

> - 除了字符串有自己独有的设置过期时间的方式外（setex），其它数据类型都要依靠expire或pexpire来设置过期时间
> - 如果没有设置过期时间，那么缓存内容永不过期
> - 如果设置了过期时间，又想让缓存永不过期，使用persist key

**2. 采用LRU算法动态清除数据**

> 内存管理的一种页面置换算法，对于在内存中但又不用的数据块（内存块）叫做LRU，操作系统会根据哪些数据属于LRU而将其移出内存，进而腾出空间来加载另外的数据。

1. **volatile-lru**：设定超时时间的数据中，删除最不常用的数据
2. **allkeys-lru**：查询所有的key中最近最不常使用的数据进行删除，这是应用最广泛的策略
3. volatile-random：在设置了过期时间的keys中，随机删除
4. allkeys-random：在所有key中，随机删除
5. volatile-ttl：在设置了过期时间的key中，删除马上要过期的数据。要采取这个策略，缓存对象的**TTL**值最好有差异
6. noeviction：不删除任何操作，一旦发现内存溢出，则直接报错，并返回错误信息
7. volatile-lfu：在所有设置了过期时间的key中，驱逐使用频率最少的键
8. allkeys-lfu：从所有键中驱逐使用频率最少的键

参考文档：  
[缓存常用淘汰算法](docs/缓存常用淘汰算法.md)  
[Redis中的LRU淘汰策略分析](docs/Redis中的LRU淘汰策略分析.md)  
[Redis缓存淘汰简要总结](docs/Redis缓存淘汰简要总结.md)  

