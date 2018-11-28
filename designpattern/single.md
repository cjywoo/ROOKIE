# 单例模式

> 在一些服务类中，有且仅有一个实例供其他程序使用

## 1.js实现

### 实现代码

``` javascript
//单例模式

class SingleObject {
    login(){
        console.log('login...')
    }
}

SingleObject.getInstance = (function(){
    let instance;
    return function(){
        if(!instance) {
            instance = new SingleObject()
        }
        return instance
    }
})()

let obj1 = SingleObject.getInstance();
obj1.login()
let obj2 = SingleObject.getInstance()
obj2.login()

console.log(obj1 === obj2)
```