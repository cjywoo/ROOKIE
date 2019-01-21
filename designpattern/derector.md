# 装饰器模式

* 为对象添加新功能
* 不改变原有的结构和功能

## 1.JS实现

### 1.1 UML图

![](/assets/decorator/import1.png)

### 1.2 es5实现



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

* 例子1

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

* 例子2

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

#### 装饰器方法

定义一个方法，入参有target,name,descriptor

* 例子1

```
function readonly (target,name,descriptor) {
    descriptor.writable = false;
    return descriptor
}

class Person {
    constructor(){
        this.first = 'A'
        this.last = 'B'
    }

    @readonly
    name() {
        return `${this.first} ${this.last}`
    }
}

let p = new Person();
p.name  = 'aa'

```

* 例子2

```
function log(target,name,descriptor){
    let oldvalue = descriptor.value
    descriptor.value = function(){
        console.log(`calling ${name} with`, arguments)
        return oldvalue.apply(this,arguments)
    }
}

class Math{
    @log
    add(a,b){
        return a+b
    }
}

let math = new Math()
console.log(math.add(2,4))
```

### 第三方开源的装饰器lib core-decorators

* 例子1

```
import {readonly} from 'core-decorators'

class Person {
    constructor(){
        this.first = 'A'
        this.last = 'B'
    }

    @readonly
    name() {
        return `${this.first} ${this.last}`
    }
}

let p = new Person();
p.name  = 'aa'

```

