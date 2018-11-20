# 静态方法(属性)、实例方法(属性)、内部方法(属性)和原型

## 1.属性
### 私有属性
只能自己使用的属性

```
( function(){
    var a = '私有属性'
})();
alert(a); //error a没有定义
```

### 实例属性 
实例对象可以访问的属性
```
var a = function(){
    this.x = "实例属性";
}
var t1 = new a();
var t2 = new a();
t1.x != t2.x //t1和t2的x各自改变不影响
```

### 原型属性
首先访问实例属性，然后再访问原型属性

```
var a = function(){
    this.x = "实例属性";
};
a.prototype.x = "原型属性";
var t = new a();
alert(t.x); //实例属性
delete(t.x);
alert(t.x) //原型属性

```

### 类属性
```
var a = function(){}
a.x = "类属性"
var t= new a();
t.x //不存在x属性
```
