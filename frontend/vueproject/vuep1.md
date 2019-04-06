# Vue 基础

## 1. Vue的基础概念

vue是MVVM模型，是Modal-View-ViewModal,主要就是开发modal层  
![](/assets/vue/vue1.png)

## 2. Vue的组件

可以通过components属性来构建组件，通过v-bind实现父组件向子组件传递数据，通过this.$emit实现子组件向父组件传递  
示例代码如下：

```javascript
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

## 4.Vue的样式绑定

* `:class="{样式:boolean变量}"`
* `:class=[数组变量]`
* `:style="{样式对象}"或者 :style="[样式数组对象]"`

## 5.Vue的动画

vue在执行动画时，会在外层包裹一个transition，然后动画的过程如下方所示。
* 显示的时候
![](/assets/vue/vue1-1.png)

* 隐藏的时候
![](/assets/vue/vue-1-2.png)

## 6.Vue的组件
### 6.1 组件使用的注意事项
* 一些比如tbody,ul,select,ol等标签，因为html5标准中要求必须里面跟上tr,li,option等标签，导致使用子组件时候渲染会有问题，因此可以采用is属性，
```
<tbody>
    <tr is="row"></tr>
</tbody>
```

### 6.2 data属性
data属性在跟路径下，是一个对象类型是可以的。但是在其他比如组件下定义时，必须返回一个函数
```
new Vue({
    data:{}
})

Vue.component('row',{
    data:function(){
        return {
            content:''
        }
    }
})
```

### 6.3 ref属性

获取dom属性,如果标记在组件上，则会显示组件的引用。
```
<div id="root">
        <div ref="hello" @click="handleClick">
            hello world
        </div>
    </div>
    <script>
        var vm = new Vue({
            el: '#root',
            methods: {
                handleClick:function(){
                    console.log(this.$refs.hello.innerHTML);
                }
            },
        })
    </script>
```
