1. 入口

2. 出口

3. 配置loader

   - css-loader
   - 

4. 生成html页面(HtmlWebpackPlugin)

5. 清理目标文件夹(CleanWebpackPlugin)

6. 使用sourceMap

   [devtool选项](https://webpack.js.org/configuration/devtool/)

   ```javascript
   module.exports = {
       devtool: "inlinbe-source-map"
   }
   ```

7. 代码分离(code-split)

   - 配置多入口，但是公用的模块会在两个包中都出现。可以使用entry的shared属性来防止公用模块的重复打包

   - 使用SplitChunksPlugin，配置optimization的splitChunks即可。

     ```javascript
     module.exports ={
         optimization: {
             splitChunks: {}
         }
     }
     ```

8. 动态导入(dynamic imports)

9. 缓存

10. 压缩js文件

    - TerserWebpackPlugin。webpack5.0和以上不需要再手动安装此插件，只需要引入，然后在配置中的optimization选项中配置即可

      ```javascript
      const TerserPlugin = require("terser-webpack-plugin")
      module.exports = {
          optimization: {
              minimize: true,
              minimizer: [
                  new TerserPlugin({
                      include: ""
                  })
              ]
          }
      }
      ```

11. 分离CSS到单独的文件(MiniCssExtractPlugin)

12. 压缩CSS文件(css-minimizer-webpack-plugin)