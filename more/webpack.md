### webpack的loader和plugin的区别

​	webpack本身只能识别**JavaScript**和**JSON**文件，想要对其他的文件资源进行打包，我们需要使用loader来对文件进行处理，得到webpack能识别的文件。

​	plugin则是在webpack整个生命周期的运行过程中修改输出结果。比如生成一个html文件（html-webpack-plugin），将css输出到单独的文件（mini-css-extract-plugin）。
