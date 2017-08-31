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
# code splitting
 代码分离，他的作用是把代码分离成不同的包，还可以决定包的加载顺序和优先级，如果用的好，可以大大减少代码加载速度。
## 实现
1. 一个是用entry设置不同的入口文件。这种是最方便，最直接的。不过他有个问题就是会把模块里的重复的代码给打包。
2. 使用CommonsChunkPlugin工具来把不同包中的共用代码打包到指定的入口文件或者是一个新的入口文件。

        new webpack.optimize.CommonsChunkPlugin({
        +       name: 'common' // Specify the common bundle's name.
        +     })

3. 使用动态import的方式，这种方式是在代码中使用代码的方式动态import（暂时没看懂）

        button.onclick = e => import(/* webpackChunkName: "print" */ './print').then(module => {
        +     var print = module.default;
        +
        +     print();
        +   });
# Caching
  缓存技术，一般用于web服务器上，因为每次用户的请求都会打到某个站点上，浏览器就会捕获这个站点的一些资源，但是如果
  每次都抓取一些没有改变的资源，这是没有意义的，所以把一些不变的资源缓存起来，每次只需要把改变的资源从服务器抓取就可以了。
  
  所以在webpack打包的时候也是遵循这个原则，不重复打包不改变的资源。
1. 第一种办法，利用hash来生成打包文件。配置output的filename加上`[ChunkHash]`，这样每次生成就会根据数据块中的内容来生成一个hash值
    每次改变的时候，这个hash值才会改。
2. 还有可以通过CommonChunkPlugin把不会容易改变的模块打成一个单独的包。
3. 在使用CommonChunkPlugin打包的时候可能会出问题，hash值可能会变
   所以需要另外两个插件来按照不同的方式去生成hash NamedModulesPlugin和 HashedModuleIdsPlugin
# 做第三方代码库
  用webpack还可以用作制作第三方的代码库，并可以上传到npm服务器使用。
  问题就是如果你的模块中引入了第三方模块，如果进行打包会把第三方的包也打包进去这样就会使打包的文件非常大。
  所以就需要使用webpack中的配置文件中`externals`这个属性来指定一些第三方的环境，可以让消费者自行安装。
# 使用别名
  在使用第三库的时候比如Jquery会用一些全局的符号来进行引用比如`$`，webpack可以在配置文件中统一用别名的方式进行配置。
1. 使用resolve，resolve可以配别名还可以配一些方便代码引入的操作，比如在import的时候不需要添加后缀等等。

            resolve: {
                alias: {
                    jquery: "jquery/src/jquery"
                }
            }

2. 还可以使用webpack的插件ProvidePlugin

            new webpack.ProvidePlugin({
              $: 'jquery',
              jQuery: 'jquery'
            })

# 使用环境变量
  可以使用环境变量去在执行的时候传递参数到脚本中，在脚本中可以根据传递过来的参数动态的决定如何进行打包，修改一些配置等等。

        module.exports = {
          plugins: [
            new webpack.optimize.UglifyJsPlugin({
        +      compress: process.env.NODE_ENV === 'production'
            })
          ]
        };
        
        {
          "scripts": {
            "build": "cross-env NODE_ENV=production PLATFORM=web webpack"
          }
        }
    
  **必须先安装cross-env这个包**

# 优化事项
## 使用include决定loader的作用范围
  使用include可以限定loader转换哪个文件夹下的资源，节约查找时间。
    
    {
      test: /\.js$/,
      include: path.resolve(__dirname, "src"),
      loader: "babel-loader"
    }
## 尽量少使用loader/plugins
  尽量减少loader/plugins的使用，因为每次添加这些工具都会消耗一定的启动时间
## resolve
  1. 尽量减少resolve.modules, resolve.extensions, resolve.mainFiles, resolve.descriptionFiles的使用，因为他们会调用文件系统比较耗时。
  2. 如果不使用npm link等把resolve.symlinks: false设置为false
  3. 如果使用的是自定义的解析器，那把resolve.cacheWithContext设置为false，因为他们不属于上下文
## dllplugins
   使用dllplugin移动代码将在单独的编译中有更小的变化，会加快编译速度，不过会增加编译过程中的复杂性。
## 让编译的文件更小
   尽量减少引用第三方库，使用CommonChunkPlugin把不同文件的公共部分统一打包。
## 开发时需注意
1. 保证最小的chunk数据块，因为webpack只会发送更新的数据块到文件系统，像HMR,[name]/[chunkhash]等都是对改变的数据块生效的。

# publicpath
 是标记我们要打包的所有资源在哪个文件夹下