# redis

## 1.redis集群方案

* RedisCluster 官方推荐 没有中心节点
* Codis 中间件产品，存在中心节点
* TWemproxy 中间件产品，存在中心节点

## 2.redisCluster的特点

* 无中心节点，客户端与redis直接连接，不需要代理
* 数据需要被分片存储\(需要设置冗余节点\)
* 管理方便，后期可以自行新增或者删除节点

## 3. redis集群的示意图
![](/assets/redis/1.png)



