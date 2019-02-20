- [1. Memcache和Redis的区别](#1-Memcache和Redis的区别)
- [2. 为什么Redis能这么快](#2-为什么Redis能这么快)
- [3. 说说你用过的Redis的数据类型](#3-说说你用过的Redis的数据类型)
- [4. 如何通过Redis实现分布式锁](#4-如何通过Redis实现分布式锁)
- [5. Redis中大量的key同时过期的注意事项](#5-Redis中大量的key同时过期的注意事项)
- [6. 如何使用Redis做异步队列](#6-如何使用Redis做异步队列)
## 1. Memcache和Redis的区别
- Memcache
  - 支持简单数据类型
  - 不支持数据持久化存储
  - 不支持主从
  - 不支持分片
- Redis
  - 数据类型丰富
  - 支持数据磁盘持久化存储
  - 支持主从
  - 支持分片
## 2. 为什么Redis能这么快
- 完全基于内存，绝大部分请求是纯粹的内存操作，执行效率高
- 数据结构简单，对数据操作也简单
- 使用多路I/O复用模型，非阻塞IO
- 采用单线程，单线程也能处理高并发请求，想多核也可以启动多实例
## 3. 说说你用过的Redis的数据类型
- String：最基本的数据类型，二进制安全
- Hash：String元素组成的字典，适合用于存储对象
- List：列表，按照String元素插入顺序排序
- Set：String元素组成的无序集合，通过Hash表实现，不允许重复
- Sorted Set：通过分数来为集合中的成员进行从小到大的排序
- 用于计数的HyperLogLog(非重点，前面五个尽量说出来)
## 4. 如何通过Redis实现分布式锁
SET key value [EX seconds] [PX milliseconds] [NX|XX]
- EX seconds：设置键的过期时间为second秒
- PX milliseconds：设置键的过期时间为millisecond毫秒
- NX：只在键不存在时，才对键进行设置操作
- XX：只在键已经存在时，才对键进行设置操作
- SET操作成功完成时，返回OK，否则返回nil
## 5. Redis中大量的key同时过期的注意事项
- 现象：
  - 集中过期，由于清楚大量的key很耗时，会出现短暂的卡顿现象
- 解决方案
  - 在设置key的过期时间的时候，给每个key加上随机值
## 6. 如何使用Redis做异步队列
使用List作为队列，RPUSH生产消息，LPOP消费消息
- 缺点：没有等待队列里面有值就直接消费
- 解决方案：
  - 可以通过在应用层引入Sleep机制去调用LPOP重试
  - BLPOP key [key ...] timeout：阻塞直到队列有消息或者超时，缺点是只能供一个消费者消费
  - pub/sub：主题订阅者模式，缺点：消息的发布是无状态的，无法保证可达
