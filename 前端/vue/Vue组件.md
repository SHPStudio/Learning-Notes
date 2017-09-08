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
   如果传递的属性或特性可能会覆盖组件自己定义的一些属性或者特性，比如就是样式问题，那么vue会把
   样式进行融合。
### 自定义事件
   使用$on绑定事件,使用$emit来触发事件，主要子组件触发父组件中的事件方法。
### 非父子组件间的通信
   也就是不同组件之间的通信，可以使用一个空的vue对象来做桥接，接收端利用钩子函数也就是生命周期函数绑定空对象中的事件，发送端则触发事件即可。
### 给原生组件绑定事件
  利用修饰符`.native`
### 更新父组件的值
   如果想让props双向绑定，可以使用.sync来使子组件也可以更新父组件的值。
   不过需要在子组件中显式的去触发方法。
   
        <comp :foo.sync="bar"></comp>
        扩展为
        <comp :foo="bar" @update:foo="val => bar = val"></comp>
        
   显式调用
    
        this.$emit('update:foo', newValue)
        
### 使用slot来分发内容
  可以使用slot来分发内容，也就是说子组件可以定义一个slot区域，来给父组件使用的时候往这里填充东西。
1. 单个slot

        子组件
        <div>
          <h2>我是子组件的标题</h2>
          <slot>
            只有在没有要分发的内容时才会显示。
          </slot>
        </div>
        父模板
        <div>
          <h1>我是父组件的标题</h1>
          <my-component>
            <p>这是一些初始内容</p>
            <p>这是更多的初始内容</p>
          </my-component>
        </div>

2. 具名slot
   具名slot可以在slot标签上加入`name`属性来指定slot是属于哪个域下的，比如可能是想在header中或者footer中
   除了添加具名slot外，还可以添加一个默认的slot，添加的内容如果没有指定的slot，默认都会添加到这里。
3. 作用域插槽
   是可以往slot里传入属性值，这样在父组件使用的时候，必须使用template指定一下scope一个临时变量，去引用slot标签上的属性。
   
        <div class="child">
          <slot text="hello from child"></slot>
        </div>
        
        <div class="parent">
          <child>
            <template scope="props">
              <span>hello from parent</span>
              <span>{{ props.text }}</span>
            </template>
          </child>
        </div>

### 动态组件
   动态组件的作用就是多个模板共用一个挂载点可以动态的切换。使用component标签的is属性去动态的切换。需要把is属性去动态的绑定。
   
        <div id="testComponent">
        	 <component v-bind:is="currentView"></component>
        </div>
        
		    var testComponent = new Vue({
		    	el: '#testComponent',
		    	data: {
		    		currentView: 'find'
		    	},
		    	components: {
		    		home: {
		    			template: '<p>我是home</p>'
		    		},
		    		find: {
		    			template: '<p>我是find</p>'
		    		}
		    	}
		    })
   **如果想让切换的模板保留在内存中**那么需要使用keep-alive包裹一下。

### 直接访问子组件
   使用`ref`给子组件添加一个id
   
        <div id="parent">
          <user-profile ref="profile"></user-profile>
        </div>
        var parent = new Vue({ el: '#parent' })
        // 访问子组件
        var child = parent.$refs.profile
   
   