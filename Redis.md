## 五大基本数据类型
### String（字符串）
1. Redis最常用的数据类型
2. 常用命令如下，更多的可以参照官方网站具体查询， [redis命令手册](https://www.redis.net.cn/order/)

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
2. 常用基本操作，命令大多以L字母开头：

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
2. 常用命令，通常以S字母开头

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
2. 常用命令，通常以H字母开头，大部分String操作的命令也适用于Hash的操作

| 命令                                    | 描述                  |
| ------------------------------------- | ------------------- |
| hset K field V                        | 在哈希K存入一个键值对field-V  |
| hget K field                          | 取出哈希K中field的值       |
| hmset K field1 V1 field2 V2 field3 V3 | 在哈希K中存入多个键值对field-V |
| hmget K field1 field2                 | 取出哈希K中多个field的值     |
|                                       |                     |
3. 适合存储对象信息并修改，例如用户信息、商品信息等
### Zset

## 三种特殊数据类型
### geospatical
### hyperloglog
### 

