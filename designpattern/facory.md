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


