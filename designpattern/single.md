# 单例模式

> 在一些服务类中，有且仅有一个实例供其他程序使用

## 基本模式
### 1.js实现

#### 实现代码1

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

#### 实现代码2

``` javascript
var CreateDiv = function( html ){
    this.html = html;
    this.init(); 
};
CreateDiv.prototype.init = function(){
    var div = document.createElement( 'div' ); 
    div.innerHTML = this.html; 
    document.body.appendChild( div );
};
var ProxySingletonCreateDiv = (function(){
    var instance;
    return function( html ){
        if ( !instance ){
            instance = new CreateDiv( html );
        }
        return instance; 
    }
})();
var a = new ProxySingletonCreateDiv( 'sven1' ); 
var b = new ProxySingletonCreateDiv( 'sven2' );
alert( a === b );  //true

```

## 懒汉模式(惰性模式)
### js实现
```
var getSingle = function( fn ){
   var result;
   return function(){
       return result || ( result = fn .apply(this, arguments ) );
   } 
};
var createLoginLayer = function(){

    var div = document.createElement( 'div' );
    div.innerHTML = '我是登录浮窗';
    div.style.display = 'none';  
    document.body.appendChild( div );

    return div;
};
var createSingleLoginLayer = getSingle( createLoginLayer );
document.getElementById( 'loginBtn' ).onclick = function(){ 
    var loginLayer = createSingleLoginLayer(); 
    loginLayer.style.display = 'block';
};
```