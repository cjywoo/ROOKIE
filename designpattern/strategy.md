# 策略模式


## 1.js实现

### es5的实现方式
```
var strategies = {
    isNonEmpty:function(value,errorMsg){
        var flag = true;
        if( value == ''){
            flag = false;
        }
        return { "flag":flag, "errorMsg":errorMsg }
    },
    minLength:function(value,length,errorMsg){
        var flag = true;
        if(value.length < length){
            flag = false;
        }
        return { "flag":flag, "errorMsg":errorMsg }
    },
    isMobile: function( value, errorMsg ){ // 手机号码格式
        var flag = true;
        if ( !/(^1[3|5|8][0-9]{9}$)/.test( value ) ){ 
            flag = false;
        } 
        return { "flag":flag, "errorMsg":errorMsg }
    }
}

var Validate = function(){
    this.cache = [];
}

Validate.prototype.add = function( dom,rule,errorMsg ){
    this.cache.push(function(){
        if(!dom){
            return {"flag":false,"errorMsg":errorMsg}
        }
        var array = rule.split(":");
        var strategy = array.shift();
        array.unshift(dom.value);
        array.push(errorMsg);
        return strategies[strategy].apply(dom,array);
    })
}

Validate.prototype.start = function(){
    for(var i =0, len = this.cache.length;i<len;i++){
        var result = this.cache[i]();
        if(!result.flag){
            console.log(result.errorMsg);
            return false;
        }
    }
    return true;
}

var formValidate = function(form){
    var validate = new Validate();
    validate.add(form.username,"isNonEmpty","用户名不能为空");
    validate.add(form.mobile,"isMobile","手机号格式不正确");
    validate.add(form.password,"minLength:6","密码不得小于6位");
    return validate.start();
}

var form = document.getElementById("demo");
form.onsubmit = function(){
    return formValidate(form);
}
```