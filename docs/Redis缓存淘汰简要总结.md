# Redis缓存淘汰简要总结

- **人工删除key**

- **给key设置过期时间，被动删除key。（访问时被删除）**

- **redis每10s随机选20个key，删除其中已过期的。**

> 在主动删除中Redis默认都会每隔10秒去遍历一次设置了过期key的那个集合，首先随机选出20个key，然后删除其中已经过期的key，如果过期key的比例超过当前20个key的四分之一那么就循环到第一步直到条件不满足退出。同时为了保证像我们之前担心的线程阻塞的问题，算法还加了超时时间（默认25ms），超时也会退出。也就是说恰好当客户端碰上贪心策略清理过期key，那么意味着它最大只会等待25ms。

> 在此需要注意的是客户端的超时时间最好根据这个时间的值来参考设置，不然话当恰好遇到大量需要淘汰的key， 客户端超时时间<算法最大阻塞时间的情况,，客户端就会在短时间出现大量连接超时。而且你在Redis的slowlog中也找不到日志，因为慢查询是指的逻辑处理过程不包含等待时间

> 当缓存稀疏到一定程度，过期数据的删除就相对比较缓慢了

> **当有大量缓存集中失效的话就会导致缓存服务持续卡顿**，不能正常的对外提供服务，严重点的话就是传说中的“缓存雪崩”，服务持续不可用是很恐怖的。为了防止缓存集中过期，那么设置过期时间时可以给key给一个可接受的随机范围，比如：
>
> expire(key, random.nextInt(1600) + 6400)

- **缓存淘汰策略，是结合maxmemory来设定的。 只有当maxmemory超标的时候，才会触发缓存淘汰策略**



### <font style="color:red">DEL</font> vs <font style="color:red">UNLINK</font>

```
del命令由主进程执行，删除key和value，如果value值很大，那么删除可能会造成暂停卡顿，如果超过25ms，则会导致其它访问redis的连接超时。所以这个删除命令具有极强的杀伤力。

redis4.0以后提供了一个unlink命令，可以异步删除键/值。

当使用unlink删除时，redis先要做个评估（调用lazyfreeGetFreeEffort()方法来评估），它会根据返回的这个effort的值来判断是否要（默认阈值为64）做个异步删除。如果是字符串类型，则这个effort恒等于1，所以对于string类型，unlink也是直接删除的。
```

>1. 释放掉Expire Dict 对 K-V 的引用
>2. 释放Main Dict 对 K-V 的引用，同时记录下这个K-V 的 Entry地址
>3. 计算释放掉这个V 所需要的代价，计算方法如下： 
>   3.1 如果这个V 是一个 String 类型，则代价为 1 
>   3.2 如果这个V 是一个复合类型，则代价为 该复合类型的长度，比如，list 则为 llen 的结果，hash 则为 hlen 的结果 …
>4. 根据得到的代价值，和代价阈值比对，如果小于 64 则，可以直接释放掉K-V 内存空间；如果大于 64 则，把该V 放入lazyfree 队列中，同时启动一个BIO后台JOB进行删除 
>   4.1 在后台线程对 V 进行删除时，也是根据不同类型的 V 做不同的操作 
>   4.2 如果是 LIST 类型，则根据LIST 长度，则直接释放空间。 
>   4.3 如果是 SET 类型，并且数据结构采用 HASH 表存储，那么遍历整个hash表，逐个释放 k，v空间；如果数据结构采用 intset，则直接释放空间即可 
>   4.4 如果是 ZSET 类型，并且数据结构采用 SKIPLIST 存储，由于 SKIPLIST 底层采用 HASH + skiplist 存储，那么会先释放掉 SKIPLIST 中 hash 存储空间，再释放掉 SKIPLIST 中 skiplist 部分； 如果数据结构采用 ZIPLIST 存储，则直接释放空间。 
>   4.5 如果是 HASH 类型，并且数据结构采用 HASH表存储，则遍历整个hash表，逐个释放 k，v空间；如果数据结构采用 ZIPLIST 存储，则直接释放空间。
>5. 设置 V 值等于NULL
>6. 释放掉 K-V 空间

简单解读一下：

​	当执行unlink时，先从主字典表中去掉这个键的引用，所以后续的查询时查不到的。然后判断如果需要异步删除，则将创建一个后台删除任务，加入到Redis的任务队列中去。 剩下的就是根据不同的类型、存储结构的不同，有针对性的释放响应的内存空间。



unlink一个大Key时是在Master节点上进行的，如果有Slave节点会怎么样呢？

```
当执行UNLINK操作时，实际上在 AOF 和 通知Slave的时候只是发送了一条DEL key 命令。
```