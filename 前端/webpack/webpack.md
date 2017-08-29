# webpack
## 简介
  webpack是一个模块打包器，他根据模块之间的依赖进行静态分析，然后按照制定规则生成相应的静态资源。他需要node.js的支持需要版本为0.6以上。
## 特点
1. 他有多种组织模块依赖的方式，同步和异步。异步依赖作为分割点，在优化依赖树后，每个异步区块都作为一个文件被打包。
2. webpack本身只可以处理原生的javascript模块，但是loader转换器可以将任何资源类型转换为javascript模块。
3. 他还有丰富的插件系统，大多数功能都是基于插件系统运行的，还可以使用各种开源的webpack插件，满足各种要求。
4. 使用异步I/O和多级缓存提高运行效率，这使得webpack能够以非常快的速度增量编译。
## 安装
1. 全局安装 npm install webpack -g 这种在任何地方都可以使用
2. 本地安装 
    1. 首先进入我们的项目目录
    2. 创建package.json
    3. 安装webpack npm install webpack --save-dev
    4. 如果需要使用webpack开发工具那么需要另外安装开发工具 npm install webpack-dev-server --save-dev
## 使用
1. 首先保证已经全局和本地项目中都已经安装webpack
2. 然后在项目目录下执行命令webpack `你要打包的文件`也可以说是入口文件 `生成的打包文件`比如bundle.js
3. 在你项目文件中引用bundle.js文件即可
### 分析依赖
1. 可以建立另外一个模块文件 module.js

    `module.exports="It works from module.js"`
2. 在入口文件中加入该模块的引入 通过`require(./module.js)`的方式
3. 重新打包。

    可以看到webpack能够自动分析出文件中各个模块依赖关系，把依赖的模块也一块打包。webpack会把所有的模块都分配一个唯一的id，通过id类似于索引搜索模块和访问。
    首先webpack会先执行入口文件中的代码，其他模块会在`require`的时候才会去执行。
### loader
1. loader是以链式执行的，每个loader都可以把资源往下传，直到最后一个loader返回javascript。
2. 可以同步或异步执行。
3. 可以通过正则表达式绑定相应类型的文件。
4. 因为他是在node.js环境中运行的所以他可以干任何事情。
5. 他可以通过接收参数来对loader进行配置
6. 他还可以通过npm安装和发布
    
    命名规范：他会以一定的规则优先级进行搜索，规则在webpack 的 resolveLoader.moduleTemplates api 中定义的
   
        ["*-webpack-loader", "*-web-loader", "*-loader", "*"]
#### loader的使用
1. 首先需要先安装相应的loader
2. 使用require去引用相应的loader并且绑定相应的文件
    1. 在入口文件中加入`require("!style-loader!css-loader!./style.css") `
    2. 或者在打包的时候先引入loader这样在文件中require的时候只需要写文件路径就可以了。
     `$ webpack entry.js bundle.js --module-bind "css=style-loader!css-loader"`
 ## 开发环境
 1. 当项目比较大构建时间比较长的话，可以在webpack的时候加入一定的参数可以显示进度条等`$ webpack --progress --colors`
 2. 如果不想每次修改模块后都要进行编译，那么可以使用watch监听模式。这样每次改动模块的时候
 没有改变的模块会缓存到内存中，不会每次都重新编译，所以编译的速度还是很快的。`$ webpack --progress --colors --watch`
 **当然可以使用webpack-dev-server开发服务工具是一个更好的选择，他会启动一个8080的express静态web服务器，会以监听模式自动运行可以打开localhost:8080或者localhost:8080/webpack-dev-sever查看
 项目中的页面和浏览编译后的资源，并且他是基于socket.io进行实时监控，自动刷新页面**

    
    $ npm install webpack-dev-server -g
    $ webpack-dev-server --progress --colors
    
## 故障处理
 如果webpack的过程中出现了问题想要查看详细的错误信息，那么就可以通过`webpack --display-error-details`来查看
 
 当webpack出现依赖包找不到的情况的话可以使用resolve和resolveLoader设置解析的细节。resolve是有关要被打包的模块，resolveLoader是配置loader模块的解析。
 当出现 Node.js 模块依赖查找失败的时候，可以尝试设置 resolve.fallback 和 resolveLoader.fallback 来解决问题。
 

    module.exports = {
      resolve: { fallback: path.join(__dirname, "node_modules") },
      resolveLoader: { fallback: path.join(__dirname, "node_modules") }
    };

## 路径处理
  当webpack的配置文件中牵扯路径配置的时候，最好使用绝对路径，建议通过`path.resolve(__dirname, "app/folder")`或者`path.join(__dirname,"app","folder")`,并且这种方法是兼容windows的。