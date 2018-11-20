# 第一章 前期基础准备

## 1.Flow
因为js是动态类型语言，所以需要flow进行静态类型检查工具


## 2.vue的目录结构
Vue.js 的源码都在 src 目录下，其目录结构如下。

```
src
├── compiler        # 编译相关 
├── core            # 核心代码 
├── platforms       # 不同平台的支持
├── server          # 服务端渲染
├── sfc             # .vue 文件解析
├── shared          # 共享代码
```
## 3.vue的构建
Vue.js 源码是基于 Rollup 构建的，它的构建相关配置都在 scripts 目录下。

## 4.vue的初始化
vue本质上就是一个用 Function 实现的 Class，然后它的原型 prototype 以及它本身都扩展了一系列的方法和属性