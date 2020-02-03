# webpack工具

## webpack的核心概念
* 入口: entry
* 输出: output
* Loader: module 
* 插件: plugins
* 模式/兼容性: 利用mode属性，可以切换development和production

> loader和插件的区别： loader去处理webpack不能处理的文件(即除了js文件)，转化为可以处理的文件； 插件的范围更广，从打包优化到压缩。

## 开发与生产模式中，按需配置
开发中使用热更新，生产模式中使用压缩插件

## babel配置
使用.babelrc配置文件，polyfile

## 如何调试

* 利用 node --inspect-brk指令进行调试
```
npx node --inspect-brk ./node_modules/.bin/webpack --inline --progress
Debugger listening on ws://127.0.0.1:9229/47e86a2e-1dfb-4ea3-8412-23be9b0277ae
```
* 打开chrome
```
# 访问 chrome://inspect/#devices
```
下放选择webpack的地方，点击inspect即可
