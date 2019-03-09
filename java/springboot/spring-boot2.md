# spring boot 打包

打包命令
```
mvn clean package -Dmaven.test.skip -U

```

打包之后，会在target目录下产生一个.jar文件，然后直接使用以下命令就可以运行
```
java -jar *.jar
```

注意点
* 模快化后，主工程一般修改为web，则需要将build拷贝至web的pom文件下，且增加configuration->mainClass
* 如果要打包成war包，需要修改配置文件，并在web模块下增加webapp/WEB-INF/web.xml文件，并且在web的目录下的pom.xml，增加package配置文件，值为war