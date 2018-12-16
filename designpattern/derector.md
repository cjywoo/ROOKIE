# 装饰器模式

* 为对象添加新功能
* 不改变原有的结构和功能

## 1.JS实现

### UML图

![](/assets/decorator/import1.png)

### 代码实现

```
class Circle{
    draw(){
        console.log("画一个原行")
    }
}

class Decorator {
    constructor(circle){
        this.circle = circle
    }
    draw() {
        this.circle.draw();
        this.setRedBorder(circle)
    }
    setRedBorder(circle){
        console.log("设置红色边框")
    }
}

let circle = new Circle();
circle.draw();
let dec = new Decorator(circle);
dec.draw();
```

### 场景
#### 1.es6的装饰器类

首先安装插件`npm install babel-plugin-transform-decorators-legacy --save-dev`，然后再.babelrc配置文件里面加上"plugins": ["transform-decorators-legacy"]即可。

例子说明1
```
function testDec(isDec){
    return function(target){
        target.isDec = isDec
    }
}

@testDec(false)
class Demo {

}

alert(Demo.isDec) //false
```

例子说明2

```
function mixins(...list){
    return function(target){
        Object.assign(target.prototype, ...list)
    }
}

const Foo = {
    foo(){
        alert('foo')
    }
}

@mixins(Foo)
class MyClass {

}

let obj = new MyClass();
obj.foo(); //foo
```

#### readonly装饰属性

