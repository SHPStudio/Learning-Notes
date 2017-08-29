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