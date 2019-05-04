# mysql

## mysql集群方案

![](/assets/devops/mysql/1.png)

## mysql集群要使用负载均衡的必要性

![](/assets/docker/mysql/2.png)

## pxc集群启动之后，因为有强一致性，所以一旦停机，必须重新删除容器，并重新启动才行

直接通过docker start node1 或者任何一个节点是启动不了的，原因是集群之前的同步机制造成的，启动任何一个节点，该节点都会去其它节点同步数据，其它节点仍处于宕机状态，所以该节点启动失败，这也是pxc集群的强一致性的表现，解决方式是，删除所有节点docker rm node1 node2 node3 node4 node 5

和数据卷中的grastate.dat文件

```
rm -rf /var/lib/docker/volumes/v1/_data/grastate.dat
rm -rf /var/lib/docker/volumes/v2/_data/grastate.dat

rm -rf /var/lib/docker/volumes/v3/_data/grastate.dat

rm -rf /var/lib/docker/volumes/v4/_data/grastate.dat

rm -rf /var/lib/docker/volumes/v5/_data/grastate.dat
```

## 数据库备份

### 冷备份

![](/assets/mysql/21.png)

### 热备份

![](/assets/mysql/22.png)

备份方案就是在宿主机创建一个数据卷，然后通过映射到容器内，实现备份。

## 数据库还原
采用冷还原，即采用空白的Mysql还原数据，然后再加入到集群中
> 注意:还原之后，一定要重启容器！！！

