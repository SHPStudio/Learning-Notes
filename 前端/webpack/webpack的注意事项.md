# webpack装载模块问题
webpack没有改动除了import和export之外的代码，因为这两段代码是属于ES2015标准的，
如何依赖的模块中有有关ES2015标准的，需要用Babel loader去转换一下。
# import与require的区别
 import是ES6的规范，然而require是属于开发者自拟的规则，只是被得到广泛利用了而已。属于野生的。
 1. 并且import是强绑定的，require只是值传递或者引用传递。
        // counter.js
            exports.count = 0
            setTimeout(function () {
              console.log('increase count to', exports.count++, 'in counter.js after 500ms')
            }, 500)
            
            // commonjs.js
            const {count} = require('./counter')
            setTimeout(function () {
              console.log('read count after 1000ms in commonjs is', count)
            }, 1000)
            
            //es6.js
            import {count} from './counter'
            setTimeout(function () {
              console.log('read count after 1000ms in es6 is', count)
            }, 1000)
            
            ➜  test node commonjs.js
            increase count to 1 in counter.js after 500ms
            read count after 1000ms in commonjs is 0
            ➜  test babel-node es6.js
            increase count to 1 in counter.js after 500ms
            read count after 1000ms in es6 is 1 
            
# webpack图片打包问题
 1. 在使用css-loader时，webpack会自动识别css中图片的一些引用并最后在处理过的css中替换相应的路径。
 2. 使用html-loader是处理在html标签里引用图片的情况。
# 最好不要在webpack.config.js中加注释