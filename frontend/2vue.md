# 数据驱动
## new Vue发生了什么

1.`src/core/instance/index.js`
这边定义了vue的构造函数

``` javascript
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}
```
主要的函数在`_init`这个函数上，他定义在`initMixin`这个方法里。

2.`src/core/instance/init.js`


``` javascript

```


``` javascript

```


``` javascript

```


``` javascript

```