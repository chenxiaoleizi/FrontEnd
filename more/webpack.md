### webpack的loader和plugin的区别

​	webpack本身只能识别**JavaScript**和**JSON**文件，想要对其他的文件资源进行打包，我们需要使用loader来对文件进行处理，得到webpack能识别的文件。

​	plugin则是在webpack整个生命周期的运行过程中修改输出结果。比如生成一个html文件（html-webpack-plugin），将css输出到单独的文件（mini-css-extract-plugin）。

### require方法如何解析路径

当遇到require(x)，会分成以下情况

1. X是**内置模块**
   - 返回内置模块
   - 停止执行
2. X以 **'/'**，'./'，'../' 开头
   - 当成文件加载，找到，返回。（js, json, node）
   - 当成目录加载，找到，返回。（index.js, index.json, index.node）
   - 报错'not found'

### import和require的区别

1. require的是一个值的拷贝，import的是值的引用
2. require是运行时处理，import是编译时处理
3. require是同步加载模块，import是异步加载(独立的模块依赖解析过程)

### webpack 优化

1. 多进程（thread-loader）
2. 并行压缩，terserWebpackPlugin 的 `parallel: true`选项
3. 预编译分包 DLLPlugin
4. 使用缓存，加速二次构建的速度
   - babel-loader 开启缓存  `babel-loader?cacheDirectory=true` （转换的js较少时效果不明显）
   - terser-webpack-plugin 开启缓存 `cache: true`
5. 缩小构建目标，配置 resolve 选项
   - alias  指定常用模块的查找路径
   - extensions  减少数量
   - 
