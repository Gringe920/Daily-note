* `package.json`文件，标准的npm说明文件，里面蕴涵了温服的信息，包括当前项目依赖模块，自定义的脚本任务。一般用`npm init`来自动创建这个文件

* `webpack.config.js`文件，webapck有很多高级的功能，这些功能通过命令行模式实现，但是不太方便而且容易出错，这个文件就是这样而生，把所有与构建相关的信息放在了里面。
	* 在`script`关键字下配置start命令，但是必须说的一点是npm的`start`是一个特殊的脚本名称，它的特殊性表现在，在命令行中使用`npm start`就可以执行相关命令，如果对应的此脚本名称不是start，想要在命令行中运行时，需要这样用`npm run {script name}如npm run build`
	* 在`modules`关键字下配置大名鼎鼎的Loaders，感叹号的作用在于使同一文件能够使用不同类型的loader，配置主要包括：
		* test：一个匹配loaders所处理的文件的拓展名的正则表达式（必须）
		* loader：loader的名称（必须）
		* include/exclude：手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）
		* query：为loaders提供额外的设置选项（可选）

	
