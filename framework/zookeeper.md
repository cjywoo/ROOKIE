# zookeeper基本介绍

## zk.cfg文件

```
* tickTime: zookeeper中使用的基本时间单位, 毫秒值.
* dataDir: 数据目录. 可以是任意目录.
* dataLogDir: log目录, 同样可以是任意目录. 如果没有设置该参数, 将使用和#dataDir相同的设置.
* clientPort: 监听client连接的端口号.
```