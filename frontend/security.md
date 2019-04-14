# Web前后端漏洞分析与防御

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



