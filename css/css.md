### 清除浮动

子集设置浮动的话会导致父级的高度崩塌。

1. 给父级设置高度(直接粗暴)。

2. 给父级添加子元素并设置子元素clear: both;

3. 给父级设置如下属性

   ```css
   .parent{
       overflow: auto;
       overflow: hidden;
       overflow: scroll;
       
       position: absolute;
       position: fixed;
       
       display: inline-block;
       display: flex;
   }
   ```

   [^技巧]: 方法2我们可以使用伪元素为父级添加子元素

   