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
