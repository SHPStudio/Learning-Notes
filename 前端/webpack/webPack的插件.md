# plugins
## 介绍
  插件可以实现loader实现不了的功能，他可以从npm上安装一些第三方的插件。也可以使用webpack内置的一些插件。
## 流程
1. 如果需要下载第三方的插件，那么首先得需要npm install第三方插件，插件可以配置在package.json中
2. 在webpack.config.js中配置插件即可
## 示例
  通过webpack自带插件实现在生成的文件中添加开头注释的功能

    var webpack=require("webpack")
    module.exports={
    	entry:'./testWebpack.js',
    	output:{
    		path:__dirname,
    		filename:'bundle.js'
    	},
    	module:{
    		loaders:[
    		   {test:/\.css$/,loader:'style-loader!css-loader'}
    		]
    	},
    	plugins:[
    		new webpack.BannerPlugin('This file is created by shape')
    	]
    }
    
## HtmlWebpackPlugin
### 介绍
  这个插件可以根据webpack.config.js中的entry入口文件和output中根据entry中文件列表的key值等相关规则生成的打包文件
  去生成一个index.html文件并自动引入所生成的打包文件。并且可以自定义这个模板。
### 配置

    module.exports = {
        entry: {
          app: './src/index.js',
          print: './src/print.js'
        },
    +   plugins: [
    +     new HtmlWebpackPlugin({
    +       title: 'Output Management'
    +     })
    +   ],
        output: {
          filename: '[name].bundle.js',
          path: path.resolve(__dirname, 'dist')
        }
## clean-webpack-plugin
### 介绍
  这个插件的功能是在每次build的时候可能在dist文件夹中追加新生成的文件，可能还有一些不需要用的文件。
  那么他可以在每次build的时候把特定文件夹清空，只留下需要用的打包的文件。
### 使用
  `new CleanWebpackPlugin(['dist'])`
## WebpackManifestPlugin
 如果想看webpack是如何管理依赖包之间的关系，知道如何打包所需要的模块，这个插件可以把依赖包与输出的打包文件
 的映射关系存储到一个清单文件中，并且这个清单文件的格式可以是json格式，方便使用。