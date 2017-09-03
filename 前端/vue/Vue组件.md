# 组件
## 组件的命名
  组件的命名最好是以`xx-xxx`的方式符合w3c的标准，如果使用的是驼峰命名法的话shpComponent，那么将用shp-component来使用，因为
  html不区分大小写
## 使用
  他可以在原生的html上进行扩展用`is`属性
## 组件间的通信
  通常一般都会有父子组件，父子组件之间的通信主要是
  1. 父组件通过props对象传递数据
  2. 子组件通过事件来告诉父组件消息
  
  **注意** 组件的数据流是单向的
## 注意事项
### data
  创建组件时的data对象必须是function方法，否则会报错
### 动态数据
  比如在组件中定义了`props`属性，那么如果想要属性的值可以动态绑定
  那么就需要使用v-bind这个命令，在数据改变的时候可以捕获到并传递给相应
  的子组件。
### 字面量语法与动态语法
  字面量语法
  
    <!-- 传递了一个字符串 "1" -->
    <comp some-prop="1"></comp>
    
  动态语法
  
    <!-- 传递实际的 number -->
    <comp v-bind:some-prop="1"></comp>
    
  也就是说如果你想传递一个其他类型的不是字符串类型的那么就需要使用v-bind命令来进行传值。

### 单向数据流
  也就是说父组件的props可以变化并传给所有的子组件，但是反过来就不可以，目的是防止随意修改父组件属性导致数据流难理解。
  1. 如果想使用局部变量的话就使用data对象建立一个其他的变量并可以用props的值来进行初始化。
  2. 如果想对props的值做一些处理那么他可以使用计算属性来对props中的值进行处理。
   
   **注意不要引用同一个外部对象** 因为javascipt的对象和数组都是引用类型，会指向同一块内存空间，如果子组件改变那么也会改变父组件变量。
### props验证
  可以对props中的属性进行类型或者是否必须的属性的验证，如果类型不匹配等等的条件不满足则会报错。
  
    // 基础类型检测 (`null` 意思是任何类型都可以)
        propA: Number,
        // 多种类型
        propB: [String, Number],
        // 必传且是字符串
        propC: {
          type: String,
          required: true
        },
        // 数字，有默认值
        propD: {
          type: Number,
          default: 100
        },
        // 数组/对象的默认值应当由一个工厂函数返回
        propE: {
          type: Object,
          default: function () {
            return { message: 'hello' }
          }
        },
        // 自定义验证函数
        propF: {
          validator: function (value) {
            return value > 10
          }
        }
    
   注意 props 会在组件实例创建之前进行校验，所以在 default 或 validator 函数里，诸如 data、computed 或 methods 等实例属性还无法使用。

### 非props属性
   比如不是props的属性可以直接添加到我们自定义的组件上，比如就是在与bootstrap等第三方框架集成的时候，可以直接添加一些属性。这些属性添加之后
   会自动的添加到组件的根元素中。

### 替换和覆盖
   如果传递的属性或特性覆盖了

  
  