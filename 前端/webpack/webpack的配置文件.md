# webpack配置文件 webpack.config.js
## 介绍
  webpack运行的时候，除了用命令行的方式去指定参数外，还可以使用webpack.config.js来配置
## 流程
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
## 打包资源配置
### 配置loaders
  不同的资源需要在webpack.config.js中配置不同的loader去转换，并且，最好使用正则表达式去匹配一类文件绑定到相应的loader上。

     module: {
    +     rules: [
    +       {
    +         test: /\.css$/,
    +         use: [
    +           'style-loader',
    +           'css-loader'
    +         ]
    +       }
    +     ]
    +   }
1. 图片资源使用file-loader，正则表达式写法`test: /\.(png|svg|jpg|gif)$/`
2. font可以用file-loader打包，正则表达式`/\.(woff|woff2|eot|ttf|otf)$/`
3. 关于一些其他资源的打包，json文件默认支持，如果要打包xml、csv等文件需要相应的loader去转换，`csv-loader` and `xml-loader`.
### 输出文件配置
  之前都是手动在html文件中配置输出的打包文件，并且在webpack.coonfig.js配置文件中手动的去指定入口文件和响应的输出文件。这样就很麻烦
  那么webpack可以通过改变配置文件中的entry入口配置，把想要分开打包的模块文件按照键值对的形式配置，在output属性中配置输出的文件可以按照
  entry的key值来进行生成，这样就不用手动的去配相应的输出文件了。
    
     module.exports = {
        entry: {
    -     index: './src/index.js',
    +     app: './src/index.js',
    +     print: './src/print.js'
        },
        output: {
    -     filename: 'bundle.js',
    +     filename: '[name].bundle.js',
          path: path.resolve(__dirname, 'dist')
        }
      };
  
  那么如果修改了入口文件名字或者想要新增入口文件，需要更改html页面中所引用的输出文件时，这种情况可以通过插件`HtmlWebpackPlugin`来解决。


