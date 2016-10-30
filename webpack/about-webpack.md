
# Webpack

`Webpack`基于`node.js`,首先官网安装`node.js`(Windows建议安装32位)

##安装环境

```
npm i webpack -g // 安装Webpack到全局
npm init  //初始化新项目
npm install //安装package.json声明的依赖文件
npm install //安装依赖
npm install --save //安装依赖并写入package.json的dependencies
npm install --save-dev //安装依赖并写入package.json的devDependencies(devDependencies下列出的模块，是我们开发时用的，比如第三方压缩插件，我们用它混淆js文件，它们不会被部署到生产环境。dependencies下的模块，则是我们生产环境中需要的依赖)

//其他命令--更新,卸载
npm update
npm uninstall
```

## package.json的scripts命令配置

```Json
{
"build": "set NODE_ENV={\"deploy\":false,\"clean\":true,\"name\":\"charge\",\"extract\":false}&&webpack",
"deploy": "set NODE_ENV={\"deploy\":true,\"clean\":true,\"name\":\"charge\",\"extract\":false"}&&webpack -p --progress --colors"
}
```

`set NODE_ENV={xxx}`(OS X下为`export`命令)

设置`node`的环境变量(`webpack`配置中可以读取并配置相关的打包选项),需要注意这里的`NODE_ENV`为严格模式的`jsonstring`,想要在`webpack`中获取需要进行相应的类型转换,当然也可以为普通的字符串,例如`deploy`.这里只是为了方便同时指定打包的目录

`webpack`普通打包模式

`-p`压缩打包模式

`--progress/--colors`指编译打包时实时显示进度条/颜色

others: 也可以在这里自己写命令去执行一些文件操作


## webpack.config.js配置


`var path = require('path');` node的原生模块,指向当前项目地址

`var PAGES_PATH = path.resolve(__dirname, './pages');`声明项目地址

`config.entry`打包的文件入口,支持多个

`config.output`打包的文件输出,其中`webpack`变量`hash`为本次打包的`hash`,`chunkhash`为该模块的`hash`

`path`指定一个输出目录

`chunkFilename` 指定图片,异步的资源名称,否则会输出类似`1-2.[hash].js`这样的怪异名称

`publicPath`文件发布的地址,可以是`cdn`(例如七牛,可是`Webpack`的解决方案都是全部文件发布到一个地址,还不能把`css`,`js`文件单独发布到本地,暂不考虑),我们选择本地目录

`config.externals `声明外部依赖--譬如`externals: {'jquery': '$'}`(即使项目中引入了已经声明的外部依赖,`webpack`就不会把文件再打包)

`config.resolve.extensions`声明`node`编译打包时搜索的文件后缀


##配置loaders
`config.module.loaders`样式`loader`根据`package.json`的命令配置去设置是否提取样式到独立的文件

```
.nodeEnvInfo.extract ? ExtractTextPlugin.extract('style-loader', 'css-loader!sass-loader') : 'style!css!sass'
```
`url loader` 根据实际情况去设定`limit`值
`'url?limit=4096'`

`babel loader`根据语法设置是否采用`es6` 
`query: {presets: ['es2015', 'stage-0', 'react']}`

##配置plugins
`config.module.plugins` 

`webpack`内置的`DefinePlugin`,根据编辑环境去设置是否开启`debug`,注意 `__DEBUG__ && console.log(xxx)`; 这样的代码被编译成 `(false) && console.log(xxx)`;之后再用`uglify`插件压缩过后这一段代码是会直接被删除的
`__DEBUG__: nodeEnvInfo.deploy ? false : true`
   
`webpack-md5-hash`,当设置提取样式文件的时候,`css/js`的修改都会影响到彼此的`hash`值,使用这个插件可以有效避免这种情况,有利于缓存

`assets-and-replace-webpack-plugin`,   
1. 支持读写`jsp/html`文件(写入原`chunsname.js`中的带最新`chunkhash`的`js`文件`--chunksname.js`仍然保留,用于对比),插件已对外发布到`npmjs`;  
2. 各个`jsp`都新建了一份带`.dev`名的文件(仅操作此文件),把对应的文件的`publishPath`打包到不带`.dev`的服务器最终读取的`jsp`文件中

`clean-webpack-plugin`每一次打包,`webpack`默认都不会操作旧文件,用这个插件可以实现清理文件夹的功能,避免文件过多
```
root: 'D:\\workspace\\duotao\\Server\\code\\duobao_foreign\\treasure_service'
```
`webpack.optimize.UglifyJsPlugin` 使用`UglifyJs`协议压缩。

`webpack.optimize.OccurenceOrderPlugin`按引用频度来排序 `ID`，以便达到减少文件大小的效果。

`extract-text-webpack-plugin`是否提取(样式)文件。
