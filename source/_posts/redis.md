---
title: Redis 技术指南
date: 2025-12-10
tags:
  - Redis
  - 数据库
  - NoSQL
  - 缓存
  - Post
categories:
  - 技术文档
---

Redis 是一个开源的、基于内存的数据结构存储系统，可以用作数据库、缓存和消息中间件。本文将介绍 Redis 的核心概念和常用命令。

<!-- more -->

## 安装与启动

### macOS 安装

```bash
# 使用 Homebrew 安装
brew install redis

# 启动 Redis 服务
brew services start redis

# 或者前台启动
redis-server
```

### Linux 安装

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install redis-server

# CentOS/RHEL
sudo yum install redis

# 启动服务
sudo systemctl start redis
sudo systemctl enable redis
```

### 连接 Redis

```bash
# 连接本地 Redis
redis-cli

# 连接远程 Redis
redis-cli -h 192.168.1.100 -p 6379 -a password
```

## 数据类型

Redis 支持多种数据类型，每种类型都有其特定的使用场景。

### 1. String（字符串）

最基本的数据类型，可以存储字符串、整数或浮点数。

```bash
# 设置键值
SET name "Redis"
# 输出: OK

# 获取值
GET name
# 输出: "Redis"

# 设置带过期时间的键（10秒后过期）
SET session "abc123" EX 10

# 或使用 SETEX
SETEX token 3600 "mytoken"

# 仅在键不存在时设置
SETNX lock "1"
# 输出: (integer) 1

# 批量设置
MSET key1 "value1" key2 "value2" key3 "value3"

# 批量获取
MGET key1 key2 key3
# 输出:
# 1) "value1"
# 2) "value2"
# 3) "value3"

# 数值递增
SET counter 100
INCR counter
# 输出: (integer) 101

INCRBY counter 10
# 输出: (integer) 111

# 数值递减
DECR counter
# 输出: (integer) 110

# 追加字符串
APPEND name " Database"
GET name
# 输出: "Redis Database"

# 获取字符串长度
STRLEN name
# 输出: (integer) 14
```

### 2. Hash（哈希）

适合存储对象，类似于字典或 Map。

```bash
# 设置哈希字段
HSET user:1001 name "张三"
HSET user:1001 age 25
HSET user:1001 email "zhangsan@example.com"

# 批量设置
HMSET user:1002 name "李四" age 30 city "北京"

# 获取单个字段
HGET user:1001 name
# 输出: "张三"

# 获取多个字段
HMGET user:1001 name age email
# 输出:
# 1) "张三"
# 2) "25"
# 3) "zhangsan@example.com"

# 获取所有字段和值
HGETALL user:1001
# 输出:
# 1) "name"
# 2) "张三"
# 3) "age"
# 4) "25"
# 5) "email"
# 6) "zhangsan@example.com"

# 获取所有字段名
HKEYS user:1001

# 获取所有值
HVALS user:1001

# 检查字段是否存在
HEXISTS user:1001 name
# 输出: (integer) 1

# 删除字段
HDEL user:1001 email

# 字段数值递增
HINCRBY user:1001 age 1
# 输出: (integer) 26

# 获取字段数量
HLEN user:1001
# 输出: (integer) 2
```

### 3. List（列表）

有序的字符串列表，支持从两端插入和弹出元素。

```bash
# 从左侧插入
LPUSH tasks "任务1" "任务2" "任务3"
# 输出: (integer) 3

# 从右侧插入
RPUSH tasks "任务4"
# 输出: (integer) 4

# 获取列表范围（0到-1表示全部）
LRANGE tasks 0 -1
# 输出:
# 1) "任务3"
# 2) "任务2"
# 3) "任务1"
# 4) "任务4"

# 获取列表长度
LLEN tasks
# 输出: (integer) 4

# 从左侧弹出
LPOP tasks
# 输出: "任务3"

# 从右侧弹出
RPOP tasks
# 输出: "任务4"

# 阻塞式弹出（适合消息队列，超时5秒）
BLPOP tasks 5

# 获取指定索引的元素
LINDEX tasks 0
# 输出: "任务2"

# 设置指定索引的值
LSET tasks 0 "更新的任务"

# 在指定元素前/后插入
LINSERT tasks BEFORE "任务1" "新任务"

# 保留指定范围的元素
LTRIM tasks 0 2
```

### 4. Set（集合）

无序的字符串集合，元素唯一。

```bash
# 添加元素
SADD fruits "apple" "banana" "orange"
# 输出: (integer) 3

# 获取所有成员
SMEMBERS fruits
# 输出:
# 1) "apple"
# 2) "banana"
# 3) "orange"

# 检查元素是否存在
SISMEMBER fruits "apple"
# 输出: (integer) 1

# 获取集合大小
SCARD fruits
# 输出: (integer) 3

# 移除元素
SREM fruits "banana"
# 输出: (integer) 1

# 随机获取元素
SRANDMEMBER fruits

# 随机弹出元素
SPOP fruits

# 集合运算
SADD set1 "a" "b" "c"
SADD set2 "b" "c" "d"

# 交集
SINTER set1 set2
# 输出: "b" "c"

# 并集
SUNION set1 set2
# 输出: "a" "b" "c" "d"

# 差集（set1 中有但 set2 中没有的）
SDIFF set1 set2
# 输出: "a"

# 将交集结果存储到新集合
SINTERSTORE result set1 set2
```

### 5. Sorted Set（有序集合）

带分数的有序集合，元素按分数排序。

```bash
# 添加元素（分数 成员）
ZADD leaderboard 100 "player1"
ZADD leaderboard 85 "player2"
ZADD leaderboard 120 "player3"
ZADD leaderboard 95 "player4"

# 批量添加
ZADD leaderboard 88 "player5" 110 "player6"

# 获取排名（从低到高，索引从0开始）
ZRANK leaderboard "player3"
# 输出: (integer) 5

# 获取排名（从高到低）
ZREVRANK leaderboard "player3"
# 输出: (integer) 0

# 获取分数
ZSCORE leaderboard "player1"
# 输出: "100"

# 按分数范围获取（从低到高）
ZRANGEBYSCORE leaderboard 90 110
# 输出: "player4" "player1" "player6"

# 按排名范围获取（从高到低，带分数）
ZREVRANGE leaderboard 0 2 WITHSCORES
# 输出:
# 1) "player3"
# 2) "120"
# 3) "player6"
# 4) "110"
# 5) "player1"
# 6) "100"

# 增加分数
ZINCRBY leaderboard 10 "player2"
# 输出: "95"

# 获取集合大小
ZCARD leaderboard
# 输出: (integer) 6

# 获取指定分数范围内的元素数量
ZCOUNT leaderboard 90 110
# 输出: (integer) 3

# 移除元素
ZREM leaderboard "player5"

# 按排名范围移除
ZREMRANGEBYRANK leaderboard 0 1
```

## 键操作

```bash
# 查看所有键（生产环境慎用）
KEYS *

# 使用模式匹配
KEYS user:*
KEYS *name*

# 检查键是否存在
EXISTS name
# 输出: (integer) 1

# 删除键
DEL name
# 输出: (integer) 1

# 批量删除
DEL key1 key2 key3

# 设置过期时间（秒）
EXPIRE session 3600

# 设置过期时间（毫秒）
PEXPIRE session 3600000

# 设置在指定时间戳过期
EXPIREAT key 1735689600

# 查看剩余生存时间（秒）
TTL session
# 输出: (integer) 3598

# 查看剩余生存时间（毫秒）
PTTL session

# 移除过期时间（持久化）
PERSIST session

# 重命名键
RENAME oldkey newkey

# 仅在新键不存在时重命名
RENAMENX oldkey newkey

# 获取键的数据类型
TYPE name
# 输出: string

# 使用 SCAN 迭代键（推荐用于生产环境）
SCAN 0 MATCH user:* COUNT 100
```

## 事务

Redis 事务可以一次执行多个命令，保证原子性。

```bash
# 开始事务
MULTI

# 添加命令到事务队列
SET account:A 1000
SET account:B 500
DECRBY account:A 100
INCRBY account:B 100

# 执行事务
EXEC

# 取消事务
DISCARD

# 监视键（乐观锁）
WATCH account:A
MULTI
DECRBY account:A 100
EXEC
# 如果在 WATCH 和 EXEC 之间 account:A 被修改，事务将失败
```

## 发布订阅

```bash
# 终端1：订阅频道
SUBSCRIBE news sports

# 终端2：发布消息
PUBLISH news "Breaking news!"
# 输出: (integer) 1（订阅者数量）

# 模式订阅
PSUBSCRIBE news:*

# 发布到匹配的频道
PUBLISH news:tech "Tech news update"

# 取消订阅
UNSUBSCRIBE news
PUNSUBSCRIBE news:*
```

## 持久化

### RDB 快照

```bash
# 手动触发快照
SAVE        # 同步保存（阻塞）
BGSAVE      # 后台异步保存

# 查看最后一次保存时间
LASTSAVE
```

### AOF 日志

```bash
# 手动触发 AOF 重写
BGREWRITEAOF
```

配置文件 `redis.conf` 中的相关设置：

```conf
# RDB 配置
save 900 1      # 900秒内有1次修改则保存
save 300 10     # 300秒内有10次修改则保存
save 60 10000   # 60秒内有10000次修改则保存

# AOF 配置
appendonly yes
appendfsync everysec  # 每秒同步一次
```

## 常用管理命令

```bash
# 查看服务器信息
INFO
INFO memory
INFO replication

# 查看客户端连接
CLIENT LIST

# 关闭客户端连接
CLIENT KILL ip:port

# 查看慢查询日志
SLOWLOG GET 10

# 清空当前数据库
FLUSHDB

# 清空所有数据库（危险！）
FLUSHALL

# 查看数据库大小
DBSIZE

# 切换数据库（默认16个，索引0-15）
SELECT 1

# 查看配置
CONFIG GET maxmemory
CONFIG GET *

# 动态修改配置
CONFIG SET maxmemory 2gb

# 查看内存使用情况
MEMORY USAGE key
MEMORY STATS
```

## 性能优化建议

### 1. 键命名规范

```bash
# 推荐使用冒号分隔的层级结构
user:1001:profile
order:2024:12:10:001
cache:article:12345
```

### 2. 管道（Pipeline）

使用管道可以减少网络往返次数，提高性能：

```bash
# 使用 redis-cli 管道
cat commands.txt | redis-cli --pipe

# commands.txt 内容示例：
# SET key1 value1
# SET key2 value2
# SET key3 value3
```

### 3. 内存优化

```bash
# 查看键的内存使用
MEMORY USAGE user:1001

# 查看大键
redis-cli --bigkeys

# 分析内存
redis-cli --memkeys
```

## 常见使用场景

### 缓存

```bash
# 设置缓存（带过期时间）
SET cache:user:1001 '{"name":"张三","age":25}' EX 3600

# 获取缓存
GET cache:user:1001
```

### 分布式锁

```bash
# 获取锁（NX: 不存在才设置，EX: 过期时间）
SET lock:order:1001 "uuid-xxx" NX EX 30

# 释放锁（需要用 Lua 脚本保证原子性）
EVAL "if redis.call('get',KEYS[1]) == ARGV[1] then return redis.call('del',KEYS[1]) else return 0 end" 1 lock:order:1001 "uuid-xxx"
```

### 限流

```bash
# 简单的滑动窗口限流
# 每分钟最多 100 次请求
MULTI
INCR ratelimit:user:1001
EXPIRE ratelimit:user:1001 60
EXEC

# 检查是否超限
GET ratelimit:user:1001
```

### 排行榜

```bash
# 更新分数
ZINCRBY leaderboard 10 "user:1001"

# 获取前10名
ZREVRANGE leaderboard 0 9 WITHSCORES

# 获取用户排名
ZREVRANK leaderboard "user:1001"
```

## 总结

Redis 是一个功能强大且灵活的内存数据库，通过合理选择数据结构和使用正确的命令，可以高效地解决各种业务场景的问题。建议在实际使用中：

1. 根据业务场景选择合适的数据类型
2. 合理设置键的过期时间
3. 注意内存使用，避免大键
4. 生产环境使用 SCAN 代替 KEYS
5. 合理配置持久化策略

更多详细信息请参考 [Redis 官方文档](https://redis.io/documentation)。

