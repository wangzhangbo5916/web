
# 函数

## 函数声明和函数表达式的区别

### 1. 提升（Hoisting）

函数声明会被提升，这意味着在代码执行之前，函数声明会被提前到作用域顶部。

```js
// 函数声明
console.log(declaredFunction()); // 输出: "This is a function declaration"
function declaredFunction() {
  return 'This is a function declaration';
}
```

函数表达式不会被提升，必须在定义之后才能使用。

```js
// 函数表达式
console.log(expressionFunction); // 输出: undefined
console.log(expressionFunction()); // TypeError: expressionFunction is not a function

var expressionFunction = function () {
  return 'This is a function expression';
};
```

### 2. 作用域

函数声明的作用域是其所在的函数或全局作用域。
函数表达式的作用域是其定义的位置。

### 3. 匿名和命名

函数表达式可以是匿名的，也可以是命名的。

```js
// 匿名函数表达式
var anonymousFunction = function () {
  return 'This is an anonymous function expression';
};

// 命名函数表达式
var namedFunction = function namedFunctionExpr() {
  return 'This is a named function expression';
};
```

函数声明则必须有名称。

```js
function declaredFunction() {
  return 'This is a function declaration';
}
```

### 4. 可读性和表达

函数声明通常在代码的顶部，这有助于提高可读性，因为它们描述了可用的函数。
函数表达式可以在代码中的任何位置创建，这允许更动态的使用方式，比如作为参数传递给其他函数。

### 5. 作为值使用

函数表达式可以立即执行（IIFE），或者作为其他函数的参数。

```js
// IIFE（立即调用函数表达式）
(function () {
  console.log('This function runs right away');
})();

// 作为参数传递
setTimeout(function () {
  console.log('This function runs after 1 second');
}, 1000);
```

函数声明则不常用于这些场合，通常是定义后多次调用。

## 立即调用函数表达式（IIFE）

### IIFE 使用场景

1. 创建私有作用域
   在 JavaScript 中，变量如果不是在函数内部声明的，那么就会成为全局变量。通过使用 IIFE，可以避免变量污染全局作用域。

   ```js
   (function () {
     var localVar = "I'm private";
     // localVar 在这个函数外部是无法访问的
   })();
   ```

2. 避免命名冲突
   当引入多个库或框架时，可能会出现命名冲突。IIFE 可以帮助隔离各自的变量和函数，防止冲突。

   ```js
   (function ($) {
     // 使用 $ 作为 jQuery 的别名，避免与其他库冲突
     $('#my-element').addClass('highlight');
   })(jQuery);
   ```

3. 模块化代码
   在模块化开发中，IIFE 可以用来模拟模块作用域，封装模块的公共接口和私有变量。

```js
var myModule = (function () {
  var privateVar = "I'm private";

  return {
    publicMethod: function () {
      console.log(privateVar);
    },
  };
})();

myModule.publicMethod(); // 访问公共方法，而不暴露私有变量
```

4. 立即执行初始化代码
   IIFE 可用于立即执行初始化代码，而不必等待事件或者回调。

```js
(function () {
  // 初始化代码
  console.log('Initialization logic executed immediately');
})();
```

### IIFE 注意事项

1. 括号的使用
   IIFE 需要被包围在括号内，以表明它是一个函数表达式，否则会被解释为一个函数声明，从而导致语法错误。

   ```js
   (function() {
     // 代码
   })(); // 正确的 IIFE

   function() {
     // 代码
   }(); // 错误的 IIFE，会导致语法错误
   ```

2. 传递参数
   当调用 IIFE 时，可以传递参数进去。这在需要将外部变量传入 IIFE 时非常有用。

   ```js
   (function (global) {
     // 代码可以访问全局对象，而不直接使用 window 或 global
   })(this);
   ```

3. 返回值
   IIFE 可以有返回值，这个返回值可以被赋值给一个变量。

```js
var result = (function () {
  return 'Hello from IIFE';
})();
console.log(result); // 输出: Hello from IIFE
```

4. 代码风格
   在团队开发中，应该遵守统一的代码风格，关于 IIFE 的使用也不例外。保持一致的风格有助于代码的可读性和维护性。
5. 性能考虑
   虽然 IIFE 提供了很多好处，但在某些性能敏感的场景下应谨慎使用，因为每次 IIFE 的调用都可能产生函数调用的开销。在现代 JavaScript 模块系统（如 ES6 模块）的支持下，IIFE 的使用已经不如以前那么普遍了。

## 箭头函数与普通函数的区别

### 1. 语法简洁

箭头函数提供了更简洁的函数写法。

```js
// 普通函数
function add(a, b) {
  return a + b;
}

// 箭头函数
const add = (a, b) => a + b;
```

### 2. this 绑定

箭头函数不绑定自己的 this，它会捕获其所在上下文的 this 值作为自己的 this 值。

```js
// 普通函数
function Person() {
  this.age = 0;

  setInterval(function growUp() {
    this.age++;
  }, 1000);
}

// 箭头函数
function Person() {
  this.age = 0;

  setInterval(() => {
    this.age++;
  }, 1000);
}
```

在普通函数中，this 是动态绑定的，而箭头函数则会继承父作用域的 this。

### 3. 没有 arguments 对象

箭头函数没有自己的 arguments 对象，不能直接访问参数列表的类数组对象，但可以通过剩余参数 ... 来访问。

```js
// 普通函数
function showArguments() {
  console.log(arguments);
}

// 箭头函数
const showArguments = (...args) => {
  console.log(args);
};
```

### 4. 不能作为构造函数

箭头函数不能使用 new 关键字，因为它没有自己的 this，也没有 prototype 属性，所以不能作为构造函数。

```js
// 普通函数
function Person(name) {
  this.name = name;
}
const person = new Person('John');

// 箭头函数
const Person = (name) => {
  this.name = name;
};
// const person = new Person('John'); // TypeError: Person is not a constructor
```

### 没有 prototype 属性

箭头函数没有 prototype 属性，而普通函数有。

```js
// 普通函数
function myFunction() {}
console.log(myFunction.prototype); // 输出: {constructor: ...}

// 箭头函数
const myArrowFunction = () => {};
console.log(myArrowFunction.prototype); // 输出: undefined
```

### 6. 不能作为 Generator 函数

由于语法设计、this 绑定行为以及 yield 关键字的使用等方面的差异，箭头函数不能用作 Generator 函数。在需要使用 Generator 函数的场景中，应该使用传统的 function\* 语法来定义。

1. 语法差异
   箭头函数是 ES6 引入的一个新的函数语法，它主要用于创建匿名函数，并且拥有更短的函数写法和词法作用域绑定。而 Generator 函数是一种可以暂停执行和恢复执行的函数，它的语法是在 function 关键字后面添加一个星号（\*）来表示。

```js
// 普通函数
function regularFunction() {
  // ...
}

// 箭头函数
const arrowFunction = () => {
  // ...
};

// Generator 函数
function* generatorFunction() {
  // ...
}
```

由于箭头函数的语法设计没有预留位置放置 \* 符号，因此无法通过箭头函数的写法来定义 Generator 函数。

2. this 绑定行为
   箭头函数不绑定自己的 this 值，而是继承自父作用域的 this。Generator 函数，作为一种特殊的函数，经常需要根据函数内部的状态管理来控制 this 值，这种行为与箭头函数的设计初衷相违背。

```js
const object = {
  regularFunction: function* () {
    yield this; // 正确地引用了 object
  },
  arrowFunction: () => {
    // 不能用作 Generator 函数，也不能使用 yield 关键字
  },
};
```

3. yield 关键字的使用
   箭头函数内部不能使用 yield 关键字，因为 yield 是 Generator 函数的专属语法。箭头函数试图使用 yield 会导致语法错误。

```js
const arrowFunction = () => {
  // yield 'value'; // 语法错误，箭头函数内不能使用 yield
};
```

4. 设计哲学
   箭头函数的设计哲学是为了简化函数定义和保留词法作用域的 this 值，而 Generator 函数的设计是为了控制函数的执行过程，两者的目的和使用场景不同。因此，ES6 的设计者没有为箭头函数添加 Generator 函数的能力。

### 箭头函数的 this 来源

头函数的 this 值来源于它所在的上下文环境，它没有自己的 this 绑定。当一个箭头函数被定义时，它的 this 被永久性地捕获了它所在外围作用域的 this 值。这种行为被称为 "Lexical Scoping"。

```js
const obj = {
  method: function () {
    return () => {
      return this; // this 指向 obj
    };
  },
};

const arrowFunction = obj.method();
console.log(arrowFunction() === obj); // 输出: true
```

在上面的例子中，arrowFunction 的 this 被捕获并固定为 obj.method 执行时的 this，即 obj 对象本身。这使得箭头函数在处理事件、定时器、异步操作等场景下非常有用，因为它们通常需要一个不变的 this 上下文


