# 组件化

## 1.createComponent

在分析createElement的实现的时候，最终会调用_createElement方法，如果不是一个普通的tag标签，则会进入createComponent方法创建一个组件vnode，主要有以下几步。

* 在createElement方法中，关键语句为`vnode = createComponent(Ctor, data, context, children, tag)`
* 主要方法在`src/vdom/create-components.js`中
* 首先构造子类的构造函数
* 其次安装钩子函数
* 最后实例化vnode

### 构造子类构造函数


``` javascript
const baseCtor = context.$options._base

  // plain options object: turn it into a constructor
  if (isObject(Ctor)) {
    Ctor = baseCtor.extend(Ctor)
  }
```
其实，context.$options._base指向的就是vue,因此vue.extend定义在`src/core/global-api/extend.js`中，主要的作用就是构造一个vue的子类，是一种非常经典的原型继承发方法，把一个纯对象转换为一个继承与vue的构造器sub，并且对这个构造函数进行了缓存。

### 安装钩子函数
在初始化component类型的vnode过程中，会实现init,prepatch,insert,destory四个钩子函数，通过
``` javascript
installComponentHooks(data)
```
来实例化钩子函数，并且存在了data.hook中

### 实例化vnode
最后一步即实例化vnode
```javascript
  const name = Ctor.options.name || tag
  const vnode = new VNode(
    `vue-component-${Ctor.cid}${name ? `-${name}` : ''}`,
    data, undefined, undefined, undefined, context,
    { Ctor, propsData, listeners, tag, children },
    asyncFactory
  )
```

需要注意，普通元素节点是有childern的，而组件的children是没有的，都在componentOptions这个属性里面。

## 2.patch
通过createComponent创建组件Vnode,接下来会走到vm._update，执行vm._patch_把vnode转成真正的dom节点。


