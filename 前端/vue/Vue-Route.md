# 路由
  前端根据相应路径匹配相应的vue组件并且在`route-view` 进行渲染
## 路径路径匹配
  比如要传递参数之类的，需要在路径上进行匹配

        const User = {
          template: '<div>User{{$route.params.id}}</div>'
        }

        const router = new VueRouter({
          routes: [
            // 动态路径参数 以冒号开头
            { path: '/user/:id', component: User }
          ]
        })

  路径上的动态的参数可以通过`$route.params.id`去获得

## 路由复用
  如果使用动态路由不同的动态参数都渲染的是同一个组件的话，那么就可能会有复用的情况
  比如`/user/foo`到`/user/too`，可能不会新生成一个组件实例，这样的话就不会调用组件的生命钩子函数，
  但是可以通过watch`$route`对象来响应路由的变化
## 编程式导航
1. `router.push(location)`

   他会向history的栈中去push一个路径进去，如果用户点击了返回那么就会返回上一个路径记录。
   这个location可以是字符串或者带有动态参数的路由路径。

        // 字符串
        router.push('home')

        // 对象
        router.push({ path: 'home' })

        // 命名的路由
        router.push({ name: 'user', params: { userId: 123 }})

        // 带查询参数，变成 /register?plan=private
        router.push({ path: 'register', query: { plan: 'private' }})

2. `route.replace(location)`
   这个replace跟push很像，但是跟push的区别就是他并不会往history栈中push新的记录，
   他会替换掉当前的history记录。如果想在router-link中实现replace就在link加一个replace的属性。

3. `route.go(n)`
   跟history.go很像，是操作返回和前进记录的。

## 重定向
    重定向的意思就是当`/a`的时候把请求重定向到`/b`，并且还可以使用命名路由规则。

        const router = new VueRouter({
          routes: [
            { path: '/a', redirect: '/b' }
          ]
        })
        const router = new VueRouter({
          routes: [
            { path: '/a', redirect: { name: 'foo' }}
          ]
        })
        const router = new VueRouter({
          routes: [
            { path: '/a', redirect: to => {
              // 方法接收 目标路由 作为参数
              // return 重定向的 字符串路径/路径对象
            }}
          ]
        })

## 别名
   别名代表你想访问`/a`，如果配了别名`/b`，在访问`/b`时，虽然路由路径仍然是`/b`，但是用到的组件还是a的。

        const router = new VueRouter({
          routes: [
            { path: '/a', component: A, alias: '/b' }
          ]
        })
