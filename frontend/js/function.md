# function调用

函数调用有四种形式

1. 作为一个函数(function) ————————skulk();
2. 作为一个对象的方法(method) ————————ninja.skulk();
3. 作为一个构造函数(constructor) ————————new Ninja()
4. 通过函数的apply或者call方法 ———————— skulk.apply(ninja)

## 作为函数被调用
skulk()
在非严格模式下，this是全局上下文，在严格模式下，this是undefined。

## 作为对象的方法调用
this是该对象。

## 作为构造函数调用
* 如果构造函数返回一个对象，则该对象将作为整个表达式的值返回而传入构造函数的this将被丢弃。
* 如果，返回的是非对象类型，则忽略返回值，返回新创建的对象。

## 作为apply和call方法
this会绑定到对象上

```
function juggle(){}
var ninja1,ninja2;
juggle.apply(ninja1,[1,2,3]);
juggle.call(ninja2,1,2,3);
```

# 解决函数上下文
## 使用箭头函数
箭头函数没有单独的this值，箭头函数的this与声明所在的上下文的相同。
但是要注意，在全局对象的属性中，使用箭头函数，this指向的windows
## 使用Bind方法
var newfunc = function.bind(object) 返回一个新的函数,这个函数的this对象绑定在object上。
