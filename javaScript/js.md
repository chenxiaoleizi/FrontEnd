### JavaScript数据类型

1. Undefined
2. Null
3. String
4. Number
5. Boolean
6. Object
7. Symbol(es6新增)

### this指向问题

1. 全局作用域中的this永远指向window。
2. 函数作用域中的this。

   - 谁调用这个函数，this就指向谁。
   - 箭头函数中的this和外层作用域中的this指向相同。
3. 构造函数中的this指向实例。
4. 调用call，apply和bind方法的函数，this会指向绑定的上下文。

### 普通函数和箭头函数的区别

1. 写法不同，普通函数需要function关键字，箭头函数则需要=>。

2. 普通函数的this指向由运行时决定，箭头函数的this指向由定义是决定(和外层作用域中的this指向相同)。

3. 由于箭头函数的this在定义时已经确定且不可更改，所以使用call，apply和bind方法是无效的，并不会改变箭头函数内部this的指向。

4. 不可以使用new命令，否侧会报错

   ```javascript
   let func = () => {};
   let a = new func(); // Uncaught TypeError: func is not a constructor
   ```

5. 箭头函数中没有arguments对象

   ```javascript
   let func = () => {
       console.log(arguments)
   };
   func(); // Uncaught ReferenceError: arguments is not defined
   ```

6. 箭头函数没有prototype属性

   ```javascript
   let normalFunc = function(){};
   let arrowFunc = () => {};
   console.log( normalFunc.prototype ); // {constructor: ƒ}
   console.log( normalFunc.arrowFunc ); // undefined
   ```

   

### 如何实现函数的call方法

call方法将改变函数的this指向，思路是利用上面this指向说的谁调用这个函数，this就指向谁。

```javascript
Function.prototype.simCall = function(thisArg, ...args){
    // 由于我们用需要绑定this的函数来调用simCall方法，所以simCall方法中的this指向的就是我们的函数
    thisArg.func = this; 
    let res = thisArg.func(...args);
    delete thisArg.func;
    return res
}
```

### 如何实现函数的apply方法

apply方法原理与call一样，只是传参不同

```javascript
Function.prototype.simCall = function(thisArg, args){ // args是一个数组
    thisArg.func = this; 
    let res = thisArg.func(...args);
    delete thisArg.func;
    return res
}
```

### 如何实现函数的bind方法

bind方法除了会绑定函数的this，还会返回一个新的函数

```javascript
Function.prototype.simBind = function(thisArg, ...args){
    let func = this;
    return function(){
        thisArg.func = this; 
        let res = thisArg.func(...args);
        delete thisArg.func;
        return res
    }
}
```

### javaScript如何实现继承

1. 原型链继承

   ```javascript
   function Super(){ // 父类
       this.arr = ['1', '2', '3']   // 引用类型会被共享
   };
   function Sub(){};   // 子类
   
   Sub.prototype = new Super(); // 将子类构造函数的prototype属性设置为父类的实例
   ```

   有两个方法确定实例和原型的关系

   - instanceof

     ```javascript
     instance instanceof Object  // true
     ```

   - isPrototypeOf

     ```
     Object.prototype.isPrototypeOf(instance) // true
     ```

   缺点  1.父类实例中的引用类型属性会被子类所有实例共享 2. 实例化子类的时候不能向父类的构造函数传参。

2. 借助构造函数继承

   借用构造函数来继承可以解决引用类型的问题，也可以在实例化子类时向父类构造函数传参。

   ```javascript
   function Super(){ // 父类
       this.arr = ['1', '2', '3']
       this.func = function(){} // 每个子类的实例都会有自己的func函数，导致函数无法复用
   };
   function Sub(...args){ // 子类
       // 因为使用了call方法，所以Super构造函数中的this指向的子类的实例
       // 此时也可以向父类构造函数传参
       Super.call(this, ...args)   
   }; 
   // 子类的每个实例都会有一个自己的arr属性和func方法
   let subInstance = new Sub()
   ```

   缺点  属性和方法都定义在父类的构造函数中，每个子类的实例都有自己的func方法，导致func方法无法复用

3. 组合继承

   组合继承指的是结合使用**原型链继承**和**构造函数继承**，发挥两者之长。

   ```javascript
   function Super(){ // 父类
       this.arr = ['1', '2', '3'] // 属性在构造函数中定义
   };
   Super.prototype.func = function(){}; // 方法在原型上定义
   
   function Sub(...args){ // 子类
       // 调用父类构造函数来继承父类构造函数中的属性(第二次调用)
       Super.call(this, ...args)   
   };
   sub.prototype= new Super(); // 用原型链来继承父类原型上的方法(第一次调用)
   
   let subInstance = new Sub()
   ```

   缺点  需要调用两次父类构造函数

4. 原型式继承

   ```javascript
   // 产生一个继承o的对象
   function inherit(o){
       function F(){};
       F.prototype = o;
       return new F()
   }
   
   // ES5提供了方法
   let sub = Object.create(Super);
   ```

5. 寄生式继承

   ```
   // 只是在原型式继承的基础上对新的对象进行增强
   // 产生一个继承o的对象
   function inherit(o){
       function F(){};
       F.prototype = o;
       let obj = new F();
       obj.func = function(){}; // 增强
       return obj
   }
   ```

6. 寄生组合式继承

   ```javascript
   function Super(){ // 父类
       this.arr = ['1', '2', '3']   // 引用类型会被共享
   };
   Super.prototype.func = function(){}; // 方法在原型上定义
   
   function Sub(){ // 子类
       Super.call(this) // 继承父类构造函数中定义的属性
   };   
   
   function inherit(Super, Sub){
        // 以父类的原型为基础创建一个新对象，该对象将作为子类的原型
       let prototype = Oject.create( Super.prototype );
       prototype.constructor = Sub; // 纠正子类原则的constructor指向
       Sub.prototype = prototype; // 重写子类的原型
   }
   ```

7. ES6的继承

   ```javascript
   class Super{}
   Class Sub extends Super{
       constructor(...args){
           super(...args)
       }
   };
   
   ```


### 实现一个防抖函数(debounce)

防抖函数是将时间间隔较短得一组操作归并为一个操作

```javascript
function debounce(func, delay){
    let timer = null;
    return function(){
        timer&&clearTimeout(timer);
        timer = setTimeout(func, delay)
    }
}
```

### 实现一个节流函数

节流函数是让函数以均匀得时间间隔执行

```javascript
function throttle(func, delay){
    let canRun = true
    return function(){
        if(!canRun){return}
        canRun = false;
        setTimeout(function(){
            func();
            canRun = true
        })
    }
}
```

### 数组的扁平化

```javascript
let arr = [1, 2, [3, 4]]

// 暴力递归拆解
function flat(arr){
    let res = [];
    function f(a){
       for(let i=0; i<a.length; i++){
           let item = a[i];
           if( Array.isArray(item) ){
               f(item)
           }else{
               res.push(item)
           }
       } 
    }
    f(arr)
}

// 迭代
function flat(arr){
    let flatArr = [];
    while(arr.length>0){
        let item = arr.shift(); // 拿到数组的第一个item
        if( Array.isArray(item) ){ // 如果是个数组，则将其拆解放到数组的前端
            arr.unshift(...item)
        }else{ // 如果不是数组则直接推到新数组中
            flatArr.push(item)
        }
    }
    return flatArr
}

// 利用数组的toString方法和字符串的split方法
arr.toString().split(",")

// 利用es6数组的flat方法
// flat方法接收一个参数表示要拉平的层数
// 1 默认值，只拉平一层
// num 拉平num层
// Infinity 拉平无限层
let newArr = arr.flat(Infinity)
```

### 改造for循环的代码

```javascript
// 改造代码使其输出0 1 2 3 4
for(var i=0; i<5; i++){
    setTimeout(function(){
        console.log(i)
    }, 1000)
}
// 5 5 5 5 5

// 1. 用let代替for循环中的var
// 2. 给定时器外面包一层自执行函数，并将i传给这个自执行函数(闭包原理)
// 3. 定时器的第三个参数会传递给定时器的回调函数
```

