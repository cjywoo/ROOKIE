# linux的基本知识

## linux的目录结构

![](/assets/devops/1.png)

## 基本指令

```shell
ls // 列出文件内容
mkdir // 创建目录
touch // 创建文件
echo Thanks > hello.txt // 向文件写入
cat hello.txt //查看文件内容
cp 原始文件 新的文件 // 拷贝文件
-r 递归目录的内容
mv 原始文件 新的文件位置 // 移动文件
ls -l 文件  // 显示文件信息
```

## 文件权限

![](/assets/devops/2.png)

* 修改权限的语句方法
```
chmod 权限数字 文件名
```
* 权限数字
r-4 w-2 x-1 没有对应0

