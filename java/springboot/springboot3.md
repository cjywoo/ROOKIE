# 通过一个理财系统的实战，来了解整个spring boot

## 整个系统的脉络

![](/assets/屏幕快照 2019-02-07 下午1.52.29.png)

## 系统的业务流程

![](/assets/屏幕快照 2019-02-07 下午1.55.08.png)

## 模块设计

![](/assets/屏幕快照 2019-02-07 下午2.00.07.png)

## 环境准备
### 下载Gradle

下载地址：[点击下载gradle](https://link.jianshu.com?t=http%3A%2F%2Fservices.gradle.org%2Fdistributions%2F)

下载最新版本:gradle-4.6 \(当前最新版2018年3月9日\),记得要下载all版本的,里面包含了Gradle SDK 所有相关的内容,包括源代码、文档、示例等.

### 配置Gradle环境

将下载的gradle解压,我的Gradle存放路径：/Users/Apple/Desktop/soft/gradle  
 要运行Gradle,必须把GRADLE\_HOME/bin 目录添加到环境变量PATH的路径才可以,在终端中输入：

``` bash
open -e .bash_profile
```

打开.bash\_profile文件,在.bash\_profile文件中输入下面内容

``` bash
GRADLE_HOME=/Users/Apple/Desktop/soft/gradle/gradle-4.6 

export GRADLE_HOME

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

