# 了解下 new 这个操作到底发生了什么

首先了解new做了什么，使用new关键字调用函数（new ClassA(…)）的具体步骤：

1. 创建一个新对象：
``` javascript
var obj = {};
```
2. 设置新对象的constructor属性为构造函数的名称，设置新对象的```__proto__```属性指向构造函数的prototype对象；
``` javascript
obj.__proto__ = ClassA.prototype;
```
3. 使用新对象调用函数，函数中的this被指向新实例对象：
``` javascript
ClassA.call(obj);　　//{}.构造函数()
```
4. 将初始化完毕的新对象地址，保存到等号左边的变量中


> 若构造函数中返回this或返回值是基本类型（number、string、boolean、null、undefined）的值，则返回新实例对象；若返回值是引用类型的值，则实际返回值为这个引用类型。

## 通过一个方法来实现

```
function NEW(f){
    return function(){
        var obj = {"__proto__":f.prototype};
        f.apply(obj,arguments);
        return obj
    }
}

function Person(name,age){
    this.name = name;
    this.age = age;
}

var p2 = New(Person)("Jack",25);
p2.name;//Jack
p2.age;//25
```