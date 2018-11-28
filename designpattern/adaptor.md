# 适配器模式

> 旧接口格式和使用者不兼容
> 中间需要加一个适配转换接口

## 1.JS实现
### 实现代码

```
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

### 应用场景
1.兼容老接口
```
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
2. vue的computed属性