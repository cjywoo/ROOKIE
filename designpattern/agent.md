# 代理模式

使用者无权访问目标对象  
中间加代理，通过代理做授权和控制

## js实现

### UML图

![](/assets/agent/import1.png)

### 代码实现

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

