## webpack配置文件 webpack.config.js
### 介绍
  webpack运行的时候，除了用命令行的方式去指定参数外，还可以使用webpack.config.js来配置
### 流程
1. 首先配置package.json 如果没有可以使用`npm init`来生成。这个是用于指定依赖包版本之类的管理用的，在利用npm install安装的时候他会默认搜索package文件，按照
package里的一些配置，比如包的版本等等来进行安装。
2. 然后运行npm install进行安装
3. 最后添加webpack.config.js的配置文件配置相应项
4. 执行webpack打包


    var webpack = require('webpack')
        module.exports = {
          entry: './entry.js',
          output: {
            path: __dirname,
            filename: 'bundle.js'
          },
          module: {
            loaders: [
              {test: /\.css$/, loader: 'style-loader!css-loader'}
            ]
          }
        }