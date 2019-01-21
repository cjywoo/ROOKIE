# 适配器模式

* 旧接口格式和使用者不兼容
* 中间需要加一个适配转换接口
* 装饰者模式作用是为了给对象增加功能，会形成一条长的作用链条，而适配器模式通常只包装一次
* 代理模式是为了控制对对象的访问，通常也只包装一次，但与适配器还是有所不同



## 1.JS实现
### 1.1 es5实现
``` javascript
var googleMap = {
    show:function(){
        console.log("使用google地图渲染");
    }
}

var baiduMap = {
    display:function(){
        console.log("使用百度地图渲染")
    }
}

var baiduMapAdaptor = {
    show:function(){
        baiduMap.display();
    }
}

var renderMap = function(map){
    if(map.show instanceof Function){
        map.show();
    }
}

renderMap(googleMap);
renderMap(baiduMapAdaptor);
```


### 1.2 es6实现

``` javascript

//适配器模式

class Adaptee {
    secificRequest(){
        return '德国标准插头'
    }
}


class Target {
    constructor(){
        this.adaptee = new Adaptee();
    }

    request(){
        return `${this.adaptee.secificRequest()}--转换器--中国插头`
    }
}

let target = new Target();
console.log(target.request())
```

#### 应用场景
1 兼容老接口

``` javascript
//新的接口
ajax({}).done(function(){})

//旧的接口
$.ajax(option)

//适配器

var $ ={
    ajax:function(option){
        return ajax(option).done(option.success)
    }
} 
```

2 vue的computed属性

``` html

    <div id="app">
        <p>顺序：{{message}}</p>
        <p>逆序：{{reverseMsg}}</p>
    </div>
    
    var vm  = new Vue({
        el:"#app",
        data:{
            message:'hello'
        },
        computed:{
            reverseMsg:function(){
                return this.message.split('').reverse().join('')
            }
        }
    })

```
