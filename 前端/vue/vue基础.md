# Vue
## 介绍
  他是一个基于自底向上的增量开发框架，他主要着重于视图层，能够非常方便的与第三方库和项目的整合，比如webpack，node.js等等。
  当与`单文件组件`和`Vue生态支持的库`结合时，能够提供复杂的单页应用驱动。
  
  他的主要解决的就是页面数据与后台数据的一个绑定，实现前后端分离。并且它的数据绑定是响应式的，脚本改变相应的标签内容也会修改。
## 指令
  `v-bind` 这种是vue的指令，带有v-前缀,不同的指令会有不同的作用。比如`v-bind:title=message`，他就把span标签的title属性绑定于
  vue对象的data域下的message属性。这样在脚本或者控制台修改vue对象下message的值，那么相应的值也会在标签中体现。
  
        <div id="app-2">
          <span v-bind:title="message">
            鼠标悬停几秒钟查看此处动态绑定的提示信息！
          </span>
        </div>
        var app2 = new Vue({
          el: '#app-2',
          data: {
            message: '页面加载于 ' + new Date().toLocaleString()
          }
        })
## 组件
  我们可以自己定义自己的组件来进行使用，在大型的项目中，把应用程序分成各个组件，这样就比较容易进行管理。
1. 首先利用`Vue.component`去创建一个自定义组件。并且他带有一个简单的html模板，而且可以绑定一个数据属性在这个组件中。
2. 我们new一个vue对象并在对象中定义一些data数据。
3. 把我们的组件中的自定义属性与vue对象的data中的数据进行绑定。这样我们就可以获取数据了。

	    <div id="app-7">
	    	<ol>
				<todo-item
				  v-for="item in groceryList"
				  v-bind:todo="item"
				  v-bin:key="item.id">
				  </todo-item>
		    </ol>
	    </div>
	    
        Vue.component('todo-item', {
          props: ['todo'],
          template: '<li>{{ todo.text }}</li>'
        })
        var app7 = new Vue({
          el: '#app-7',
          data: {
            groceryList: [
              { id: 0, text: '蔬菜' },
              { id: 1, text: '奶酪' },
              { id: 2, text: '随便其他什么人吃的东西' }
            ]
          }
        })
## vue实例
1. data
  vue中的data数据对象，他是响应式的，如果引用了外部对象，那么他将跟外部对象引用相同，改变了vue的data中的数据也就改变了外部引用的数据。
  但是如果添了新的属性，那么他不会获取到，所以，可以事先决定好需要用到哪些属性，先给一个初始值。到以后再改。
  
        var data = { a: 1 }
        var vm = new Vue({
          el: '#example',
          data: data
        })
2. 暴露的实例
  vue暴露了一些实例，这些实例都带有`$`符号
  
        vm.$data === data // => true
        vm.$el === document.getElementById('example') // => true
        // $watch 是一个实例方法
        vm.$watch('a', function (newValue, oldValue) {
          // 这个回调将在 `vm.a` 改变后调用
        })

3. 实例的生命周期
   vue实例会有自己生命周期，并且这些生命周期中比较重要的会暴露相应的声明周期钩子函数。
   比如`create`、`updated`等等。
   **注意**这些钩子函数或者属性监听中不要使用箭头函数，因为箭头函数中的`this`是和父级绑在一起的，可能会被报出未定义。

## 模板语法
  `{{ msg }}`这种插值的形式能够把dom绑定到vue底层的数据上，首先，vue先把模板渲染成一个虚拟的dom，然后在数据改变时，通过响应系统，能够
  以最小的代价重新渲染组件并应用到dom中。
## 使用javascript
  vue可以在插值中使用简单的javascript表达式，不过只能绑定单个表达式。
  
    {{ number + 1 }}
    {{ ok ? 'YES' : 'NO' }}
    {{ message.split('').reverse().join('') }}
    <div v-bind:id="'list-' + id"></div>
## 过滤器
  过滤器是对绑定的参数进行一些过滤，把不符合要求的值都过滤掉。
### 使用
  `v-bind:id="value | filter"` 首先就是在标签上写上一个filter，
  然后在vue实例中实现这个filter
        
        new Vue({
          // ...
          filters: {
            capitalize: function (value) {
              if (!value) return ''
              value = value.toString()
              return value.charAt(0).toUpperCase() + value.slice(1)
            }
          }
        })
  
  **并且filter是可以串联的**
  
  `{{ message | filterA | filterB }}`filterA处理完继续让filterB处理