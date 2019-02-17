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

```bash
vi ~/.bash_profile
```

打开.bash\_profile文件,在.bash\_profile文件中输入下面内容

```bash
export GRADLE_HOME=/Users/Apple/Desktop/soft/gradle/gradle-4.6 
export PATH=$PATH:$GRADLE_HOME/bin
```

注: 将GRADLE\_HOME的值替换成你的Gradle路径

然后保存.bash\_profile文件,在终端上执行

```bash
source ~/.bash_profile
```

更新.bash\_profile文件

#### 验证

在终端上执行gradle -version，查看是否配置成功。如显示如下表示配置成功.

### 1.2 产品设计

* 产品表

| 序号 | 英文 | 中文 | 类型 | 长度 | 备注 |
| --- | --- | --- | --- | --- | --- |
| 1 | id | 编号 | varchar | 50 |  |
| 2 | name | 名称 | varchar | 50 |  |
| 3 | threshold\_amount | 起投金额 | decimal | 15,3 |  |
| 4 | step\_amount | 投资步长 | decimal | 15,3 |  |
| 5 | lock\_term | 锁定期 | smallint |  | 单位天 |
| 6 | reward\_rate | 收益率 | decimal | 5,3 | 0-100 |
| 7 | status | 状态 | varchar | 20 | 销售中，锁定，已结束 |
| 8 | memo | 备注 | varchar | 200 |  |
| 9 | create\_at | 创建时间 | datetime |  |  |
| 10 | create\_user | 创建者ID | varchar | 20 |  |
| 11 | update\_at | 更新时间 | datetime |  |  |
| 12 | update\_user | 更新者ID | varchar | 20 |  |

* 订单表

| 序号 | 英文 | 中文 | 类型 | 长度 | 备注 |
| --- | --- | --- | --- | --- | --- |
| 1 | order\_id | 订单编号 | varchar | 50 |  |
| 2 | chan\_id | 渠道编号 | varchar | 50 | 淘客公司编号 |
| 3 | product\_id | 产品编号 | varchar | 50 |  |
| 4 | chan\_user\_id | 用户编号 | varchar | 50 | 淘客公司用户编号 |
| 5 | order\_type | 订单类型 | varchar | 50 | 申购:apply,赎回:redeem |
| 6 | order\_status | 订单状态 | varchar | 50 | 初始化:INIT,处理中:PROCESS,成功:SUCESS,失败:FAIL |
| 7 | outer\_order\_id | 外部订单编号 | varchar | 50 | 淘客公司订单编号 |
| 8 | amount | 订单金额 | decimal | 15,3 |  |
| 9 | memo | 备注 | varchar | 200 |  |
| 10 | create\_at | 创建者时间 | datetime |  |  |
| 11 | update\_at | 更新时间 | datetime |  |  |


### 2. 管理功能 manager
* 开发的时候，要进行分块，且需要添加相应的依赖
```
\\ build.gradle
dependencies{
    compile project(":entity")
    compile libs.mysql
}
```
* applicaiton.yml可以设置上下文根
```
server:
  servlet:
    context-path: /manager
  port: 8081
```
* 如果错误提示entity没有注入，需要在ManagerApp里面增加注解，注明包扫描的路径
```
@EntityScan(basePackages = {"com.imooc.entity"})
```
* 异常需要统一处理
对于浏览器端的，可以通过在resource/public/error 添加5xx.html来实现映射

对于机器端的，需要自己写一个MyErrorController，并且继承BasicErrorController，来实现。

* 自动化测试
注意要测试覆盖率和边界条件
执行顺序可以通过@FixMethodOrder来完成

* swagger
 1. 需要配置依赖，在dependencies.gradle里面
 2. 重新新建swagger模块
 3. 重写resoueces/META-INF/resources/swagger-ui.html来实现国际化
 4. 创建EnableMySwagger注解类
 5. 个性化的配置，通过@ConfigurationProperties(prefix = "swagger")，并且在application.yml里面进行书写
 6. 在主工程里面添加@EnableMySwagger注解
 
### 3.销售端
利用jsonrpc来实现内部系统交互

#### 3.1 主要通过一个配置类来实现rpc
* 注意，如果rpc代理不是在本modal下，需要添加包路径扫描
* rpc路径的定义不能以斜杠开始
* rpc不要返回单个对象的那种，最好返回基本类型

##### 服务端

* 1.新增一个配置
```
@Configuration
public class RpcConfiguration {

    @Bean
    public AutoJsonRpcServiceImplExporter rpcServiceImplExporter(){
        return new AutoJsonRpcServiceImplExporter();
    }
}
```

* 2. 实现类必须添加`@AutoJsonRpcServiceImpl`注解以及交由spring管理的`@Service`注解
```
@AutoJsonRpcServiceImpl
@Service
public class ProductRpcImpl implements ProductRpc {

    private static Logger LOG = LoggerFactory.getLogger(ProductRpcImpl.class);

    @Autowired
    private ProductService productService;

    @Override
    public List<Product> query(ProductRpcReq req) {
        LOG.info("查询多个产品，请求：{}",req);
        Pageable pageable = new PageRequest(0,1000, Sort.Direction.DESC,"rewardRate");
        Page<Product> result = productService.query(req.getIdList(),req.getMinRewardRate(),req.getMaxRewardRate(),req.getStatusList(),pageable);
        LOG.info("查询多个产品，结果：{}",result);
        return result.getContent();
    }

    @Override
    public Product findOne(String id) {
        LOG.info("查询多个产品，请求：id:{}",id);
        Product result = productService.findOne(id);
        LOG.info("查询多个产品，结果：{}",result);
        return result;
    }
}
```

##### 客户端

```
@Configuration
@ComponentScan(basePackageClasses = {ProductRpc.class})
public class RpcConfiguration {

    private static Logger LOG = LoggerFactory.getLogger(RpcConfiguration.class);

    @Bean
    public AutoJsonRpcClientProxyCreator rpcClientProxyCreator(@Value("${rpc.manager.url}") String url){
        AutoJsonRpcClientProxyCreator creator = new AutoJsonRpcClientProxyCreator();
        try {
            creator.setBaseUrl(new URL(url));
        } catch (MalformedURLException e) {
            LOG.error("创建rpc服务地址错误",e);
        }
        creator.setScanPackage(ProductRpc.class.getPackage().getName());
        return creator;
    }
}
```