## 五大基本数据类型
### String（字符串）
1. Redis最常用的数据类型
2. 底层实现是动态字符串数组
3. 常用命令如下，更多的可以参照官方网站具体查询， [redis命令手册](https://www.redis.net.cn/order/)

| 命令                                | 描述                              |
| --------------------------------- | ------------------------------- |
| set K V                           | 设置值                             |
| get K                             | 获得值                             |
| keys *                            | 获取所有的key                        |
| exists K                          | key是否存在                         |
| append K V                        | 追加字符串，如果K不存在，相当于set K V         |
| strlen K                          | 获得字符串的长度                        |
| incr K                            | 使得K自加1 ，**常见应用就是浏览量等**          |
| decr K                            | 使得K自减1                          |
| incrby K 步长                       | 使得K按照步长增加                       |
| decrby K 步长                       | 使得K按照步长减小                       |
| getrange K start end              | 截取K字符串的值，下标从0开始， 0 -1 代表全字符串    |
| setrange K offset str             | 字符串替换，从offset开始用str替换原字符串       |
| setex （set with expire）K expire V | 给K设置过期时间，单位秒                    |
| setnx （set if not exist） K V      | 不存在，再设置,否则不做任何操作。**多用于分布式锁实现**。 |
| mset K1 V1 K2 V2...               | 批量设置值                           |
| mget k1 k2 k3...                  | 批量获取值                           |
| msetnx k1 v1 k2 v2...             | 原理同setnx，但是原子性操作，要么一起成功，要么一起失败  |
| getset K V                        | 先获取值，再重新赋值。K为空即为直接赋值            |
3. 使用场景：
	1. 计数器，统计网站浏览量、点击量、粉丝数等
	2. JSON对象缓存存储
	3. 分布式锁实现


### List（列表）
1. 基本数据类型，列表，实际上是一个链表实现，可以从左右插入删除。可以当成队列、栈来使用。
2. 底层实现是链表，早期实现是压缩列表和双向链表，后期设计是quicklist，即压缩列表和双向链表的一种混合设计。**将 linkedlist 按段切分，每一段使用 ziplist 来紧凑存储，多个 ziplist 之间使用双向指针串接起来。**
3. 常用基本操作，命令大多以L字母开头：

| 命令                 | 描述                                 |
| ------------------ | ---------------------------------- |
| lpush K V1 V2...   | 将一个或者多个值放入K列表中，左侧压入列表              |
| rpush K V1 V2...   | 将一个或者多个值放入K列表中，右侧压入列表              |
| lpop K             | 移除K列表的第一个元素并返回                     |
| rpop K             | 移除K列表的最后一个元素并返回                    |
| lrange K start end | 将K列表中的元素按照区间来倒序取出，其中 0 -1 代表取出所有元素 |
| lindex K offset    | 返回K列表下标的具体offset的元素                |
| llen K             | 返回K列表的长度                           |
| lrem K count value | 移除K列表中指定count数的value，精确匹配删除        |
| ltrim K start end  | 将K列表按照下标起始位置进行截断                   |
| rpoplpush K K1     | 将K列表尾部的元素移除并添加到K1列表的头部，如下展示        |
```bash
127.0.0.1:6379> LRANGE testlist 0 -1
1) "c"
2) "b"
3) "a"
4) "d"
127.0.0.1:6379> LRANGE newlist 0 -1
1) "three"
2) "tow"
3) "one"
127.0.0.1:6379> rpoplpush testlist newlist
"d"
127.0.0.1:6379> LRANGE testlist 0 -1
1) "c"
2) "b"
3) "a"
127.0.0.1:6379> LRANGE newlist 0 -1
1) "d"
2) "three"
3) "tow"
4) "one"
127.0.0.1:6379>
```


| 命令                                    | 操作                                      |
| ------------------------------------- | --------------------------------------- |
| lset K index value                    | 将K列表中index下标的元素替换为value，若列表不存在或者下标越界则报错 |
| linsert K before/after value newvalue | 在K列表value元素前后插入newvalue                 |
3. 使用场景
	1. 可以实现队列、栈等


### Set（集合）
1. 和Java的set类似，是不能重复的集合
2. 底层编码采用intset或者hashtable实现
3. 常用命令，通常以S字母开头

| 命令              | 操作                 |
| --------------- | ------------------ |
| sadd K V1 V2... | 向集合K中添加元素，支持复数个    |
| smembers K      | 列举出集合K中的所有元素       |
| scard K         | 返回集合K中元素的个数        |
| srem K V        | 移除集合K中的元素V         |
| sdiff K K1      | K与K1集合之间求差集，返回K的差集 |
| sinter K K1     | K与K1集合求交集          |
| sunion K K1     | K与K1集合求并集          |
求差集
```bash
127.0.0.1:6379> SMEMBERS myset
1) "sty"
2) "james"
3) "styjames"
4) "shaotianyu"
127.0.0.1:6379> SMEMBERS mynewset
1) "sty"
2) "stty"
3) "james"
4) "Sty"
127.0.0.1:6379> SDIFF myset mynewset
1) "styjames"
2) "shaotianyu"
127.0.0.1:6379> SDIFF  mynewset myset
1) "stty"
2) "Sty"
127.0.0.1:6379> 
```

求交集
```bash
127.0.0.1:6379> SINTER myset mynewset
1) "sty"
2) "james"
127.0.0.1:6379> 
```

求并集
```bash
127.0.0.1:6379> SUNION myset mynewset
1) "sty"
2) "james"
3) "styjames"
4) "shaotianyu"
5) "stty"
6) "Sty"
127.0.0.1:6379> 
```

3. 常用场景：
	1. 社交APP的共同关注、共同爱好、推荐好友等
### Hash（哈希）
1. Map集合应用，Key-Map集合，主要用于存储对象信息。可以理解为String处理的拓展
2. hash的底层实现是dict，编码采用压缩列表或者hashtable来实现
3. 常用命令，通常以H字母开头，大部分String操作的命令也适用于Hash的操作

| 命令                                    | 描述                                |
| ------------------------------------- | --------------------------------- |
| hset K field V                        | 在哈希K存入一个键值对field-V                |
| hget K field                          | 取出哈希K中field的值                     |
| hmset K field1 V1 field2 V2 field3 V3 | 在哈希K中存入多个键值对field-V               |
| hmget K field1 field2                 | 取出哈希K中多个field的值                   |
| hgetall K                             | 返回哈希K中所有的键值对                      |
| hlen K                                | 返回哈希K中的键值对个数                      |
| hexists K field                       | 判断哈希K中的某个键是否存在                    |
| hkeys K                               | 返回哈希K中所有的key                      |
| hvals K                               | 返回哈希K中所有的value                    |
| hincrby K field number                | 指定增程,对哈希K中的field进行增量加减            |
| hsetnx K field V                      | 如果存在则不能赋值，不存在则可以赋值。**也可用于分布式锁设计** |
3. 适合存储经常变动的对象信息，例如用户信息、商品信息等
### Zset（有序集合）
1. 在set基础上，增加了一个值作为排序依据。
2. 底层编码采用ziplist或者skiplist，
3. 常用命令，以Z字母作为开头

| 命令 | 描述 |
| ---- | ---- |
| zadd K score V | 可放置多个，score代表顺序，将V放在K有序集合的score位置 |
| zrange K start end | 按照顺序将K有序集合从start到end展示 |
| ZRANGEBYSCORE K -inf +inf | 按照score顺序的顺序输出K有序集合中的元素 |
| ZREVRANGE K start end | 按照score倒序的顺序输出K有序集合中的元素 |
| zcount K start end | 返回开始结束范围内的K有序集合的个数 |
| zrem K V | 移除K中的元素V |
| zcard K | 返回K有序集合中的元素 |

3. 常见使用场景，各种表单的排序、带权重进行判定、排行榜、取top N等
## 三种特殊数据类型
### geospatical 地理位置
1. **同时geo是基于Zset实现的，所以可以直接使用Zset命令来对geo进行操作**
2. 常用命令如下：

| 命令 | 描述 |
| ---- | ---- |
| geoadd K 经度 纬度 名称 | 添加地理数据，不包含两级。- 有效的经度从-180度到180度。<br>有效的纬度从-85.05112878度到85.05112878度 |
| geopos K 名称 | 返回某个指定名称的经纬度 |
| geodist K 名称1 名称2 单位 | 返回名称1和名称2之间的单位距离，- **m** 表示单位为米。<br>- **km** 表示单位为千米。<br>- **mi** 表示单位为英里。<br>- **ft** 表示单位为英尺。 |
| GEORADIUS K 经度 纬度 范围 | 以给定的经纬度为中心， 找出某一半径内的地区 |
| GEOHASH K 名称 | 返回一个或多个地区的经纬度哈希表示。11个字符的Geohash字符串，不会丢失精度。 |
|  GEORADIUSBYMEMBER K 名称 距离 | 以名称为中心，找出距离半径内的其他地区。 |
3. 常用于朋友定位、附近的人、打车距离等，可以推算地理位置的信息。
### hyperloglog
1. 用于基数统计，可以用于统计各种点击量等，统计规则是基于概率完成的，误差可容忍的情况下可以使用。
2. 常用命令如下：

| 命令 | 描述 |
| --- | --- |
|  |  |

### bitmap
1. 位图，统计用户信息：比如活跃/不活跃，登陆/不登录，或者每日打卡，两种状态的统计都可以使用位图的方式。
2. 常用命令如下：

| 命令 | 描述 |
| --- | --- |
|  |  |


## redis设计
1. 内存存储，内存存储没有额外的磁盘开销
2. Redis的网络模型采用单线程来处理分发网络请求，没有线程竞争
3. 采用非阻塞的IO（多路复用）+事件处理模型，减少网络IO消耗
4. 数据结构设计都是可直接应用的成熟设计，底层支持好
5. 自己构建了新的虚拟内存交换技术，保持热数据一直在内存中

## redis的持久化机制
### AOF和RDB
1. RDB是指定时间间隔的一个内存快照，恢复时将快照直接存储到内存中
2. AOF是只追加文件，将每一个redis命令实时写入日志中。根据参数可以分为实时同步、秒级同步、不同步三种
3. 可以根据数据的敏感性、实时性要求来判断使用何种持久化机制。如果用作内存数据库，可以考虑两个都开启，RDB用作数据备份，AOF保证数据不丢失。但是相应的会出现运行效率降低和存储空间的占用。
