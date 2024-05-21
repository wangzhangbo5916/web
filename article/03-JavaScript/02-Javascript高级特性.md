# 高级特性

- 异步编程: 回调函数、Promises、async/await、事件循环。
- 模块化: CommonJS、ES6 Modules、动态导入。
- Dynamic Import: 允许你在需要的时候再导入模块。
- import.meta: 一个提供有关模块的信息的元数据对象。
- 事件处理: 事件监听、事件委托、自定义事件。
- 内存管理: 垃圾回收机制、内存泄漏问题。
- 模板字符串: 使用反引号(`)进行字符串插值和多行字符串。
- 类（Class）: 引入了基于类的面向对象编程。
- Class Fields: 引入了类实例字段和静态字段。
- Private Methods and Accessors: 类的私有方法和访问器。
- 迭代器（Iterator）和生成器（Generator）: 引入了迭代器协议和生成器函数，可以自定义迭代行为。
- Proxy: Proxy 用于创建一个对象的代理，从而实现基本操作的拦截和自定义。
- Reflect: Reflect 是一个内置的对象，它提供拦截 JavaScript 操作的方法。
- Async/Await: 用于简化使用 Promise 的异步操作。
- 异步迭代（for-await-of）: 允许你迭代等待 Promise 解决的异步可迭代对象。
- Promise.finally: 用于指定不管 Promise 对象最后状态如何，都会执行的操作。
- Promise.allSettled: 返回一个在所有给定的 promise 已经兑现或被拒绝后的 promise，并带有一个对象数组，每个对象表示对应的 promise 结果。
- Promise.any: 类似于 Promise.race，但只要其中的一个 promise 兑现，就返回那个已经兑现的 promise。
- Rest/Spread 属性: 对象字面量中的剩余属性和扩展属性。
- WeakRefs: 引入了弱引用（WeakRef）和最终化注册表（FinalizationRegistry），允许开发者手动管理内存。
- Logical Assignment Operators: 引入了逻辑赋值运算符，如 &&=、||= 和 ??=。
- Top-Level await: 允许在模块的顶层使用 await 关键字。

## DOM 和浏览器 API

- DOM 操作: 查询和修改 DOM 元素、事件处理、创建和删除节点。
- BOM: 浏览器对象模型，包括 window、navigator、location、history、screen 对象。
- AJAX 和网络请求: XMLHttpRequest 对象、Fetch API。
- 本地存储: localStorage、sessionStorage、IndexedDB。
- 文件和流处理: File API、Blob、FileReader、Streams API。
