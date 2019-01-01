# console中的一些方法

## console.assert
当我们传入的第一个参数为假时，console.assert打印跟在这个参数后面的值。当你需要在特殊情况下打印一些信息的时候这很有用 - 现在你可以在不使用if表达式的情况下做到这件事情。另外，你还可以白白得到一个堆栈的跟踪信息 ；-）

```
var value = null
undefined
console.assert(value,"前面为false则打印")
VM533:1 Assertion failed: 前面为false则打印
(anonymous) @ VM533:1

```

