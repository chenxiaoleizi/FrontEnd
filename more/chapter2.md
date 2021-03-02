### 获取 WebGL 上下文

```javascript
function getContext(canvas) {
    let contextNames = [ "webgl", "experimental-webgl" ];
    let gl;
    for (let i = 0; i < contextNames.length; i++) {
        gl = canvas.getContext( contextNames[i] );
        if (gl) {
            return gl
        }
    }
}
```



### 初始化着色器

##### 创建 shader

```javascript
function createShader(gl, type, source) {
    let shader = gl.createShader( type )
    gl.shaderSource( shader, source )
    gl.compileShader( shader )
    let success = fl.getShaderParameter(shader, gl.COMPILE_STATUS)
    if (success) {
        return shader
    }
}
```

创建 program

```javascript
function createProgram(gl, vertexShaderSource, fragmentShaderSource) {
    let program = gl.createProgram();
    gl.attachShader(program, vertexShaderSource);
    gl.attachShader(program, fragmentShaderSource);
    gl.linkProgram(program);
    let success = gl.getProgramParameter(program, gl.LINK_STATUS)
    if (success) {
        return program
    }
}
```

### 使用色器变量

为了使着色器的数据更加的灵活，我们使用 JavaScript 向着色器传入数据。

首先需要在着色器代码中声明全局变量，一共有三种类型的全局变量

1. attribute 类型，该类型只能出现在顶点着色器中，并且只能是全局变量。
2. uniform 类型，该类型可以出现子顶点着色器和片元着色器中，只能是全局变量。
3. varying 类型，该类型必须是全局变量，任务是从顶点着色器向片元着色器传输数据，必须在顶点着色器和片元着色器声明同类型，同名称的变量。

attribute 类型的变量只有顶点着色器可以使用，保存的是和顶点有关的数据。

给 attribute 变量赋值

```javascript
let vertexShader = `
attribute vec4 a_Position;
void main() {
	gl_Position = a_Position;
}`
let a_Position = gl.getAttribLocation(program, "a_Position")
gl.vertexAttrib3f( a_Position, 1.0, 0.0, 0.0 )
```

给 uniform 变量赋值

```javascript
// 片元着色器没有默认的精度，使用变量是必须手动设置精度 precision mediump float
let fragmentShader = `
precision mediump float;
uniform vec4 u_FragColor;
void main() {
	gl_FragColor = u_FragColor;
}`
let u_FragColor = gl.getUniformLocation(program, "U_FragColor");
gl.uniform4f(u_FragColor, 0.0, 1.0, 0.0, 1.0)
```

使用 varying 变量

```javascript
let vertexShader = `
attribute vec4 a_Position;
varying vec3 v_Position;
void main() {
	gl_Position = a_Position;
}`
let fragmentShader = `
precision mediump float;
uniform vec4 u_FragColor;
varying vec3 v_Position;
void main() {
	gl_FragColor = u_FragColor;
}`
```



### 绘制

gl.drawArrays(mode, first, count) 是一个强大的函数，能够绘制各种图形。该函数接受三个参数

1. mode	指定绘制方式，有如下几个选项
   - gl.POINTS
   - gl.LINES
   - gl.LINE_STRIP
   - gl.LINE_LOOP
   - gl.TRIANGLES
   - gl.TRINGLE_STRIP
   - gl.TRINGLE_FAN
2. first    指定从哪个点开始绘制
3. count    指定绘制需要多少个点

```
gl.drawArrays( gl.POINTS, 0, 1 )
```

### 使用缓冲区对象

缓冲区对象允许我们一次向 attribute 传多个值

```javascript
let vertices = new Float32Array([
    0, 0,
    0, 0.5,
    0.7, 0
])
// 创建 buffer 对象
let verticesBuffer = gl.createBuffer()；

// 绑定缓冲区对象
gl.bindBuffer( gl.ARRAY_BUFFER, verticesBuffer );

// 将数据写入缓冲区对象
gl.bufferData( gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW );

// 将缓冲区对象分配给 attribute
gl.vertexAttribPointer( a_Position, 2, gl.FLOAT, false, 0, 0 );

// 开启 attribute 变量
gl.enableVertexAttribArray( a_Position )
```

