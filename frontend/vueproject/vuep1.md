# Vue 基础

## 1. Vue的基础概念

vue是MVVM模型，是Modal-View-ViewModal,主要就是开发modal层
![](/assets/vue/vue1.png)

## 2. Vue的组件
可以通过components属性来构建组件，通过v-bind实现父组件向子组件传递数据，通过this.$emit实现子组件向父组件传递
示例代码如下：
``` javascript
components:{
                TodoItem:{
                    props:['content'],
                    template:"<li>{{content}}</li>"
                }
            }

```

## 3.Vue的计算属性
* 计算属性`computed`有缓存，如果值没有发生变化，不会重新计算
* `watch`属性，也只是变化才会触发
* `computed`有get和set方法

