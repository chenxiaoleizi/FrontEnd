### WebGL 是什么

canvas 提供了两种上下文，第一种是 2D 上下文，可以让我们在 canvas 上绘制矩形、弧形、路径、文本和图像，所有的操作都是处在一个二维空间中。第二种是 3D 上下文，就是我们所说的 WebGL。相对于 2D 上下文，WebGL 提供的是一个三维空间，API也更加丰富，功能更加强大。



### 如何获取 WebGL 上下文

和获取 2D 上下文一样，首先获取 canvas 元素，然后调用 canvas 的 getContext 方法，只是传入的参数不同。获取 WebGL 传出的标准参数是 `webgl`，但有些浏览器是 `experimental-webgl`。

```javascript
// 获取 canvas
let canvas = document.querySelector("#canvas")

// 获取 WebGL 上下文
let gl;
let contextNames = [ "webgl", "experimental-webgl" ];
for (let i = 0; i < contextNames.length; i++) {
    let contextName = contextNames[i];
    gl = canvas.getContext( contextName );
    if (gl) break
}
```



### WebGL 程序的组成

WebGL 程序由 **JavaScript 代码**和**着色器（shader）代码**两部分组成。

JavaScript 代码一是用来控制绘制，二是提供绘制所需的位置、颜色和一些自定义信息。

着色器代码则是执行真正的绘制。着色器代码又由**顶点着色器**代码和**片元着色器**代码组成。顶点着色器决定点的位置，片元着色器决定像素的颜色，两者缺一不可。着色器代码会以字符串的形式呈现，最后插入到 JavaScript 代码中。



### WebGL 坐标系

WebGL 采用笛卡尔坐标系，坐标系的原点处在 canvas 的中心点。水平向右为 X 轴的正方向，垂直向上为 Y 轴的正方向，垂直于屏幕向外为 Z 轴的正方向。坐标的范围是 (-1, 1)。

示意图



### 着色器

着色器代码是使用着色器语言(GLSL)写的，GLSL 是一种语法类似于 C/C++ 的强类型语言。

#### 顶点着色器

顶点着色器的作用时计算每个顶点的坐标，每个顶点坐标的计算都要执行一次顶点着色器的代码

```glsl
void main() {
    gl_Position = vertexPosition;
}
```

#### 片元着色器片

片元着色器的作用是计算当前像素的颜色，每个像素颜色的计算都要执行一次片元着色器代码

```glsl
void main() {
    gl_FragColor = pixelColor;
}
```

`gl_Position` 和 `gl_FragColor` 是 GLSL 内置的两个变量，分别代表顶点的位置和像素的颜色。

#### 着色器代码的位置 

WebGL 中着色器代码是以字符串的方式呈现的，有三种书写方法。

1. 用字符串拼接

   ```javascript
   let vertexShader = 
       "void main() {\n" +
       "    gl_Position = vertexPosition;\n" +
       "}\n"
   let fragmentShader = 
       "void main() {\n" +
       "    gl_FragColor = pixelColor;\n" +
       "}\n"
   ```

2. 写在 script 标签中

   ```html
   <script type="x-shader/x-vertex" id="vertexShader">
       void main() {
       	gl_Position = vertexShader;
       }
   </script>
   <script type="x-shader/x-fragment" id="fragmentShader">
       void main() {
       	gl_Position = pixelColor;
       }
   </script>
   <script>
       let vertexShader = document.getElementById("vertexShader").textContent;
       let fragmentShader = document.getElementById("fragmentShader").textContent;
   </script>
   ```

   script 标签的 type 属性是必要的，目的是避免标签中的内容被当成 js 执行。

3. 使用模板字符串

   ```javascript
   let vertexShader = `
   void main() {
   	gl_Position = vertexShader;
   }`
   let fragmentShader = `
   void main() {
   	gl_FragColor = pixelColor;
   }`
   ```

第三种方法是最推荐的，直接写在模板字符串中，不需要用加号拼接，也不需要先拿到 dom 的引用。

### 一个简单的示例代码

下面的是一个最简单的示例，设置清空画布的颜色并用该颜色清空画布。

```javascript
let canvas = document.querySelector("#canvas")

// 获取 WebGL 上下文
let gl;
let contextNames = [ "webgl", "experimental-webgl" ];
for (let i = 0; i < contextNames.length; i++) {
    let contextName = contextNames[i];
    gl = canvas.getContext( contextName );
    if (gl) break
}

gl.clearColor(0, 0, 0, 1) // 四个参数分别代表 R, G, B, A
gl.clear() // canvas 被用黑色清空
```

