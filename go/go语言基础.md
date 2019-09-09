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


