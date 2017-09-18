## Vue.nextTick( [callback, context] )
  这个api是在修改了相关vue的实例的值或者其他操作导致dom更新的时候会触发回调函数。并获取更新的dom对象。
## Vue.set(target,key,value)
  设置对象属性，主要解决一些响应式对象，在修改对象属性后或者添加对象属性，需要视图同步的更新。
## Vue.directive(id,[definaition])
  自定义指令，可以自己定义自己的指令，并且在相应的生命周期中进行一些操作。
## Vue.complile(template)
  用于render函数用的编译的字符串模板。**只在独立构建时有效**

    var res = Vue.compile('<div><span>{{ msg }}</span></div>')
    new Vue({
      data: {
        msg: 'hello'
      },
      render: res.render,
      staticRenderFns: res.staticRenderFns
    })

## data
   Vue的实例数据对象，可以使用`vm.$data.a`，访问原始数据对象，但是vue实例可以进行代理访问`vm.a`，
   所以以`$`或者`_`开头的属性不会被vue代理，因为防止与vue内置属性和api方法冲突。

   **注意data不应该使用箭头函数，因为箭头函数中的this并不会指向vue实例**

## props
   vue的属性，可以是数组或者对象，用于接收来自父组件的数据，并且是对象时，可以对属性配置高级选项，类型检测、自定义校检等等。

        // 简单语法
        Vue.component('props-demo-simple', {
          props: ['size', 'myMessage']
        })
        // 对象语法，提供校验
        Vue.component('props-demo-advanced', {
          props: {
            // 只检测类型
            height: Number,
            // 检测类型 + 其他验证
            age: {
              type: Number,
              default: 0,
              required: true,
              validator: function (value) {
                return value >= 0
              }
            }
          }
        })

## propsData
   props对应的数据，**只能用于new创建的实例中**,创建实例时传递props的值，主要用于测试。

        var Comp = Vue.extend({
          props: ['msg'],
          template: '<div>{{ msg }}</div>'
        })
        var vm = new Comp({
          propsData: {
            msg: 'hello'
          }
        })

## $ref
   除了使用props和event来进行父子组价的沟通，还可以使用`$ref`来标记子组件，并在父组件实例中使用`parent.$ref.xxx`来直接访问子组件实例。
## vm.$nextTick( [callback] )
   可以让回调函数在下一次更新dom时调用。

        new Vue({
          // ...
          methods: {
            // ...
            example: function () {
              // 修改数据
              this.message = 'changed'
              // DOM 还没有更新
              this.$nextTick(function () {
                // DOM 现在更新了
                // `this` 绑定到当前实例
                this.doSomethingElse()
              })
            }
          }
        })

## key
  组件的key属性相当于标识组件的唯一性，是用于vue的虚拟dom更好的对组件进行更好的管理，比如清理一些没有key的vue组件实例。还有就是根据key可以复用一些组件。