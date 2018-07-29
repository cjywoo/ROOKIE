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
