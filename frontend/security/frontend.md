# Web前端漏洞分析与防御

## 主要关注点

* 私密性 不被非法获取和利用
* 可靠性 不丢失不被篡改
* 前端关注点
  * 跨站脚本攻击 xss
  * 跨站请求伪造 CSRF
  * 前端Cookie安全性
  * 点击劫持
  * 传输安全问题
  * SQL注入攻击
  * 信息泄露和社会工程学
  * 重放攻击

## 1. XSS


![](/assets/front/safety1.png)

### 1.1 可以实现功能
* 获取页面数据
* 获取cookie
* 劫持前端请求
* 发送请求
* 偷取用户数据、资料

### 1.2 XSS的攻击类型分类
* 反射型 URL直接注入
  攻击方法：http://localhost/?from=%3Cscript%3Ealert(%27haha%27)%3C/script%3Egoogle
* 存储型 存储到DB中读取时注入
  攻击方法：评论里面注入<script>注入脚本</script>

### 1.3 XSS的攻击注入点
注入点有以下几点：
* HTML节点内容
* HTML属性
  ```
  <img src="1" onerror="alert(1)" >
  ```
* javascript代码
  ``` 
  var data = "#{data}";
  var data = "hello";alert(1);"";
  ```
* 富文本

### 1.4 如何防御
#### XSS浏览器自带的防御
浏览器自带的XSS防御只能防止反射到Html中的属性和内容。
koa2里面通过设置
```
ctx.set('X-XSS-Protection',0); //0是关闭，1是打开
```
#### 防御html和html属性
```
// 转义html
var escapeHTML = function(str){
	if(!str) return '';
	str = str.replace(/</g, '&lt;');
	str = str.replace(/>/g, '&rt;');
	return str;
}

// 转义html属性
var escapeHTMLProperty = function(str){
	if(!str) return '';
	str = str.replace(/"/g, '&quto;');
	str = str.replace(/"/g, '&#39;');
	return str;
}
```

#### 防御javascript
```
// 在js的变量的地方加上
JSON.stringify()
```

#### 防御富文本标签
通过白名单过滤，通过js-xss插件来实现

#### CSP
内容安全策略
[如何防御](https://blog.csdn.net/maquealone/article/details/79550144)

## 2. CSRF
跨站请求伪造
### 2.1 攻击原理
B网站冒用A网站的cookie，然后向A网站发送请求

### 2.2 如何进行防御
1）禁止第三方网站带Cookie, 利用cookie中的same-site属性为struct
2) 在A网站加入验证信息
  * 利用ccap插件在前端加入验证码机制
  * 利用token来进行防御,对表单中csrftoken和cookie中的csrftoken进行比对
  * referer进行防御

