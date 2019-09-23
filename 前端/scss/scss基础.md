## &父选择器
```
a {
  font-weight: bold;
  text-decoration: none;
  &:hover { text-decoration: underline; }
  body.firefox & { font-weight: normal; }
}
```
会被编译为
```
a {
  font-weight: bold;
  text-decoration: none; }
  a:hover {
    text-decoration: underline; }
  body.firefox a {
    font-weight: normal; }
```

## 命名空间
```
.funky {
  font: {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}
```
编译为
```
.funky {
  font-family: fantasy;
  font-size: 30em;
  font-weight: bold; }
```

# $变量
sass支持用脚本的方式来开发样式，可以声明一个变量`$width: 5em`,
然后就可以在样式中使用这个变量
```
#main {
  width: $width;
}
```
局部变量就是在样式内部声明的变量，如果想把局部变量转换为全局变量可以添加`!global`来声明。
```
#main {
  $width: 5em !global;
  width: $width;
}

#sidebar {
  width: $width;
}
```

## 字符串处理
scss中字符串有两种一种带引号的，另外一种不带引号，但是如果想用变量来处理样式时的选择器写法是不带引号的，所以可以使用