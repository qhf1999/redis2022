### Docker配置redis、nginx
### 更新数据库、造成的数据一致性问题
### 缓存穿透 
* 数据库、redis都不存在，每次请求都会到达数据库 
* 解决方案:
1. 不存在数据value置为null,存入redis中 
2. 布隆过滤
### 缓存雪崩
* redis中大量key过期或者redis服务宕机
* 解决方案
1. 给不同key的TTL设置随机值
2. 搭建Redis集群（主从复制）
3. 给缓存添加降级限流策略
4. 给业务添加多级缓存（不止Redis）
### 缓存击穿
* 高并发访问且缓存重建业务较复杂的key突然失效，无数的请求会访问数据库
* 解决方案

1.互斥锁
> 未命中缓存后，请求新建缓存的过程中加互斥锁。只允许一个请求可以新建缓存。请求想要新建缓存必须获得互斥锁
>
2.逻辑过期
> 在value中设置一个过期时间，而不设置key的TTL,所以这个数据实际上不会过期
> 线程通过对比value中的过期时间,如果发现缓存已经过期，就新开启一个线程去更新缓存（开启锁），自己返回旧的缓存即可。
> 在新线程更新的期间，如果有请求，就返回旧的缓存。
