### 盒子模型

盒子模型是指将页面上的一个元素看成margin, border, padding和content组成的矩形。

盒子模型分为两种，由box-sizing属性控制

1. `box-sizing: content-box;`  设置元素的宽高属性`width: 100px; height: 100px`就是设置盒子的**content**宽高
2. `box-sizing: border-box;`  设置元素的宽高属性`width: 100px; height: 100px`就是设置**content+padding+border**的宽高

[^思考]: 设计者为什么会想到盒子模型，我觉得可能是因为考虑到计算每个元素的具体位置（浏览器的渲染页面过程的布局阶段会计算元素的具体位置）。而图形或者几何学中有一种叫bounding box的计算方法，就是用一个最小矩形来包围需要计算的物体，在计算物体的位置或者是否碰撞时就以物体的包围盒为基准，忽略物体的不规则的部分，从而简化了计算。



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

   ### flex布局

   #### 父级属性

   - display

     设置父级为flex布局盒子

     ```css
     parent: {
         display: flex;
     }
     ```

   - flex-direction

     主轴方向，即设置子集的排列方向

     ```css
     parent: {
         flex-direction: row;            /* 主轴为水平方向，从左向右排列(→) */
         flex-direction: row-reverse;    /* 主轴为水平方向，从右向左排列(←) */
         flex-direction: column;         /* 主轴为垂直方向，从上向下排列(↓) */
         flex-direction: column-reverse; /* 主轴为垂直方向，从下向上排列(↑) */
     }
     ```

   - flex-wrap

     定义子集再一行排不下时是否换行

     ```css
     parent: {
         flex-wrap: nowrap;       /* 不换行(默认) */
         flex-wrap: wrap;         /* 换行 */
         flex-wrap: wrap-reverse; /* 换行，但是从下往上排列 */
     }
     ```

   - flex-flow

     flex-direction和flex-wrap的简写

     ```css
     .parent{
         flex-flow: <flex-direction> <flex-wrap>;
     }
     ```

   - justify-content

     子集在主轴方向上的排列方式

     ```css
     .parent{
         justify-content: flex-start;    /* 紧贴主轴的起点依次排列 */
         justify-content: flex-end;      /* 紧贴主轴的终点依次排列 */
         justify-content: center;        /* 剧中排列 */
         justify-content: space-between; /* 紧贴主轴的两端均匀分布 */
         justify-content: space-around;  /* 子集两边的间隔相等 */
     }
     ```

   - align-items

     子集在交叉轴方向上的对齐方式

     ```css
     .parent{
         align-items: flex-start;  /* 和交叉轴的起点对齐 */
         align-items: flex-end;    /* 和交叉轴的终点对齐 */
         align-items: center;      /* 和交叉轴的中点对齐 */
         align-items: baseline;    /* 和第一行文字的基线对齐 */
         align-items: stretch;     /* 子集未设置高度的话，会被拉成和父级的高度一样 */
     }
     ```

   - align-content

     多根轴线的对齐方式

     ```css
     .parent{
         align-content: flex-start;    /* 和交叉轴的起点对齐 */
         align-content: flex-end;      /* 和交叉轴的终点对齐 */
         align-content: center;        /* 和交叉轴的中点对齐 */
         align-content: space-between; /* 和交叉轴两端对齐 */
         align-content: space-around;  /* 轴线两侧的间隔相等 */
         align-content: stretch;       /* 轴线占满整个父级的高度 */
     }
     ```

     #### 子集属性

     - order

       子集的排列顺序，数值越小排列越靠前。

     - flex-grow

       子集放大的比例。用当前子集的flew-grow除以所有子集的flex-grow之和就是当前子集的放大比例。默认为0。

     - flex-shrink

       子集的缩小比例，用当前子集的flew-shrink除以所有子集的flex-shrink之和就是当前子集的缩小比例。默认为0。

     - flex-basis

       定义在分配多余空间之前，子集占据的主轴空间，默认值为auto，可以向width和height一样设置固定值。

     - flex

       flex-grow，flex-shrink和flex-basis的简写

       ```css
       .child{
           flex: <flex-grow> <flex-shrink> <flex-basis>;
       }
       ```

     - align-self

       定义子集的对齐方式，会覆盖父级的align-items属性



### flex-shrink 计算规则

```javascript
A = 宽度*flexShrink + 宽度1*flexShrink1 + 宽度2*flexShrink2
B = 宽度*flexShrink / A
减小的宽度 = 宽度 - B*(子集总宽度 - 父级宽度)
```



### 使用CSS创建三角形

​	原理是使用border来模拟三角形

​	我们先给四个边的border不同的颜色，看一下border到底是什么样。

```css
.varied-border {
    width: 100px;
    height: 100px;
    border-color: orange skyblue red green;
    border-width: 50px;
    border-style: solid;
}

```

![](https://github.com/chenxiaoleizi/FrontEnd/blob/master/image/image-20200929201438090.png)

再将元素的宽高设为0

```css
.varied-border {
    width: 0;
    height: 0;
    border-color: orange skyblue red green;
    border-width: 50px;
    border-style: solid;
}
```

![](https://github.com/chenxiaoleizi/FrontEnd/blob/master/image/image-20200929202629753.png)

可以发现每个边单独的border就是一个三角形，如果我们想要一个向上的红色三角形，只要把左上右三边的border颜色设为transparent就行了，其方向类推。

```css
.triangle-up {
    width: 0;
    height: 0;
    border-color: transparent transparent red transparent;
    border-width: 50px;
    border-style: solid;
}
```

### 等宽高比

```css
.box {
    width: 100px;
    background: skyblue;
}
.box::before {
    content: "";
    float: left;
    padding-top: 100%;
}
.box::after {
    content: "";
    display: block;
    clear: both;
}
```

