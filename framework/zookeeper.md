# zookeeper基本介绍

## zk.cfg文件

```
* tickTime: zookeeper中使用的基本时间单位, 毫秒值.
* dataDir: 数据目录. 可以是任意目录.
* dataLogDir: log目录, 同样可以是任意目录. 如果没有设置该参数, 将使用和#dataDir相同的设置.
* clientPort: 监听client连接的端口号,默认2181.
* initLimit: 允许从节点链接并同步到Master主节点的初始化链接时间，以tickTime的整数倍
* syncLimit：master主节点与从节点之间发送消息和应答时间，以tickTime的整数倍
```

## 启动与停止
``` bash
./bin/zkServer.sh start
./bin/zkServer.sh status
./bin/zkServer.sh stop

./bin/zkCli.sh 客户端连接
```

## zookeeper基本数据结构
是一个树形结构，类似于前端的tree.js
每个节点称为znode,也可以有子节点，也可以有数据
可以有临时节点和永久节点
每个节点有自己的版本号，可以通过命令行显示
每当节点数据发生变化，该节点的版本会自动累加
每个zk节点存储的数据不易过大
节点可以设置acl，可以通过权限来限制用户的访问

## zookeeper的作用
* master的选举，首脑模式，保证集群高可用
* 统一配置文件管理，只需要部署一台服务器，就可以把相同的配置文件同步到其他服务器。
* 发布与订阅，类似于消息队列
* 提供分布式锁，分布式环境中不同进程之间争夺资源，类似于多线程中的锁。
* 集群管理，集群中保持数据的一致性。

## zookeeper的基本命令
```
./zkCli.sh 打开zk的客户端命令行后台
get 与set命令
create 与 delete命令

```

## zk session的基本原理
客户端与服务端之间的连接存在会话
每个会话都可以设置一个超时时间
心跳结束，session过期
心跳机制：客户端向服务端的ping包请求

## zk watcher机制
针对每个节点的操作，都有一个watcher
当监控的某个对象发生了变化，则触发watcher事件
zk的watcher是一次性的，触发之后就销毁了
父节点和子节点都能触发watcher事件

给父节点设置watcher事件
```
stat path watch
get path watch
set path watch
```

给子节点设置watcher事件
```
ls path watch
```

### watcher事件的类型
* NodeCreated
* NodeDataChanged
* NodeDeleted
* NodeChildrenCreated

### watcher的使用场景

