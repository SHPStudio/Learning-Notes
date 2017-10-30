## let与var
 let是一个块级作用域的本地变量然而var是一个全局或者一整个函数块的作用域。
### 场景介绍
    function varTest() {
      var x = 1;
      if (true) {W
        var x = 2;  // 同样的变量!
        console.log(x);  // 2
      }
      console.log(x);  // 2
    }
    W
    function letTest() {
      let x = 1;
      if (true) {
        let x = 2;  // 不同的变量
        console.log(x);  // 2
      }
      console.log(x);  // 1
    }
## 正则表达式
### 介绍
 正则表达式是用来匹配字符串中字符组合的一种形式，在JavaScript中正则表达式也是一种对象。

## 返回上一页

### history.goback();
  任何地方都可以使用history.goback()去返回上一页

## 登录拦截前端跳转页面
### location
  这个对象包含了域名和锚点还有一些其他的请求参数，可以利用这些来进行登录链接的跳转，
  一般的登录拦截会跳到登录页面，登录页面会有相应的请求参数去记录要回跳的链接。
  所以我们可以使用location对象进行拼接。
