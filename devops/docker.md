# Docker基础知识

![](/assets/devops/docker/1.png)

![](/assets/devops/docker/2.png)

## docker的一些常用指令

![](/assets/devops/docker/4.png)

## docker常用指令

* 在线安装镜像  
  ![](/assets/devops/docker/5.png)

* 导入导出镜像  
  ![](/assets/devops/docker/6.png)

* 启动命令

```bash
docker run -it -p 9000:8080 -v /Users/zhuojingruan/Desktop/project:/soft --privileged --name myjava java bash

-p 端口映射 宿主机端口号:容器内的端口号
-v 目录映射 宿主机目录:容器内目录
--provoleged 授予权限
--name 名字 容器的名字
```

* 暂停和停止容器
![](/assets/devops/docker/8.png)

* 查看当前的容器
```
docker ps -a
```

* 进入当前容器
```
docker exec -it 容器名 bash
```

* 以root用户登陆当前容器
```
docker exec -ti -u root 容器id bash
```


## docker创建集群准备工作
* 创建网段
```
docker network create --subnet=172.18.0.0/24 net1
```

* 查看网段

```
docker inspect net1
```
* 删除网段

```
docker network rm net1
```

* 创建docker卷

```
docker volume create --name v1
```

* 查看卷
```
docker inspect v1
```

