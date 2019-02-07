# 通过一个理财系统的实战，来了解整个spring boot

## 整个系统的脉络

![](/assets/屏幕快照 2019-02-07 下午1.52.29.png)

## 系统的业务流程

![](/assets/屏幕快照 2019-02-07 下午1.55.08.png)

## 模块设计

![](/assets/屏幕快照 2019-02-07 下午2.00.07.png)

## 1. 项目准备
### 1.1 环境准备
#### 下载Gradle

下载地址：[点击下载gradle](https://link.jianshu.com?t=http%3A%2F%2Fservices.gradle.org%2Fdistributions%2F)

下载最新版本:gradle-4.6 \(当前最新版2018年3月9日\),记得要下载all版本的,里面包含了Gradle SDK 所有相关的内容,包括源代码、文档、示例等.

#### 配置Gradle环境

将下载的gradle解压,我的Gradle存放路径：/Users/Apple/Desktop/soft/gradle  
 要运行Gradle,必须把GRADLE\_HOME/bin 目录添加到环境变量PATH的路径才可以,在终端中输入：

``` bash
vi ~./bash_profile
```

打开.bash\_profile文件,在.bash\_profile文件中输入下面内容

``` bash
export GRADLE_HOME=/Users/Apple/Desktop/soft/gradle/gradle-4.6 
export PATH=$PATH:$GRADLE_HOME/bin

```

注: 将GRADLE\_HOME的值替换成你的Gradle路径

然后保存.bash\_profile文件,在终端上执行

``` bash
source ~/.bash_profile
```

更新.bash\_profile文件

#### 验证

 在终端上执行gradle -version，查看是否配置成功。如显示如下表示配置成功.
 
### 1.2 产品设计

* 产品表

|序号|英文|中文|类型|长度|备注|
|---|---|---|---|---|---|
|1|id|编号|varchar|50||
|2|name|名称|varchar|50||
|3|threshold_amount|起投金额|decimal|15,3||
|4|step_amount|投资步长|decimal|15,3||
|5|lock_term|锁定期|smallint||单位天|
|6|reward_rate|收益率|decimal|5,3|0-100|
|7|status|状态|varchar|20|销售中，锁定，已结束|
|8|memo|备注|varchar|200||
|9|create_at|创建时间|datetime|||
|10|create_user|创建者ID|varchar|20||
|11|update_at|更新时间|datetime|||
|12|update_user|更新者ID|varchar|20||

* 订单表

|序号|英文|中文|类型|长度|备注|
|---|---|---|---|---|---|
|1|order_id|订单编号|varchar|50||
|2|chan_id|渠道编号|varchar|50|淘客公司编号|
|3|product_id|产品编号|varchar|50||
|4|chan\_user\_id|用户编号|varchar|50|淘客公司用户编号|
|5|order_type|订单类型|varchar|50|申购:apply,赎回:redeem|
|6|order_status|订单状态|varchar|50|初始化:INIT,处理中:PROCESS,成功:SUCESS,失败:FAIL|
|7|outer\_order\_id|外部订单编号|varchar|50|淘客公司订单编号|
|8|amount|订单金额|decimal|15,3||
|9|memo|备注|varchar|200||
|10|create_at|创建者时间|datetime|||
|11|update_at|更新时间|datetime|||




