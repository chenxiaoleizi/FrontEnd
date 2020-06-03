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

   