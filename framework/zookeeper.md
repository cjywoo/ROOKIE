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
```
