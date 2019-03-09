# springboot 数据源配置

## 如何配置单数据源

### 步骤
1. 引入对应数据库驱动 --h2
2. 引入jdbc依赖 -- spring-boot-starter-jdbc
3. 获取 DataSource Bean 打印信息
4. 通过/acturator/beans 查看Bean

### 项目
可以参考 /program/springboot/jdbcdemo

利用了h2的内嵌数据库，结合data.sql和schema.sql可以做数据库的初始化