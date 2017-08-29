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
