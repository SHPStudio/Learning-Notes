# 使用npmscript启动
 可以在package.json中配置scripts的build属性为webpack，可以用`npm run build`就可以进行webpack的编译了
 不需要写很长的命令代码。在scripts里可以引用npm所安装的包的名字比如webpack，而不需要把一个完整的路径写下来。
 
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
    	"build":"webpack"
      },
# 给命令添加参数
 自定义的参数可以通过在参数与命令之间加两个`--`破折号来进行传递。`npm run build -- --colors`
# 全局打包
 推荐把要打包的资源放到一个单独的文件夹下，这样的话，如果别的项目需要用这个模块的资源，那么直接把这个拷贝到
 相应项目中即可，只需要配置相同即可比如加载器之类的。
 
 比如很多模块都有一个共享资源的话，那么使用别名`aliasing`去import将会更好。
# 使用sourcemap追踪开发中的源代码中的错误或警告
  在开发的时候如果打包的脚本中在运行时出现了错误，那么指向的错误位置是打包的脚本，如果在webpack.config.js中配置
  上sourcemap那么他会在出错的时候会把错误的代码位置映射到真正出错的脚本位置。
  `devtool: 'inline-source-map'`
# 可以自动打包 但最好用于开发不要用于生产环境
  如果每次修改依赖模块的代码，都要从新使用npm run build的话太麻烦了，可以使用几种办法，让webpack去监控这些依赖模块，
  当模块修改了，他会自己去编译并且只会编译改变的文件。
1. 使用npm脚本添加一个webpack的watch命令 
    `"watch":"webpack --watch"`
    然后就可以使用`npm run watch`命令开启watch监控模式
    
    他的缺点就是改变编译完成之后得需要手动刷新浏览器才能看到改变。
2. 使用webpack-dev-server
   他支持提供一个简单的服务器去实时的看到所依赖的模块的改变，并且他可以让浏览器获取实时的改变不需要重新刷新。
    1. 首先使用安装webpack-dev-server
    2. 在webpack.config.js中配置`devserver`让devserver知道寻找哪里的文件。


            devServer: {
                       contentBase: './dist'
                    },
    
    3. 在npm脚本中加入`"start": "webpack-dev-server --open",`
    4. 使用npm run start命令启动
3. 可以使用webpack-dev-middleware
   他相当于一个中间件，但是他可以作为一个单独的包，去设置更多的自定义选项，他可以说是
   webpack-dev-server与express服务器的一个结合。
    1. 首先需要下载webpack-dev-middleware
    2. 编写一个服务器脚本 绑定ip和端口

            const express = require('express');
            const webpack = require('webpack');
            const webpackDevMiddleware = require('webpack-dev-middleware');
            
            const app = express();
            const config = require('./webpack.config.js');
            const compiler = webpack(config);
            
            // Tell express to use the webpack-dev-middleware and use the webpack.config.js
            // configuration file as a base.
            app.use(webpackDevMiddleware(compiler, {
              publicPath: config.output.publicPath
            }));
            
            // Serve the files on port 3000.
            app.listen(3000, function () {
              console.log('Example app listening on port 3000!\n');
            });
    
    3. 在配置文件中指定`publicPath`来寻找我们自定义的服务器脚本
    4. 在npm的脚本中写一个启动webpack-dev-middleware的指令
    `"server": "node server.js",`

# 使用HMR热模块替换
  他允许在运行时更新所有类型的模块。
1. 开启HMR
2. 可以在模块代码中捕获模块的更新
3. 通过社区提供的各种loader来处理其他类型模块的更新比如vue-loader
# 使用tree shaking来优化打包文件
  在使用es2015标准的符号import和export时，可能我们所用的入口文件中，只需要引用外部接口文件中的一个方法。
  那么针对所import的模块除了我们用到的接口，其余接口的代码可以称为死代码。所以就可以用tree shaking来把咱们不需要
  的死代码从打包文件中剔除掉，减少bundle文件的大小，这种在大型项目中的效果比较好。
1. 首先是需要使用import和export `import {cube} form './math.js'`
2. 使用`UglifyJSPlugin`插件来去除bundle文件中没有被引用的模块代码。
# 把生产环境配置与开发环境配置分开
 通常生产环境和开发环境要求的目的是不一样的，开发环境通常的目的是一个强壮的源代码映射可以方便的查找错误，还有更好的实时能够捕获改变的文件。
 然而生产环境要求一个比较弱化的源代码映射和一个更小的打包文件提高加载速度。
 
 所以把开发环境的配置和生产环境分开是一个比较好的选择，但是通常他们会有一些通用的配置，比如入口文件
 和输出文件等等。
## 实现
1. 首先建立三个配置文件，通用的，开发环境用的，生产环境用的。
2. 安装webpack-merge工具，为的是可以把通用的配置与特定环境配置进行整合。
3. 编写npm script不同命令执行不同的配置文件

**process.env.NODE_ENV**

  一般会用这个来标识到底是运行在开发环境或者还是生产环境，有些代码库用这个来判断是否添加或删除一些特定环境的模块。
  比如在开发环境中判断这个标识来增加一些方便于打日志或者测试的模块。在线上环境则就添加一些模块来优化用户的体验。比如加快加载速度。
  
        process.env.NODE_ENV === 'production' ? '[name].[hash].bundle.js' : '[name].bundle.js'