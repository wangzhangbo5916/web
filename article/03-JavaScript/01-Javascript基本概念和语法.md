# 基本概念和语法

- 变量和数据类型: 使用 var、let、const 声明变量；了解基本数据类型（字符串、数字、布尔值等）和复合数据类型（对象、数组）。
- Symbol：一种全新的原始数据类型，表示独一无二的值。
- Symbol.prototype.description: 允许读取 Symbol 的描述。
- BigInt: 一种新的数字类型，可以表示非常大的整数。
- globalThis: 提供了一个标准的方式来获取不同环境下的全局 this 对象。
- 操作符: 算术、逻辑、比较、位运算、赋值、字符串拼接等。
- 指数运算符\*\*: 新的运算符用于计算幂运算。
- Nullish coalescing Operator (??): 一种逻辑运算符，当左侧的操作数为 null 或 undefined 时，返回其右侧操作数。
- Optional Chaining (?.): 允许你在访问对象属性时不必检查每级属性是否存在。
- 控制结构: if-else、switch、for、while、do-while、for-in、for-of。
- for-in mechanics: for-in 循环现在有了明确的枚举顺序。
- 函数: 函数声明、函数表达式、箭头函数、回调函数、立即执行函数表达式（IIFE）。
- 作用域和闭包: 理解全局作用域、局部作用域、块级作用域以及闭包的概念。
- 对象和原型: 对象字面量、构造函数、原型链、继承、Object 类的方法。
- 数组和迭代方法: 数组的创建和操作，以及 map、filter、reduce 等迭代方法。
- 字符串和正则表达式: 字符串处理方法和正则表达式的使用。
- 错误处理: try-catch 语句、throw 关键字、Error 对象。
- 箭头函数: 提供了一种更简洁的方式来写函数表达式。
- 默认参数: 函数参数可以有默认值。
- 解构赋值: 一种特殊的语法，允许我们“解开”数组或对象，并将其元素或属性分配给一系列变量。
- Array 新方法: Array.prototype.find、 Array.prototype.includes 等。
- Array 的 flat()和 flatMap(): flat() 用于将嵌套的数组“拉平”，flatMap() 首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。
- 新数据结构: Map、Set、WeakMap 和 WeakSet。
- Object.values/Object.entries: 返回一个对象自身的所有可枚举属性值的数组/Object.entries()返回一个数组，其元素是与直接在 object 上找到的可枚举属性键值对相对应的数组。
- Object.fromEntries: 将键值对列表转换为一个对象。
- String padding: String.prototype.padStart 和 String.prototype.padEnd 允许你将空字符串或其他字符串添加到原始字符串的开头或结尾。
- String.prototype.trimStart/trimEnd: 用于去除字符串开头/结尾的空白字符。
- String.prototype.matchAll: 返回一个迭代器，它产生所有匹配正则表达式的匹配对象。
- String.prototype.replaceAll: 允许你替换字符串中的所有匹配项。
- 正则表达式命名捕获组: 允许给正则表达式的捕获组命名。
- 正则表达式反向断言: 允许匹配前面不是特定模式的字符串。
- 正则表达式 dotAll 模式: 允许点（.）匹配任何单个字符，包括换行符和其他通常不被匹配的字符。
- RegExp Match Indices: 正则表达式匹配对象的 .indices 属性，提供了匹配字符串的起始和结束索引。
- Template Literal Revision: 允许模板文字中的嵌入表达式包含字符串字面量。
- 可选的 Catch Binding: 允许 catch 语句省略异常变量。
- Function.prototype.toString 的修订: toString() 方法返回的字符串将与函数的源代码保持一致性。
- Ergonomic brand checks for Private Fields: 提供了一种更简洁的方式来检查一个对象是否包含某个私有字段。
- Temporal API: 提供了一套新的 API 用于处理日期和时间。

## 变量和数据类型

### var、let、const 使用场景与注意事项

#### var 声明变量

- 场景: 主要用于兼容旧代码或需要函数级作用域的变量。
- 注意事项:
  - 存在变量提升。
  - 可以重复声明。
  - 函数外声明的变量是全局变量。

```js
console.log(foo); // 输出：undefined，因为变量提升
var foo = 'Hello, var!';

var bar = 'First declaration.';
var bar = 'Second declaration.'; // 有效，可以重复声明
console.log(bar); // 输出：Second declaration.

for (var i = 0; i < 3; i++) {
  // ...
}
console.log(i); // 输出：3，因为var声明的变量是函数级作用域
```

#### let 声明变量

- 场景: 声明块级作用域的局部变量，适用于限定变量作用范围。
- 注意事项:
  - 不存在变量提升。
  - 同一作用域内不能重复声明。
  - 适用于循环和条件语句的局部变量。

```js
// console.log(baz); // 错误：ReferenceError，因为不存在变量提升
let baz = 'Hello, let!';

let qux = 'First declaration.';
// let qux = 'Second declaration.'; // 错误：SyntaxError，不能重复声明

for (let j = 0; j < 3; j++) {
  console.log(j); // 输出：0, 1, 2
}
// console.log(j); // 错误：ReferenceError，因为let声明的变量是块级作用域
```

#### const 声明变量

- 场景: 声明块级作用域的常量，值不可变。
- 注意事项:
  - 必须在声明时初始化。
  - 值不能改变，不可重复声明。
  - 对象和数组内部成员可以修改。

```js
// const quux; // 错误：SyntaxError，必须初始化
const quux = 'Hello, const!';
// quux = 'New value'; // 错误：TypeError，不能重新赋值

const obj = { key: 'value' };
obj.key = 'newValue'; // 有效，对象的属性可以修改
console.log(obj.key); // 输出：newValue

const arr = [1, 2, 3];
arr.push(4); // 有效，数组的元素可以修改
console.log(arr); // 输出：[1, 2, 3, 4]
```

### 暂时性死区（Temporal Dead Zone, TDZ）

暂时性死区指的是变量从它所在的作用域开始到初始化之间的那段区域。在这个区域内访问变量会导致 ReferenceError。这个概念主要与 let 和 const 声明的变量有关，因为它们不会像使用 var 声明的变量那样在作用域开始时就进行提升。

#### TDZ 的特点

1. 在代码块内，使用 let 和 const 声明的变量在声明之前都不可访问。
2. TDZ 的存在使得 let 和 const 声明的变量不会被提升。
3. 访问 TDZ 内的变量会导致 ReferenceError。
4. TDZ 的结束点是变量声明处，一旦通过声明语句，TDZ 结束，变量可以被正常访问。

#### TDZ 的作用

TDZ 的存在有助于捕捉错误，因为它确保变量在使用前已经被声明和初始化，这有助于避免因变量提升导致的错误和不可预测的行为。

```js
// TDZ开始
console.log(myVar); // undefined，因为var声明的变量会被提升
// console.log(myLet); // ReferenceError，因为变量myLet还在TDZ中
// console.log(myConst); // ReferenceError，因为变量myConst还在TDZ中

var myVar = 'var can be hoisted';
let myLet = 'let cannot be accessed before declaration';
const myConst = 'const cannot be accessed before declaration';
// TDZ结束

console.log(myVar); // 输出：var can be hoisted
console.log(myLet); // 输出：let cannot be accessed before declaration
console.log(myConst); // 输出：const cannot be accessed before declaration
```

在上面的代码中，尝试在声明之前访问 myLet 和 myConst 都会导致 ReferenceError，因为它们还处于各自的暂时性死区中。而 var 声明的变量由于变量提升，可以在声明前访问，但是值为 undefined。这个例子说明了 TDZ 的概念以及它是如何工作的。

### js 的基础数据类型和复合数据类型分别是什么？

#### JavaScript 的基础数据类型（原始类型）

尽管 BigInt 和 Symbol 都可以被包装成对象，但是它们与 Number、String 和 Boolean 不同，后者有对应的构造函数（如 new Number()、new String()、new Boolean()）可以创建显式的包装对象。而尝试使用 new BigInt() 或 new Symbol() 创建包装对象会导致抛出错误，因为这两种类型并不是构造函数。

1. undefined - 表示未定义的值，通常用于变量声明后未初始化时的默认值。
2. null - 表示空值或不存在的对象。
3. boolean - 表示逻辑值，有两个字面值：true 和 false。
4. number - 表示数字，包括整数和浮点数。
5. BigInt - 用于表示大于 2^53 - 1 的整数。BigInt 没有一个专门的包装对象类。不过，与其他原始类型一样，BigInt 值可以通过 BigInt() 函数隐式地转换为一个包装对象。这个包装对象是一个普通的 JavaScript 对象，它包含一个指向原始 BigInt 值的内部插槽。
6. string - 表示文本数据，由字符组成的序列。
7. Symbol - 表示唯一的、不可变的原始值，常用于对象属性的键。尽管没有一个像 Number 或 String 那样的专门的包装对象，但 JavaScript 允许使用 Symbol() 函数隐式地将 Symbol 值包装成一个对象形式。这种包装对象包含一个指向原始 Symbol 值的引用。然而，这并不是通过 new Symbol() 这样的构造函数来创建的，因为 Symbol 不是一个构造函数。

#### JavaScript 的复合数据类型（引用类型）

复合数据类型的特点是它们可以包含多个值，这些值可以是基础数据类型的值，也可以是另一个复合数据类型的值。复合数据类型在内存中通常是通过引用来访问的。

1. Object - 表示键值对的集合，是所有复杂数据结构的基础。
   - Function - 函数是对象的一种，可以执行代码。
   - Array - 数组是有序集合的对象，用于存储多个值。
   - Date - 日期和时间的对象。
   - RegExp - 正则表达式对象，用于字符串的模式匹配。
2. 其他构造对象 - 如 Map、Set、WeakMap、WeakSet 等，提供了额外的数据结构。

### js 的原始类型自动装箱和拆箱?

在 JavaScript 中，自动装箱和拆箱是原始类型与它们的包装对象之间的隐式转换过程。

自动装箱(Auto-boxing)
当你对一个原始类型的值执行一些看似需要对象才能进行的操作时，JavaScript 引擎会临时将这个原始值转换成一个对应的包装对象，从而可以访问那些只有对象才有的属性和方法。这个过程叫做自动装箱。例如：

```js
let myString = 'hello';
console.log(myString.toUpperCase()); // 自动装箱成String对象来调用toUpperCase方法
```

在上面的代码中，myString 是一个原始字符串。当我们调用 .toUpperCase() 方法时，JavaScript 临时将 myString 装箱成一个 String 对象，然后在这个对象上调用 .toUpperCase() 方法。

拆箱(Unboxing)
拆箱是自动装箱的逆过程。当从一个对象中获取原始值时，JavaScript 引擎会自动从对象中提取出原始值。这通常发生在需要使用原始值的上下文中，比如算术运算或逻辑比较。例如：

```js
let myNumberObj = new Number(10);
let myNumber = myNumberObj.valueOf(); // 拆箱成原始类型的值
```

在上面的代码中，myNumberObj 是一个 Number 对象。当我们调用 .valueOf() 方法时，JavaScript 提取出对象内部的原始数值，并将其赋值给 myNumber。

注意点

- 自动装箱和拆箱通常是透明的，开发者可能并不会意识到这些过程的发生。
- 尽管自动装箱提供了便利，但它可能会导致性能问题，因为每次装箱和拆箱都涉及创建和销毁临时对象的开销。
- 为了避免不必要的装箱和拆箱，推荐直接使用原始类型，而不是它们的包装对象。
- null 和 undefined 是没有包装对象的原始类型，所以它们不参与装箱和拆箱过程。

### js 的原始类型（包含 Symbol 和 BigInt）自动装箱和拆箱有哪些场景？

JavaScript 的原始类型包括：undefined、null、boolean、number、string、symbol 和 bigint。其中，undefined 和 null 没有对应的包装对象，因此不参与自动装箱和拆箱。以下是其他原始类型的自动装箱和拆箱场景。

#### 字符串(String)

装箱: 当对原始字符串类型使用任何非原始的方法或属性时，如 length 属性或 toUpperCase()方法。

```js
let str = 'hello';
console.log(str.length); // 自动装箱为String对象以访问length属性
console.log(str.charAt(0)); // 自动装箱为String对象以调用charAt方法
```

拆箱: 当字符串对象用在需要原始字符串的地方时，如算术运算。

```js
let strObj = new String('world');
console.log(strObj + '!'); // 拆箱为原始字符串进行连接操作
```

#### 数字(Number)

装箱: 当对原始数字类型使用任何非原始的方法或属性时，如 toFixed()方法。

```js
let num = 123;
console.log(num.toFixed(2)); // 自动装箱为Number对象以调用toFixed方法
```

拆箱: 当数字对象用在需要原始数字的地方时，如算术运算。

```js
let numObj = new Number(456);
console.log(numObj * 2); // 拆箱为原始数字进行乘法运算
```

#### 布尔值(Boolean)

装箱: 当对原始布尔类型使用任何非原始的方法或属性时。

```js
let bool = true;
console.log(bool.toString()); // 自动装箱为Boolean对象以调用toString方法
```

拆箱: 当布尔对象用在需要原始布尔值的地方时，如逻辑运算。

```js
let boolObj = new Boolean(false);
console.log(boolObj && true); // 拆箱为原始布尔值进行逻辑与运算
```

#### 符号(Symbol)

装箱: 当对原始符号类型尝试使用任何非原始的方法或属性时，即使符号类型没有包装对象，JavaScript 也会抛出错误。

```js
let sym = Symbol('unique');
// Symbol类型不会自动装箱，以下操作会抛出错误
// console.log(sym.description);
```

拆箱: 符号类型不存在拆箱，因为它们没有对应的对象包装器。

#### 大整数(BigInt)

装箱: 当对原始 BigInt 类型使用任何非原始的方法或属性时。

```js
let bigInt = 123n;
console.log(bigInt.toString()); // 自动装箱为BigInt对象以调用toString方法
```

拆箱: BigInt 类型不存在拆箱，因为它们通常直接作为原始值使用。

#### 注意点

- 在使用原始类型时，通常不需要显式地考虑装箱和拆箱。
- JavaScript 引擎在内部处理这些转换，但是了解这些过程可以帮助开发者避免潜在的问题，比如性能问题或者在使用 typeof 运算符时产生的意外结果。
- 尽管可以创建如 new String()或 new Number()的对象包装器，但这种做法并不常见，且可能导致混淆，因为它们与原始类型在比较时是不同的。

### Symbol 的基础语法以及属性方法

#### 基础语法

创建 Symbol

```js
let symbol1 = Symbol();
let symbol2 = Symbol('description'); // 带有描述
```

Symbol.for 和 Symbol.keyFor

```js
let symbol3 = Symbol.for('key'); // 在全局注册表中创建或重用Symbol
let key = Symbol.keyFor(symbol3); // 获取全局注册表中Symbol的键
```

#### Symbol 的方法

1. **Symbol.prototype.toString()**
   返回 Symbol 的字符串表示。

   ```js
   // 创建一个Symbol
   let symbol = Symbol('demo');
   // 使用 toString()方法将 Symbol 转换为字符串形式
   let symbolString = symbol.toString();
   console.log(symbolString); // 输出: "Symbol(demo)"
   ```

2. **Symbol.prototype.valueOf()**
   返回 Symbol 对象的原始值。

   ```js
   // 创建一个Symbol
   let symbol = Symbol('demo');
   // 使用valueOf()方法获取Symbol的原始值
   let symbolValue = symbol.valueOf();
   console.log(symbolValue === symbol); // 输出: true
   console.log(symbolValue); // 输出: Symbol(demo)
   ```

3. **Symbol.prototype.description**
   返回 Symbol 的可选描述的字符串。

   ```js
   // 创建一个Symbol
   let symbol = Symbol('demo');
   // 使用description属性获取Symbol的描述
   let symbolDescription = symbol.description;
   console.log(symbolDescription); // 输出: "demo"
   ```

4. **Symbol.for()**
   在全局 Symbol 注册表中搜索现有的 Symbol，并返回它，否则将创建一个新的 Symbol。

   ```js
   // 使用Symbol.for()从全局注册表中创建或检索Symbol
   let symbol1 = Symbol.for('globalSymbol');
   let symbol2 = Symbol.for('globalSymbol');
   // 检查两个Symbol是否相同
   console.log(symbol1 === symbol2); // 输出: true
   // 获取Symbol的描述
   let symbolDescription = symbol1.description;
   console.log(symbolDescription); // 输出: "globalSymbol"
   ```

5. **Symbol.keyFor()**
   从全局 Symbol 注册表中检索与某个 Symbol 关联的键。

   ```js
   // 使用Symbol.for()方法在全局注册表中创建一个Symbol
   let globalSymbol = Symbol.for('globalSymbol');
   // 使用Symbol.keyFor()方法获取全局Symbol的键
   let key = Symbol.keyFor(globalSymbol);
   console.log(key); // 输出: "globalSymbol"
   ```

#### Symbol 的属性

1. **Symbol.iterator**
   一个方法，它被 for-of 语句调用，返回对象的默认迭代器。

   ```js
   // 创建一个自定义对象
   let customIterable = {};

   // 为对象添加一个迭代器方法
   customIterable[Symbol.iterator] = function () {
     let count = 0;
     return {
       next: function () {
         count++;
         if (count <= 3) {
           return { value: count, done: false };
         } else {
           return { value: undefined, done: true };
         }
       },
     };
   };

   // 使用for...of循环来迭代对象
   for (let value of customIterable) {
     console.log(value); // 输出: 1, 2, 3
   }
   ```

2. **Symbol.asyncIterator**
   一个方法，它被异步迭代语句调用，返回对象的默认异步迭代器。

   ```js
   // 创建一个自定义异步迭代对象
   let customAsyncIterable = {
     async *[Symbol.asyncIterator]() {
       yield 'hello';
       yield 'async';
       yield 'iterator';
     },
   };

   // 使用for await...of循环来异步迭代对象
   (async function () {
     for await (let value of customAsyncIterable) {
       console.log(value); // 依次输出: "hello", "async", "iterator"
     }
   })();
   ```

3. **Symbol.match**
   一个正则表达式方法，它比较字符串与正则表达式。

   ```js
   class CustomMatcher {
     constructor(value) {
       this.value = value;
     }
     [Symbol.match](string) {
       return string.includes(this.value);
     }
   }

   let matcher = new CustomMatcher('hello');
   // JavaScript 的 String 对象的 match 方法可以接受一个参数，该参数可以是一个正则表达式或者一个具有 Symbol.match 方法的对象。
   console.log('hello world'.match(matcher)); // 输出: true
   ```

4. **Symbol.search**
   一个正则表达式方法，它返回字符串中匹配正则表达式的索引。

   ```js
   class CustomSearcher {
     constructor(value) {
       this.value = value;
     }
     [Symbol.search](string) {
       return string.indexOf(this.value);
     }
   }

   let searcher = new CustomSearcher('world');
   console.log('hello world'.search(searcher)); // 输出: 6
   ```

5. **Symbol.species**
   一个构造函数值，用来创建派生对象。

   ```js
   class MyArray extends Array {
     // 在一个类上定义 Symbol.species 属性，以指定在创建衍生对象时应该使用哪个构造函数。默认情况下，Symbol.species 是一个返回当前类构造函数的 getter。
     static get [Symbol.species]() {
       return Array;
     }
   }

   let a = new MyArray(1, 2, 3);
   // 当调用一个可能返回类的实例的方法时，比如 map, filter, slice 等，这些方法内部会通过 Symbol.species 属性来确定使用哪个构造函数来创建新的实例。
   let mapped = a.map((x) => x * x);

   console.log(mapped instanceof MyArray); // 输出: false
   console.log(mapped instanceof Array); // 输出: true
   ```

   Symbol.species 允许开发者控制哪些方法（如 map, filter, slice 等）应该返回当前实例的类型，还是另一个可以自定义的类型。这在类继承时特别有用，当你想要继承的类的方法返回新实例，但你希望这些新实例是父类的实例而不是当前类的实例时。

   例如，当你扩展原生的 Array 类并添加一些自定义功能，但希望所有由原生方法返回的数组仍然是普通的 Array 实例时，你可以设置 Symbol.species 为 Array。这样，即使是从你的自定义数组类创建的实例，使用原生方法返回的也将是普通的 Array 实例，而不是你的自定义数组类的实例。

6. **Symbol.split**
   一个正则表达式方法，它在匹配正则表达式的索引位置，将字符串拆分为数组。

   ```js
   class CustomSplitter {
     constructor(value) {
       this.value = value;
     }
     [Symbol.split](string) {
       return string.split(this.value);
     }
   }

   let splitter = new CustomSplitter(' ');
   console.log('hello world'.split(splitter)); // 输出: ["hello", "world"]
   ```

7. **Symbol.toPrimitive**
   一个方法，它将对象转换为相应的原始值。

   ```js
   let obj = {
     [Symbol.toPrimitive](hint) {
       if (hint === 'number') {
         return 10;
       }
       if (hint === 'string') {
         return 'hello';
       }
       return true;
     },
   };

   console.log(+obj); // 输出: 10
   console.log(`${obj}`); // 输出: "hello"
   console.log(obj == true); // 输出: true
   ```

8. **Symbol.toStringTag**
   一个字符串值，用于创建对象的默认字符串描述。

   ```js
   class CustomObject {
     get [Symbol.toStringTag]() {
       return 'CustomObject';
     }
   }

   let obj = new CustomObject();
   console.log(obj.toString()); // 输出: "[object CustomObject]"
   ```

### Symbol 的使用场景

#### 1. 唯一属性键

Symbol 可用于创建对象的唯一属性键，这样可以防止属性名的冲突，特别是在大型项目中或者当你的代码需要与第三方库协同工作时。

```js
let id = Symbol('id');
let user = {
  name: 'John',
  [id]: 123, // Symbol作为属性名
};

console.log(user[id]); // 123
console.log(user['id']); // undefined，因为"user.id"并不等同于"user[id]"
```

#### 2. 表示私有成员

在类中，Symbol 可以用来模拟私有成员。由于 Symbol 是不可枚举的，因此无法通过常规方法（如 for-in 循环或 Object.keys()）直接访问到使用 Symbol 作为键的属性。

```js
let _age = Symbol('age');

class Person {
  constructor(name, age) {
    this.name = name;
    this[_age] = age;
  }

  displayAge() {
    console.log(this[_age]);
  }
}

let bob = new Person('Bob', 30);
bob.displayAge(); // 30
console.log(bob._age); // undefined
```

#### 3. 元编程

Symbol 提供了一系列内置的元编程属性，例如 Symbol.iterator、Symbol.asyncIterator、Symbol.toStringTag 等，它们用于改变语言级别的行为（如迭代器协议）。

```js
class Collection {
  constructor(...elements) {
    this.items = elements;
  }

  [Symbol.iterator]() {
    let index = 0;
    let items = this.items;

    return {
      next() {
        if (index < items.length) {
          return { value: items[index++], done: false };
        } else {
          return { done: true };
        }
      },
    };
  }
}

let collection = new Collection(1, 2, 3);
for (let item of collection) {
  console.log(item); // 1, 2, 3
}
```

#### 4. 防止魔术字符串

在代码中使用字符串作为动态的键值或者状态码时，可以替换为 Symbol，这样可以减少因字符串重复或拼写错误导致的问题。

```js
class Collection {
  constructor(...elements) {
    this.items = elements;
  }

  [Symbol.iterator]() {
    let index = 0;
    let items = this.items;

    return {
      next() {
        if (index < items.length) {
          return { value: items[index++], done: false };
        } else {
          return { done: true };
        }
      },
    };
  }
}

let collection = new Collection(1, 2, 3);
for (let item of collection) {
  console.log(item); // 1, 2, 3
}
```

### BigInt 的基础语法以及属性方法

BigInt 提供了在 JavaScript 中处理大整数的能力，特别是在进行高精度计算或处理大数值的场景下非常有用。

#### 基础语法

BigInt 是一种内置对象，它提供了一种方法来表示大于 2^53 - 1 的整数。在 JavaScript 中，这是可以用 Number 数据类型安全表示的最大整数。BigInt 可以通过在整数后面添加 n 后缀或者调用 BigInt 函数来创建。

```js
// 使用n后缀创建BigInt
const largeNumber = 1234567890123456789012345678901234567890n;

// 使用BigInt函数创建
const anotherLargeNumber = BigInt('1234567890123456789012345678901234567890');
```

#### 属性

BigInt 类型本身没有提供任何属性。它是一个原始类型，类似于 Number，不像对象那样拥有属性。

#### 方法

由于 BigInt 是一个原始类型，它并不提供像对象那样的方法。但是，可以在 BigInt 实例上使用所有的基本算术运算符，以及与其他 BigInt 值进行比较。

```js
// 算术运算
const sum = 1n + 2n; // => 3n
const difference = 5n - 2n; // => 3n
const product = 2n * 3n; // => 6n
const quotient = 10n / 3n; // => 3n (注意：结果会被截断)
const remainder = 10n % 3n; // => 1n

// 比较运算
const isEqual = 1n === 1n; // => true
const isNotEqual = 2n !== 1n; // => true
const isGreaterThan = 2n > 1n; // => true
```

BigInt 不支持与常规 Number 类型的混合运算，如果尝试这样做，会抛出 TypeError。如果需要进行混合运算，必须先将 Number 转换为 BigInt。

```js
const number = 5;
const bigInt = 10n;

// 将Number转换为BigInt进行混合运算
const mixedSum = BigInt(number) + bigInt; // => 15n
```

#### 全局 BigInt 对象的方法

BigInt 对象本身提供了一些用于处理 BigInt 值的静态方法。

```js
// BigInt.asIntN
const maxInt32 = 2 ** 31 - 1;
const bigIntValue = BigInt.asIntN(32, BigInt(maxInt32) + 1n);
// => -2147483648n (因为超过了32位带符号整数的最大值)

// BigInt.asUintN
const maxUint32 = 2 ** 32 - 1;
const bigIntValueUnsigned = BigInt.asUintN(32, BigInt(maxUint32) + 1n);
// => 0n (因为超过了32位无符号整数的最大值)
```

#### 注意事项

在使用 BigInt 时，需要注意以下几点：

- BigInt 不能与 Number 直接混合运算。
- JSON.stringify()不支持 BigInt 值，尝试序列化 BigInt 会抛出错误。
- 不是所有的 JavaScript 环境都支持 BigInt，特别是在一些旧的浏览器或 JavaScript 环境中可能不可用。
- 在进行位运算时，BigInt 的行为与 Number 不同，因为 BigInt 可以表示任意大小的整数。

### globalThis

#### 概述

globalThis 提供了一个标准的方式来获取不同 JavaScript 环境中的全局对象。在不同的环境中，全局对象可能有不同的名字。例如，在浏览器环境中，全局对象通常是 window，在 Node.js 中是 global，在 Web Workers 中是 self。globalThis 旨在提供一个统一的、环境无关的方式来访问这个全局对象。

#### 使用 globalThis

globalThis 的主要用途是提供一个可靠的方法来访问全局作用域内的属性和方法，无论代码运行在什么环境中。

```js
// 设置一个全局变量
globalThis.myGlobalVar = 'Hello, world!';

// 访问一个全局变量
console.log(globalThis.myGlobalVar); // 输出: Hello, world!
```

#### 兼容性

globalThis 是在 ECMAScript 2020 规范中引入的。大多数现代浏览器和 JavaScript 环境已经实现了这个特性。但是，在一些旧的浏览器或者 JavaScript 环境中可能还不支持 globalThis。为了确保兼容性，可以使用以下的 polyfill：

```js
if (typeof globalThis === 'undefined') {
  Object.defineProperty(Object.prototype, '_globalThis', {
    get: function () {
      return this;
    },
    configurable: true,
  });
  _globalThis.globalThis = _globalThis; // `this` is the global object
  delete Object.prototype._globalThis;
}
```

#### 注意事项

- 使用 globalThis 时，应该注意不要无意中覆盖全局作用域中的重要变量。
- 在模块化的代码中，应避免在全局作用域中添加变量或方法，而应该使用模块导出和导入来共享功能。
- 在严格模式下，this 在全局作用域中不会指向全局对象，而 globalThis 始终会指向全局对象。
- 虽然 globalThis 提供了一个标准的方法来访问全局对象，但是仍然推荐尽可能地使用局部变量和模块作用域，以避免潜在的命名冲突和代码维护问题。

### 指数运算符

#### 概述

JavaScript 的指数运算符是\*\*，用于执行指数（幂）计算。它在 ES2016（也称为 ES7）中被引入 JavaScript 语言规范。

#### 语法

```js
let result = base ** exponent;
```

- base 是底数。
- exponent 是指数。

#### 示例

```js
// 2的3次方
let a = 2 ** 3; // 结果为8

// 5的2次方
let b = 5 ** 2; // 结果为25

// 负数的指数
let c = 4 ** -1; // 结果为0.25 (1/4)

// 括号改变运算顺序
let d = 2 ** (3 ** 2); // 结果为512, 因为先计算3的2次方得到9，然后计算2的9次方
let e = (2 ** 3) ** 2; // 结果为64, 因为先计算2的3次方得到8，然后计算8的2次方
```

#### 注意事项

- 指数运算符具有右结合性，意味着在没有括号的情况下，它会从右到左计算。
- 可以使用 Math.pow()函数来获得相同的结果，这在不支持指数运算符的旧版 JavaScript 环境中尤其有用。

  ```js
  // 使用Math.pow()的等效示例
  let f = Math.pow(2, 3); // 结果为8
  ```

- 在使用指数运算符时，如果指数是小数，那么结果将是底数的分数次幂。

  ```js
  // 使用小数作为指数
  let g = 9 ** 0.5; // 结果为3, 因为这是9的平方根
  ```

- 指数运算符也可以与赋值运算符结合使用，形成复合赋值运算符\*\*=。

  ```js
  // 使用复合赋值运算符
  let h = 3;
  h **= 2; // 现在h的值为9 (等同于h = h ** 2)
  ```

### Nullish Coalescing Operator (??)

#### 概述

Nullish coalescing operator (??) [**非空合并运算符**]是一个逻辑运算符，它在 ES2020（也称为 ES11）中被添加到 JavaScript 语言中。该运算符用于为可能是 null 或 undefined 的变量提供一个默认值。

#### 语法

```js
let result = value1 ?? defaultValue;
```

- value1 是要检查的值。
- defaultValue 是当 value1 是 null 或 undefined 时要返回的默认值。

#### 示例

```js
// 当变量为null时使用默认值
let a = null ?? 'default value'; // 结果为'default value'

// 当变量为undefined时使用默认值
let b = undefined ?? 'default value'; // 结果为'default value'

// 当变量有实际值（即非null/非undefined）时
let c = 'actual value' ?? 'default value'; // 结果为'actual value'

// 当变量为其他falsy值（如0或''）时，不会使用默认值
let d = 0 ?? 'default value'; // 结果为0
let e = '' ?? 'default value'; // 结果为''
```

#### 注意事项

- Nullish coalescing operator 与逻辑 OR 运算符(||)不同，后者在左侧操作数为任何 false 值（如 0、''、false、null、undefined、NaN）时返回右侧操作数。
- ??仅在左侧操作数为 null 或 undefined 时返回右侧操作数，这使得它在为变量设置默认值时更加精确。
- ??不能与||或&&运算符直接混合使用，因为这会引起语法错误。如果需要混合使用，应该使用括号来明确运算符的优先级。

  ```js
  // 错误的混合使用，会引发语法错误
  // let f = null || undefined ?? 'default value'; // SyntaxError

  // 正确的混合使用
  let g = (null || undefined) ?? 'default value'; // 结果为'default value'
  ```

### Optional Chaining (?.)

#### 概述

Optional Chaining (?.) 是一个在 JavaScript 中用于访问对象属性的运算符，它在 ES2020（也称为 ES11）中被引入。这个运算符可以在尝试访问一个对象的嵌套子属性时，不需要显式地验证每一层属性是否存在。

#### 语法

```js
let value = object?.property;
let value = object?.[expression];
let value = array?.[index];
let methodValue = object?.method(args);
```

- object 是要访问其属性的对象。
- property 是对象中我们想要安全访问的属性。
- expression 是计算属性名时使用的表达式。
- index 是数组中我们想要安全访问的索引。
- method 是对象中我们想要安全调用的方法。

#### 示例

```js
const user = {
  name: 'John',
  address: {
    street: 'Main St',
    city: 'New York',
  },
};

// 访问嵌套属性时不会抛出错误
let street = user.address?.street; // 结果为'Main St'
let zipcode = user.address?.zipcode; // 结果为undefined，因为zipcode不存在

// 在数组中使用
let firstItem = [1, 2, 3]?.[0]; // 结果为1
let tenthItem = [1, 2, 3]?.[9]; // 结果为undefined，因为索引9不存在

// 在方法调用中使用
let length = user.getName?.(); // 结果为undefined，因为getName方法不存在
```

#### 注意事项

- 如果在?.之前的属性是 null 或 undefined，表达式的运算结果会立即返回 undefined，而不会继续访问后面的属性。
- Optional Chaining 只能用来访问属性或调用方法，不能用来赋值或作为左值使用。
- 使用 Optional Chaining 时，不能用来判断一个属性是否存在，因为无论属性是否存在，如果前面的对象是 null 或 undefined，都会返回 undefined。

  ```js
  // 错误的使用方式，不能用于赋值
  // user.address?.street = '123 Maple St'; // SyntaxError

  // 判断属性是否存在
  let hasStreet = 'street' in user.address; // 结果为true
  ```
