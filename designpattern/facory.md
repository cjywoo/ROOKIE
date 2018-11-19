# 工厂模式

> 一般涉及到new 操作符

## js实现方法

### UML类图
![](/assets/uml.png)

### 实现代码
```
class Prodect{
    constructor(name){
        this.name = name
    }

    init(){
        console.log('init')
    }

    fn(){
        console.log('fn')
    }
}

class Create{
    create(name){
        return new Prodect(name)
    }
}

//test
let factory = new Create();
let p = factory.create('a')
console.log(p.name)
p.init()
p.fn()
```

### 场景实现
1. jQuery $("div")

```
class jQuery{
    constructor(selector){
        let slice = Array.prototype.slice
        let dom = slice.call(document.querySelectorAll(selector)) //将多个dom节点转化为数组
        let len = dom ?dom.length :0
        for(let i =0;i<len;i++){
            this[i] = dom[i]
        }
        this.length = len
        this.selector = selector || ''

    }

    append(node){

    }

    addClass(name){

    }

    html(data){

    }

    //此处省略n个api
}

window.$ = function(selector){
    return new jQuery(selector)
}
```

