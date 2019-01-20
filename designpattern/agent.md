# 代理模式

* 保护代理：使用者无权访问目标对象，中间加代理，通过代理做授权和控制
* 虚拟代理：如果一个事情开销很大，则需要由代理决定什么时候做


## 1.js实现

### 1.1 UML图

![](/assets/agent/import1.png)

### 1.2 es5实现

```

var Flower = function(){
    console.log("create a flower");
}

var xiaoMin = {
    sendFlower:function(proxy){
        var flower = new Flower();
        proxy.receiveFlower(flower);
    }
}

var Proxy = function(target){
    this.target = target;
}
Proxy.prototype.receiveFlower = function(flower){
    var self = this;
    self.target.listenGoodMood(function(){
        console.log("target is good mood");
        self.target.receiveFlower(flower);
    })
}

var Target = {
    receiveFlower:function(flower){
        console.log("I receieve Flower");
    },
    listenGoodMood:function(fn){
        setTimeout(function(){
            console.log("now I am good mood");
            fn();
        },2000);
    }
}

var proxy4xiaomi = new Proxy(Target);
xiaoMin.sendFlower(proxy4xiaomi);

```


### 1.2 es6代码实现

```
class RealImg {
    constructor(fileName) {
        this.fileName = fileName
        this.loadFromDisk()
    }

    loadFromDisk(){
        console.log(`loading... ${this.fileName}`)
    }

    display(){
        console.log(`display... ${this.fileName}`)
    }
}

class ProxyImg{
    constructor(fileName){
        this.realImg = new RealImg(fileName)
    }

    display(){
        this.realImg.display()
    }
}

//test
let proxyImg = new ProxyImg('1.png')
proxyImg.display()
```

### 1.3 场景演示

* 1.网页事件代理

```
即addEventListener('click')
```

* 2.$.proxy

```
<body>
    <div id="test">
        <a href="#">a1</a>
        <a href="#">a2</a>
        <a href="#">a3</a>
        <a href="#">a4</a>
        <a href="#">a5</a>
    </div>
    <script src="https://code.jquery.com/jquery-2.2.4.js"></script>
  <script>
      $("#test").click(function(){
          var fn = function(){
              $(this).css('background-color','yellow')
          }
          fn = $.proxy(fn,this)
          setTimeout(fn,1000)
      })
  </script>
</body>
```

* 3.es6 Proxy

```
let star = {
    name:'陈骏宇',
    mobile:'1506******5',
    age:'28'
}

let manager = new Proxy(star,{
    get:function(target,key){
        if(key == 'mobile'){
            return '无可奉告'
        }
        if(key == 'price'){
            return 'i00,000,000'
        }
        return target[key]
    },
    set:function(target,key,val){
        if(key == 'customprice'){
            if(val <1000){
                throw new Error('价格太低')
            }else{
                target[key] = val
                return true
            }
        }
    }
})
 
console.log(manager.name)  //陈骏宇
console.log(manager.mobile) //无可奉告
console.log(manager.price) //100,000,000
manager.customprice = 50000
console.log(manager.customprice) //50000
manager.customprice = 100
```

# 1.4 es5实现