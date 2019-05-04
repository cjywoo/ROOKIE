# 实战一 搭建mysql+haproxy集群方案



### 1. 搭建基于pxc的mysql集群

1. 拉镜像 
   `docker pull percona/percona-xtradb-cluster:5.7`
2. 镜像名字有点长，起个短点的 
   `docker tag percona/percona-xtradb-cluster:5.7 pxc`
3. 出于安全考虑，针对PXC集群实例创建内部网络 创建的时候通过参数指定IP段和子网掩码，Docker默认使用的IP 172.17.0.1 
   `docker network create --subnet=172.18.0.0./24 pxc-network`
4. 创建第一个节点 
   `docker run -d -p 33010:3306 -e MYSQL_ROOT_PASSWORD=root -e CLUSTER_NAME=pxc_cluster --name=pxc_node1 --net=pxc-network pxc`
    执行 docker logs pxc\_node1 查看执行状态，如果看到 mysqld: ready for connections. 可以使用navicat等工具测试连接。
5. 创建第二个数据库节点，并加入到第一个集群中，注意多了 CLUSTER\_JOIN 参数 
   `docker run -d -p 33011:3306 -e MYSQL_ROOT_PASSWORD=root -e CLUSTER_NAME=pxc_cluster -e CLUSTER_JOIN=pxc_node1 --name=pxc_node2 --net=pxc-network pxc`
6. 创建第三个数据库节点，并加入到第一个集群中，注意多了 CLUSTER\_JOIN 参数 
   `docker run -d -p 33012:3306 -e MYSQL_ROOT_PASSWORD=root -e CLUSTER_NAME=pxc_cluster -e CLUSTER_JOIN=pxc_node1 --name=pxc_node3 --net=pxc-network pxc`
7. 接下来可以创建更多的节点，只需修改 容器名称 --name 和映射的端口；
8. 测试：本地连接这三个节点，在其中一个创建demo数据，其他节点都自动同步数据过去了

### 注意

1. 启动第一个节点后记得使用docker logs查看启动状态，然后使用navicat等工具测试连接，等第一个mysql运行成功后再运行第二个容器。否则第二个起不来，需要重新启动容器。
2. 停止一个容器后，如果要在启动它，需要在他的启动配置里面加上他加入的集群在启动，否则再启动时连接不上了。需要在下面花更多的时候看文档，逛论坛。更多技术细节参见

[官方文档](https://www.percona.com/doc/percona-xtradb-cluster/5.7/index.html)



# 2. 搭建haproxy+keepalived

下载镜像 

1. `docker pull haproxy`
2. [宿主机](https://cloud.tencent.com/product/cdh)
   创建 haproxy 的配置文件，比如路径是 D:\Docker\haproxy\haproxy.cfg
3. 最重要的就是配置文件了。这里内容如下：

```
global
        daemon
        # nbproc 1
        # pidfile /var/run/haproxy.pid
        # 工作目录
        chroot /usr/local/etc/haproxy

defaults
        log 127.0.0.1 local0 err #[err warning info debug]
        mode http                #默认的模式mode { tcp|http|health }，tcp是4层，http是7层，health只会返回OK
        retries 2                #两次连接失败就认为是服务器不可用，也可以通过后面设置
        option redispatch        #当serverId对应的服务器挂掉后，强制定向到其他健康的服务器
        option abortonclose      #当服务器负载很高的时候，自动结束掉当前队列处理比较久的链接
        option dontlognull       #日志中不记录负载均衡的心跳检测记录
        maxconn 4096             #默认的最大连接数
        timeout connect 5000ms   #连接超时
        timeout client 30000ms   #客户端超时
        timeout server 30000ms   #服务器超时
        #timeout check 2000      #=心跳检测超时

######## 监控界面配置 #################
listen admin_status
        # 监控界面访问信息
        bind 0.0.0.0:8888
        mode http
        # URI相对地址
        stats uri /dbs
        # 统计报告格式
        stats realm Global\ statistics
        # 登录账户信息
        stats auth admin:123456
########frontend配置##############

######## mysql负载均衡配置 ###############
listen proxy-mysql
        bind 0.0.0.0:3306
        mode tcp
        # 负载均衡算法
        # static-rr 权重, leastconn 最少连接, source 请求IP, 轮询 roundrobin
        balance roundrobin
        # 日志格式
        option tcplog
        # 在 mysql 创建一个没有权限的haproxy用户，密码为空。 haproxy用户
        # create user 'haproxy'@'%' identified by ''; FLUSH PRIVILEGES;
        option mysql-check user haproxy
         # 这里是容器中的IP地址，由于配置的是轮询roundrobin，weight 权重其实没有生效
        server MYSQL_1 172.18.0.2:3306 check weight 1 maxconn 2000
        server MYSQL_2 172.18.0.3:3306 check weight 1 maxconn 2000
        server MYSQL_3 172.18.0.4:3306 check weight 1 maxconn 2000
        # 使用keepalive检测死链
        # option tcpka
#########################################
```

1. 启动 haproxy 的容器，镜像名称为 h1，网络名称使用上节中创建的 pxc-network，就是和 mysql 集群处于同一网络。 
   `docker run -it -d -p 4001:8888 -p 4002:3306 -v D:/Docker/haproxy:/usr/local/etc/haproxy --name h1 --net=pxc-network`
2. 进去容器，并让 haproxy 加载配置 
   `docker exec -it h1 bash`
   `haproxy -f /usr/local/etc/haproxy/`
3. 宿主机打开 
   `http://localhost:4001/dbs`
    这是haproxy 提供的图形界面

>

![](https://ask.qcloudimg.com/http-save/yehe-1412306/jy5afhkfm9.png?imageView2/2/w/1620)

 image.png

可以看到每个mysql节点运行状态是绿色，说明正常。

1. 测试，停掉一个数据库节点 
   `docker stop pxc_node1`
    ，发现有一个变红了。

>

![](https://ask.qcloudimg.com/http-save/yehe-1412306/x0r5ag6b24.png?imageView2/2/w/1620)


1. 项目中可以使用配置的 4002 来连接数据库，这样请求会被分发到各个子节点。

  






