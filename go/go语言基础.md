# Go语言基础
## 变量声明
`var [名称] [类型]`
eg: `var a, b *int` 表示a,b为int的指针类型

go语言的基本类型：
- bool
- string
- int int8 int16 int32 int64
- uint uint8 uint16 uint32 uint64 uintptr
- byte // uint8的别名 
- rune // int32的别名代表一个Unicode码
- float32 float64
- complex64 complex128

变量声明后，系统自动赋予该类型的零值，int为0，float为0.0 bool为false string为空字符串 指针为nil。

变量命名遵循驼峰命名

变量声明的几种方式：
- var [名字] [类型]
- 批量
```
var (
  a int
  b string
  c []float32
  d func() bool
  e struct {
     x int
  }
)
```
- 简短格式 `名字:=表达式`，不过它有一些限制 
1. 定义变量同时需要显式初始化
2. 不能提供数据类型
3. 只能用在函数内部

## 匿名变量
`_`，任何赋给该特殊标识符的变量的值都会被抛弃，后续代码也无法使用或者进行某些运算。
匿名变量不占用命名空间，不会分配内存。

## 内置类型
内置类型是由语言提供的一组类型。它们分别是数值类型、字符串类型和布尔类型。
