# 外观模式

## 1.js 实现方式

### 1.1 es5实现方式1

```
function bindEvent(elem,type,selector,fn) {
    if(fn == null && selector instanceof Function){
        fn = selector;
        selector = undefined;
    }
    // ***
}

//调用
bindEvent(elem,'click','#div1',fn);
bindEvent(elem,'click',fn)
```