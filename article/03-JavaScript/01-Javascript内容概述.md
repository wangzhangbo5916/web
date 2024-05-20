# Javascript 内容概述

## 基本概念和语法

- 变量和数据类型: 使用 var、let、const 声明变量；了解基本数据类型（字符串、数字、布尔值等）和复合数据类型（对象、数组）。
- 操作符: 算术、逻辑、比较、位运算、赋值、字符串拼接等。
- 控制结构: if-else、switch、for、while、do-while、for-in、for-of。
- 函数: 函数声明、函数表达式、箭头函数、回调函数、立即执行函数表达式（IIFE）。
- 作用域和闭包: 理解全局作用域、局部作用域、块级作用域以及闭包的概念。
- 对象和原型: 对象字面量、构造函数、原型链、继承、Object 类的方法。
- 数组和迭代方法: 数组的创建和操作，以及 map、filter、reduce 等迭代方法。
- 字符串和正则表达式: 字符串处理方法和正则表达式的使用。
- 错误处理: try-catch 语句、throw 关键字、Error 对象。

## 高级特性

- 异步编程: 回调函数、Promises、async/await、事件循环。
- 模块化: CommonJS、ES6 Modules、动态导入。
- 事件处理: 事件监听、事件委托、自定义事件。
- 内存管理: 垃圾回收机制、内存泄漏问题。
- 模板字符串: 使用反引号(`)进行字符串插值和多行字符串。

## DOM 和浏览器 API

- DOM 操作: 查询和修改 DOM 元素、事件处理、创建和删除节点。
- BOM: 浏览器对象模型，包括 window、navigator、location、history、screen 对象。
- AJAX 和网络请求: XMLHttpRequest 对象、Fetch API。
- 本地存储: localStorage、sessionStorage、IndexedDB。
- 文件和流处理: File API、Blob、FileReader、Streams API。

1. ES6 (ECMAScript 2015) 新特性
let 和 const

let 允许你声明一个作用域限制在块级中的变量。
const 允许你声明一个只读的常量。
箭头函数

提供了一种更简洁的方式来写函数表达式。
模板字符串

使用反引号(``)来创建含有嵌入表达式的字符串。
默认参数

函数参数可以有默认值。
解构赋值

一种特殊的语法，允许我们“解开”数组或对象，并将其元素或属性分配给一系列变量。
扩展运算符（...）

允许一个表达式在某处展开数组或字符串，或者展开对象的属性。
Promise 和异步编程

Promise 是异步编程的一种解决方案，让你可以更优雅地处理异步操作。
类（Class）

引入了基于类的面向对象编程。
模块化

支持 import 和 export 语句，可以创建模块并导入其他模块。
新的内置方法

如 Array.prototype.find、 Array.prototype.includes 等。
新的数据结构

如 Map、Set、WeakMap 和 WeakSet。
Symbol

一种全新的原始数据类型，表示独一无二的值。
迭代器（Iterator）和生成器（Generator）

引入了迭代器协议和生成器函数，可以自定义迭代行为。
Proxy 和 Reflect

Proxy 用于创建一个对象的代理，从而实现基本操作的拦截和自定义。
Reflect 是一个内置的对象，它提供拦截 JavaScript 操作的方法。
2. ES2016 (ECMAScript 7) 新特性
Array.prototype.includes

用来判断一个数组是否包含一个指定的值。
指数运算符（）**

新的运算符用于计算幂运算。
3. ES2017 (ECMAScript 8) 新特性
Async/Await

用于简化使用 Promise 的异步操作。
Object.values/Object.entries

返回一个对象自身的所有可枚举属性值的数组/Object.entries()返回一个数组，其元素是与直接在object上找到的可枚举属性键值对相对应的数组。
String padding

String.prototype.padStart 和 String.prototype.padEnd 允许你将空字符串或其他字符串添加到原始字符串的开头或结尾。
Object.getOwnPropertyDescriptors

返回一个对象的所有自身属性的描述符。
4. ES2018 (ECMAScript 9) 新特性
异步迭代（for-await-of）

允许你迭代等待 Promise 解决的异步可迭代对象。
Promise.finally

用于指定不管 Promise 对象最后状态如何，都会执行的操作。
Rest/Spread 属性

对象字面量中的剩余属性和扩展属性。
正则表达式命名捕获组

允许给正则表达式的捕获组命名。
正则表达式反向断言

允许匹配前面不是特定模式的字符串。
正则表达式 dotAll 模式

允许点（.）匹配任何单个字符，包括换行符和其他通常不被匹配的字符。
Template Literal Revision

允许模板文字中的嵌入表达式包含字符串字面量。
5. ES2019 (ECMAScript 10) 新特性
Array的flat()和flatMap()

flat() 用于将嵌套的数组“拉平”，flatMap() 首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。
Object.fromEntries

将键值对列表转换为一个对象。
String.prototype.trimStart/trimEnd

用于去除字符串开头/结尾的空白字符。
可选的Catch Binding

允许 catch 语句省略异常变量。
Function.prototype.toString 的修订

toString() 方法返回的字符串将与函数的源代码保持一致性。
Symbol.prototype.description

允许读取 Symbol 的描述。
6. ES2020 (ECMAScript 11) 新特性
BigInt

一种新的数字类型，可以表示非常大的整数。
Dynamic Import

允许你在需要的时候再导入模块。
Nullish coalescing Operator (??)

一种逻辑运算符，当左侧的操作数为 null 或 undefined 时，返回其右侧操作数。
Optional Chaining (?.)

允许你在访问对象属性时不必检查每级属性是否存在。
Promise.allSettled

返回一个在所有给定的 promise 已经兑现或被拒绝后的 promise，并带有一个对象数组，每个对象表示对应的 promise 结果。
globalThis

提供了一个标准的方式来获取不同环境下的全局 this 对象。
for-in mechanics

for-in 循环现在有了明确的枚举顺序。
import.meta

一个提供有关模块的信息的元数据对象。
String.prototype.matchAll

返回一个迭代器，它产生所有匹配正则表达式的匹配对象。
7. ES2021 (ECMAScript 12) 新特性
String.prototype.replaceAll

允许你替换字符串中的所有匹配项。
Promise.any

类似于 Promise.race，但只要其中的一个 promise 兑现，就返回那个已经兑现的 promise。
WeakRefs

引入了弱引用（WeakRef）和最终化注册表（FinalizationRegistry），允许开发者手动管理内存。
Logical Assignment Operators

引入了逻辑赋值运算符，如 &&=、||= 和 ??=。
8. ES2022 (ECMAScript 13) 及之后的新特性
Class Fields

引入了类实例字段和静态字段。
Private Methods and Accessors

类的私有方法和访问器。
Top-Level await

允许在模块的顶层使用 await 关键字。
RegExp Match Indices

正则表达式匹配对象的 .indices 属性，提供了匹配字符串的起始和结束索引。
Ergonomic brand checks for Private Fields

提供了一种更简洁的方式来检查一个对象是否包含某个私有字段。
Temporal API

提供了一套新的 API 用于处理日期和时间。
