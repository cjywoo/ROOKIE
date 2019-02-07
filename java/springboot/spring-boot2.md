# spring boot 打包

打包命令
```
mvn -Dmaven.test.skip -U clean package

```

注意点
* 模快化后，主工程一般修改为web，则需要将build拷贝至web的pom文件下，且增加configuration->mainClass