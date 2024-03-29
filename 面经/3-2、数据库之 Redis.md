

# 3-2　Redis

## 1、Redis 简介

Redis 就是一个**使用 C 语言开发的数据库**，不过与传统数据库不同的是 **Redis 的数据是存在内存中**的 ，也就是它是内存数据库，所以读写速度非常快，因此 Redis 被广泛应用于缓存方向。

Redis 除了做缓存之外，也经常用来做分布式锁，甚至是消息队列。

## 2、Redis 数据结构

Redis 主要有 5 种基础数据结构和 3 种特殊数据结构：

- **5 种基础数据结构** ：String（字符串）、List（列表）、Set（集合）、Hash（散列）、Zset（有序集合）。
- **3 种特殊数据结构** ：HyperLogLogs（基数统计）、Bitmap （位存储）、Geospatial (地理位置)。

> + Redis 的使用场景？在项目中用到了哪些？
> + Redis 的数据类型和底层数据结构？

### 2.1 String（字符串）

#### 简介

`String` 是 Redis 中最简单同时也是最常用的一个数据结构，是一种 **二进制安全的数据结构**，可以用来存储任何类型的数据，比如**字符串**、**整数**、**浮点数**、**图片**（图片的 base64 编码或者解码或者图片的路径）、**序列化后的对象**。

![image-20220809101317038](https://img.zxdmy.com/2022/202208091013460.png)

> 虽然 Redis 是用 C 语言写的，但是 Redis 并没有使用 C 的字符串表示，而是自己构建了一种 **简单动态字符串**（Simple Dynamic String，**SDS**）。相比于 C 的原生字符串，Redis 的 SDS 不光可以保存文本数据还可以保存二进制数据，并且获取字符串长度复杂度为 O(1)（C 字符串为 O(N)），除此之外，Redis 的 SDS API 是安全的，不会造成缓冲区溢出。

#### 应用场景

**需要存储常规数据的场景**

- 举例 ：缓存 session、token、图片地址、序列化后的对象（相比较于 Hash 存储更节省内存）。
- 相关命令 ： `SET`、`GET`。

**需要计数的场景**

- 举例 ：用户单位时间的请求数（简单限流可以用到）、页面单位时间的访问数。
- 相关命令 ：`SET`、`GET`、 `INCR`、`DECR` 。

### 2.2 List（列表）

#### 简介

Redis 中的 `List` 是 **链表** 数据结构的实现，是一个  **双向链表**，即可以支持反向查找和遍历，更方便操作，不过带来了部分额外的内存开销。

![image-20220809101305554](https://img.zxdmy.com/2022/202208091013648.png)

![image-20220809101051718](https://img.zxdmy.com/2022/202208091010047.png)

#### 应用场景

**信息流展示**

- 举例 ：最新文章、最新动态。
- 相关命令 ： `LPUSH`、`LRANGE`。

**消息队列**

Redis List 数据结构可以用来做消息队列，只是功能过于简单且存在很多缺陷，不建议这样做。

相对来说，Redis 5.0 新增加的一个数据结构 `Stream` 更适合做消息队列一些，只是功能依然非常简陋。和专业的消息队列相比，还是有很多欠缺的地方比如消息丢失和堆积问题不好解决。

### 2.3 Hash（哈希）

#### 简介

Redis 中的 `Hash` 是一个 **String 类型的 field-value（键值对） 的映射表**，特别适合用于存储对象，后续操作的时候，可以直接修改这个对象中的某些字段的值。

![image-20220809101241885](https://img.zxdmy.com/2022/202208091012270.png)

#### 应用场景

**对象数据存储场景**

- 举例 ：用户信息、商品信息、文章信息、购物车信息。
- 相关命令 ：`HSET` （设置单个字段的值）、`HMSET`（设置多个字段的值）、`HGET`（获取单个字段的值）、`HMGET`（获取多个字段的值）

### 2.4 Set（集合）

#### 简介

Redis 中的 `Set` 类型是一种无序集合，集合中的元素没有先后顺序但都唯一，有点类似于 Java 中的 `HashSet` 。当你需要存储一个列表数据，又不希望出现重复数据时，Set 是一个很好的选择，并且 Set 提供了判断某个元素是否在一个 Set 集合内的重要接口，这个也是 List 所不能提供的。

你**可以基于 Set 轻易实现交集、并集、差集的操作**，比如你可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。这样的话，Set 可以非常方便的实现如共同关注、共同粉丝、共同喜好等功能。这个过程也就是求交集的过程。

![image-20220809101428794](https://img.zxdmy.com/2022/202208091014953.png)

#### 应用场景

**需要存放的数据不能重复的场景**

- 举例：网站 UV 统计（数据量巨大的场景还是 `HyperLogLog`更适合一些）、文章点赞、动态点赞等场景。
- 相关命令：`SCARD`（获取集合数量） 。

![img](https://img.zxdmy.com/2022/202208091015575.png)

**需要获取多个数据源交集、并集和差集的场景**

- 举例 ：共同好友(交集)、共同粉丝(交集)、共同关注(交集)、好友推荐（差集）、音乐推荐（差集） 、订阅号推荐（差集+交集） 等场景。
- 相关命令：`SINTER`（交集）、`SINTERSTORE` （交集）、`SUNION` （并集）、`SUNIONSTORE`（并集）、`SDIFF`（交集）、`SDIFFSTORE` （交集）。

![img](https://img.zxdmy.com/2022/202208091015971.png)

**需要随机获取数据源中的元素的场景**

- 举例 ：抽奖系统、随机。
- 相关命令：`SPOP`（随机获取集合中的元素并移除，适合不允许重复中奖的场景）、`SRANDMEMBER`（随机获取集合中的元素，适合允许重复中奖的场景。

### 2.5 Zset（Sorted Set，有序集合）

#### 简介

Sorted Set 类似于 Set，但和 Set 相比，Sorted Set 增加了一个权重参数 `score`，使得集合中的元素能够按 `score` 进行有序排列，还可以通过 `score` 的范围来获取元素的列表。有点像是 Java 中 `HashMap` 和 `TreeSet` 的结合体。

![image-20220809101547131](https://img.zxdmy.com/2022/202208091015570.png)

#### 应用场景

**需要随机获取数据源中的元素根据某个权重进行排序的场景**

- 举例 ：各种排行榜比如直播间送礼物的排行榜、朋友圈的微信步数排行榜、王者荣耀中的段位排行榜、话题热度排行榜等等。
- 相关命令 ：`ZRANGE` (从小到大排序) 、 `ZREVRANGE` （从大到小排序）、`ZREVRANK` (指定元素排名)。

### 2.6 Bitmap

#### 简介

Bitmap 存储的是连续的二进制数字（0 和 1），通过 Bitmap, 只需要一个 bit 位来表示某个元素对应的值或者状态，key 就是对应元素本身 。我们知道 8 个 bit 可以组成一个 byte，所以 Bitmap 本身会极大的节省储存空间。

你可以将 Bitmap 看作是一个存储二进制数字（0 和 1）的数组，数组中每个元素的下标叫做 offset（偏移量）。

![image-20220809101645390](https://img.zxdmy.com/2022/202208091016697.png)

#### 应用场景

**需要保存状态信息（0/1 即可表示）的场景**

- 举例 ：用户签到情况、活跃用户情况、用户行为统计（比如是否点赞过某个视频）。
- 相关命令 ：`SETBIT`、`GETBIT`、`BITCOUNT`、`BITOP`。

### 2.7 HyperLogLog

#### 简介

HyperLogLog 是一种有名的基数计数概率算法 ，基于 LogLog Counting(LLC)优化改进得来，并不是 Redis 特有的，Redis 只是实现了这个算法并提供了一些开箱即用的 API。

Redis 提供的 HyperLogLog 占用空间非常非常小，只需要 12k 的空间就能存储接近`2^64`个不同元素。这是真的厉害，这就是数学的魅力么！并且，Redis 对 HyperLogLog 的存储结构做了优化，采用两种方式计数：

- **稀疏矩阵** ：计数较少的时候，占用空间很小。
- **稠密矩阵** ：计数达到某个阈值的时候，占用 12k 的空间。

#### 应用场景

**数量量巨大（百万、千万级别以上）的计数场景**

- 举例 ：热门网站每日/每周/每月访问 ip 数统计、热门帖子 uv 统计、
- 相关命令 ：`PFADD`、`PFCOUNT`

### 2.8 Geospatial index

#### 简介

Geospatial index（地理空间索引，简称 GEO） 主要用于存储地理位置信息，基于 Sorted Set 实现。

通过 GEO 我们可以轻松实现两个位置距离的计算、获取指定位置附近的元素等功能。

![image-20220809101829997](https://img.zxdmy.com/2022/202208091018211.png)

#### 应用场景

**需要管理使用地理空间数据的场景**

- 举例：附近的人。
- 相关命令: `GEOADD`、`GEORADIUS`、`GEORADIUSBYMEMBER`



## 3、Redis 线程模型

### 3.1 单线程（6.0 之前）

**`Redis` 基于 `Reactor` 模式来设计开发了自己的一套高效的事件处理模型**，而这个事件处理模型对应于 Redis 中的 **文件事件处理器**（file event handler），由于**文件事件处理器** 是单线程运行的，所以认为 **Redis 是单线程模型**。

**文件事件处理器**（file event handler）主要是包含 4 个部分：

- 多个 socket（客户端连接）
- IO 多路复用程序（支持多个客户端连接的关键）
- 文件事件分派器（将 socket 关联到相应的事件处理器）
- 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）

![image-20220809102823524](https://img.zxdmy.com/2022/202208091028657.png)

Redis 通过**IO 多路复用程序** 来监听来自客户端的大量连接（或者说是监听多个 socket）。

> **I/O 多路复用技术** 的使用让 Redis 不需要额外创建多余的线程来监听客户端的大量连接，降低了资源的消耗。

### 3.2 多线程（6.0之后）

**Redis 在 4.0 之后的版本中就已经加入了对多线程的支持**，主要是针对一些大键值对的 **删除** 操作的命令，使用其他线程进行异步处理。

大体上来说，**Redis 6.0 之前主要还是单线程处理。**

> 为什么不使用多线程？
>
> 1. 单线程编程容易并且更容易维护；
> 2. Redis 的性能瓶颈不在 CPU ，主要在内存和网络；
> 3. 多线程就会存在死锁、线程上下文切换等问题，甚至会影响性能。

**Redis6.0 引入多线程主要是为了提高网络 IO 读写性能**，因为这个算是 Redis 中的一个性能瓶颈（Redis 的瓶颈主要受限于内存和网络）。

Redis6.0 的多线程默认是禁用的，只使用主线程。如需开启需要修改 redis 配置文件 `redis.conf` ：

```bash
io-threads-do-reads yes
```

开启多线程后，还需要设置线程数，否则是不生效的。同样需要修改 redis 配置文件 `redis.conf` :

```bash
io-threads 4 #官网建议4核的机器建议设置为2或3个线程，8核的建议设置为6个线程
```

> Redis6.0 执行命令仍然是单线程顺序执行。因此，你也不需要担心线程安全问题。

## 4、Redis 内存管理

### 4.1 过期判断

`Redis` 通过一个 **过期字典**（可以看作是 hash 表）来保存数据过期的时间。

过期字典的键指向 Redis 数据库中的某个 key(键)，过期字典的值是一个 long long 类型的整数，这个整数保存了 key 所指向的数据库键的过期时间（毫秒精度的 UNIX 时间戳）。

![image-20220809104357687](https://img.zxdmy.com/2022/202208091043071.png)

### 4.2 过期删除策略与内存淘汰机制

#### 过期删除策略

常用的过期数据的删除策略有两个：

1. **惰性删除** ：只会在取出 key 的时候才对数据进行过期检查。这样对 CPU 最友好，但是可能会造成太多过期 key 没有被删除。
2. **定期删除** ： 每隔一段时间抽取一批 key 执行删除过期 key 操作。并且，Redis 底层会通过限制删除操作执行的时长和频率来减少删除操作对 CPU 时间的影响。

定期删除对内存更加友好，惰性删除对 CPU 更加友好。两者各有千秋，所以 Redis 采用的是 **定期删除+惰性/懒汉式删除** 。

#### 内存淘汰机制

采用 **定期删除和惰性删除**，仍有可能漏掉很多过期 key 的情况。这样就导致大量过期 key 堆积在内存里，然后就 Out of memory 了。

可以使用 **Redis 内存淘汰机制** 解决此问题。

Redis 提供 6 种数据淘汰策略：

1. **volatile-lru（least recently used）**：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰
2. **volatile-ttl**：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
3. **volatile-random**：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰
4. **allkeys-lru（least recently used）**：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的 key（这个是最常用的）
5. **allkeys-random**：从数据集（server.db[i].dict）中任意选择数据淘汰
6. **no-eviction**：禁止驱逐数据，也就是说当内存不足以容纳新写入数据时，新写入操作会报错。这个应该没人使用吧！

4.0 版本后增加以下两种：

1. **volatile-lfu（least frequently used）**：从已设置过期时间的数据集（server.db[i].expires）中挑选最不经常使用的数据淘汰
2. **allkeys-lfu（least frequently used）**：当内存不足以容纳新写入数据时，在键空间中，移除最不经常使用的 key

## 5、 Redis 持久化机制

所谓**持久化数据**，是将内存中的数据写入到硬盘里面，大部分原因是为了之后重用数据（比如重启机器、机器故障之后恢复数据），或者是为了防止系统故障而将数据备份到一个远程位置。

Redis 两种不同的持久化操作：

+ 快照（snapshotting，RDB）
+ 只追加文件（append-only file, AOF）

### 5.1 快照持久化（RDB 持久化）

**Redis 可以通过创建快照来获得存储在内存里面的数据在某个时间点上的副本。**

Redis 创建快照之后，可以对快照进行备份，可以将快照复制到其他服务器从而创建具有相同数据的服务器副本（Redis 主从结构，主要用来提高 Redis 性能），还可以将快照留在原地以便重启服务器的时候使用。

快照持久化是 Redis 默认采用的持久化方式，在 `redis.conf` 配置文件中默认有此下配置：

```shell
save 900 1           #在900秒(15分钟)之后，如果至少有1个key发生变化，Redis就会自动触发BGSAVE命令创建快照。

save 300 10          #在300秒(5分钟)之后，如果至少有10个key发生变化，Redis就会自动触发BGSAVE命令创建快照。

save 60 10000        #在60秒(1分钟)之后，如果至少有10000个key发生变化，Redis就会自动触发BGSAVE命令创建快照。
```

### 5.2 只追加文件持久化（AOF 持久化）

与快照持久化相比，AOF 持久化的实时性更好，因此已成为主流的持久化方案。

默认情况下 Redis 没有开启 AOF（append only file）方式的持久化，可以通过 appendonly 参数开启。

开启 **AOF 持久化**后，每执行一条会更改 Redis 中的数据的命令，Redis 就会将该命令写入到内存缓存 `server.aof_buf` 中，然后再根据 `appendfsync` 配置来决定何时将其同步到硬盘中的 AOF 文件。

在 Redis 的配置文件中存在三种不同的 AOF 持久化方式，它们分别是：

```shell
appendfsync always    # 每次有数据修改发生时都会写入AOF文件,这样会严重降低Redis的速度
appendfsync everysec  # 每秒钟同步一次，显式地将多个写命令同步到硬盘
appendfsync no        # 让操作系统决定何时进行同步
```

为了兼顾数据和写入性能，用户可以考虑 `appendfsync everysec` 选项 ，让 Redis 每秒同步一次 AOF 文件，Redis 性能几乎没受到任何影响。而且这样即使出现系统崩溃，用户最多只会丢失一秒之内产生的数据。当硬盘忙于执行写入操作的时候，Redis 还会优雅的放慢自己的速度以便适应硬盘的最大写入速度

> AOF 文件的保存位置和 RDB 文件的位置相同，都是通过 dir 参数设置的，默认的文件名是 `appendonly.aof`。

### 5.3 AOF 重写

AOF 重写可以产生一个新的 AOF 文件，这个新的 AOF 文件和原有的 AOF 文件所保存的数据库状态一样，但体积更小。

> 该功能是通过读取数据库中的键值对来实现的，程序**无须**对现有 AOF 文件进行任何读入、分析或者写入操作。

### 5.4 持久化机制的优化

Redis 4.0 开始支持 RDB 和 AOF 的混合持久化（默认关闭，可以通过配置项 `aof-use-rdb-preamble` 开启）。

如果把混合持久化打开，AOF 重写的时候就直接把 RDB 的内容写到 AOF 文件开头。这样做的好处是可以结合 RDB 和 AOF 的优点, 快速加载同时避免丢失过多的数据。当然缺点也是有的， AOF 里面的 RDB 部分是压缩格式不再是 AOF 格式，可读性较差。

## 6、Redis 事务

### 6.1 基本使用

**Redis 事务提供了一种将多个命令请求打包的功能。然后，再按顺序执行打包的所有命令，并且不会被中途打断。**

Redis 可以通过 **`MULTI`，`EXEC`，`DISCARD` 和 `WATCH`** 等命令来实现事务(transaction)功能。

使用 `MULTI` 命令后可以输入多个命令。Redis 不会立即执行这些命令，而是将它们放到队列，当调用了 `EXEC` 命令将执行所有命令。

这个过程是这样的：

1. 开始事务（`MULTI`）。
2. 命令入队(批量操作 Redis 的命令，先进先出（FIFO）的顺序执行)。
3. 执行事务(`EXEC`)。

通过 `DISCARD` 命令取消一个事务，它会清空事务队列中保存的所有命令。

```
> MULTI
OK
> SET USER "Guide哥"
QUEUED
> GET USER
QUEUED
> EXEC
1) OK
2) "Guide哥"
```

### 6.2 不支持回滚

Redis 的事务和我们平时理解的关系型数据库的事务不同。

Redis 事务在运行错误的情况下，除了执行过程中出现错误的命令外，其他命令都能正常执行。并且，Redis 是不支持回滚（roll back）操作的。因此，Redis 事务其实是不满足原子性的（而且不满足持久性）。

> 除了不满足原子性之外，事务中的每条命令都会与 Redis 服务器进行网络交互，这是比较浪费资源的行为。
>
> Redis 事务是不建议在日常开发中使用的。

### 6.3  Lua 脚本：解决事务缺陷

Redis 从 2.6 版本开始支持执行 Lua 脚本，它的功能和事务非常类似。我们可以利用 Lua 脚本来批量执行多条 Redis 命令，这些 Redis 命令会被提交到 Redis 服务器一次性执行完成，大幅减小了网络开销。

一段 Lua 脚本可以视作一条命令执行，一段 Lua 脚本执行过程中不会有其他脚本或 Redis 命令同时执行，保证了操作不会被其他指令插入或打扰。

如果 Lua 脚本运行时出错并中途结束，出错之后的命令是不会被执行的。并且，出错之前执行的命令是无法被撤销的。因此，严格来说，通过 Lua 脚本来批量执行 Redis 命令也是**不满足原子性**的。

## 7、Redis 优化

### bigkey 问题

> 如果一个 key 对应的 value 所占用的内存比较大，那这个 key 就可以看作是 bigkey。

bigkey 除了会消耗更多的内存空间，对性能也会有比较大的影响。

因此，我们应该尽量避免写入 bigkey！

可以**使用 Redis 自带的 `--bigkeys` 参数来查找bigkey**。

### 大量 key 集中过期问题

两种常见的方法：

1. 给 key 设置随机过期时间。
2. 开启 lazy-free（惰性删除/延迟释放） 。lazy-free 特性是 Redis 4.0 开始引入的，指的是让 Redis 采用异步方式延迟释放 key 使用的内存，将该操作交给单独的子线程处理，避免阻塞主线程。

## 8、缓存穿透与缓存雪崩

### 8.1 缓存穿透

#### 简介

大量请求的 key 不存在于缓存中，导致请求直接到了数据库上，没有经过缓存这一层。

#### 解决方案

**1、参数校验**

对一些不合法的参数请求，直接抛出异常信息返回给客户端。

比如查询的数据库 id 不能小于 0、传入的邮箱格式不对。

**2、缓存无效 key**

如果缓存和数据库都查不到某个 key 的数据，就写一个到 Redis 中，并设置过期时间。

> 这种方式可以解决请求的 key 变化不频繁的情况。
>
> 如果黑客恶意攻击，每次构建不同的请求 key，会导致 Redis 中缓存大量无效的 key 。

**3、布隆过滤器**

**布隆过滤器** 是一个数据结构，通过它可以非常方便地判断一个给定数据是否存在于海量数据中。

具体操作：把所有可能存在的请求的值都存放在布隆过滤器中，当用户请求过来，先判断用户发来的请求的值是否存在于布隆过滤器中。不存在的话，直接返回请求参数错误信息给客户端，存在的话才会走下面的流程。

![image-20220809111136868](https://img.zxdmy.com/2022/202208091111471.png)

但可能会误判：

+ **布隆过滤器说某个元素存在，小概率会误判。**
+ **布隆过滤器说某个元素不在，那么这个元素一定不在。**

通过布隆过滤器的原理，可以得出原因：

+ 将元素加入布隆过滤器：
    1. 使用布隆过滤器中的哈希函数对元素值进行计算，得到哈希值（有几个哈希函数得到几个哈希值）
    2. 根据得到的哈希值，在位数组中把对应下标的值置为 1
+ 判断是否存在于布隆过滤器：
    1. 对给定元素再次进行相同的哈希计算
    2. 得到值之后判断位数组中的每个元素是否都为 1。如果值都为 1，那么说明这个值在布隆过滤器中，如果存在一个值不为 1，说明该元素不在布隆过滤器中。

所以，一定会出现这样一种情况：**不同的字符串可能哈希出来的位置相同**。

> 可以适当增加位数组大小或者调整我们的哈希函数来降低概率。

### 8.2 缓存雪崩

#### 简介

**缓存在同一时间大面积的失效，后面的请求都直接落到了数据库上，造成数据库短时间内承受大量请求。**

**有一些被大量访问数据（热点缓存）在某一时刻大面积失效，导致对应的请求直接落到了数据库上。**

#### 解决方案

**针对 Redis 服务不可用的情况：**

1. 采用 Redis 集群，避免单机出现问题整个缓存服务都没办法使用。
2. 限流，避免同时处理大量的请求。

**针对热点缓存失效的情况：**

1. 设置不同的失效时间比如随机设置缓存的失效时间。
2. 缓存永不失效。





##　redis 分布式锁可重入怎么实现？

## Redis + MySQL 数据如何同步

## redis 里扩容的实现

## Redis 的主从同步