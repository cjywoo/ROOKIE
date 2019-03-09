# springBoot 连接池

## 好用的连接池 HikariCP

### 为什么这么快
1. 字节码级别优化
2. 大量小改进
 * 用FastStatemntList代替ArrayList
 * 无锁集合
 
### 在Spring Boot中的配置

Spring Boot 2.x 
* 默认使用HikariCP
* 配置spring.datasource.hikari.* 配置

Spring Boot 1.x
* 默认使用Tomcat连接池，需要移除tomcat-jdbc依赖
* spring.datasource.type = com.zaxxer.hikari.HikariDataSource

## 好用的连接池 Alibaba Druid 
### 特点
* 详细的监控
* 防止sql注入
* 内置加密配置