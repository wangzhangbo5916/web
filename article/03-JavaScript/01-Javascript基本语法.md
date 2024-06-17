# 基本语法

## 变量

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

## 数据类型

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

### Map

Map 是一种键值对集合，类似于对象，但它的键可以是任何类型的值，包括函数、对象或任意基本类型。

```js
let map = new Map();

// 设置键值对
map.set('key1', 'value1');
map.set('key2', 'value2');

// 获取值
console.log(map.get('key1')); // 输出: value1

// 检查键是否存在
console.log(map.has('key2')); // 输出: true

// 遍历 Map
map.forEach((value, key) => {
  console.log(key + ': ' + value);
});

// 删除键
map.delete('key1');

// 清空 Map
map.clear();
```

**使用场景**：

- 当需要存储不同数据类型的键时。
- 当需要保持键值对的插入顺序时。
- 当进行大量的查找、删除操作时，因为 Map 的这些操作通常比在对象中更快。

**最佳实践**：

- 使用 Map 来替代对象字面量，尤其是键的集合不仅限于字符串和符号时。
- 在需要频繁增删键值对的场合使用 Map，以获得更好的性能。

```js
let productRatings = new Map();

// 添加商品评分
productRatings.set('product1', 4.7);
productRatings.set('product2', 4.3);

// 更新商品评分
productRatings.set('product1', 4.8);

// 获取商品评分
console.log(productRatings.get('product1')); // 输出: 4.8

// 迭代商品评分
for (let [product, rating] of productRatings) {
  console.log(product + ' has a rating of ' + rating);
}
```

### Set

Set 是一种集合数据结构，它只存储唯一的值，不允许重复。

```js
let set = new Set();

// 添加值
set.add('apple');
set.add('banana');
set.add('apple'); // 重复的 'apple' 不会被添加

// 检查值是否存在
console.log(set.has('apple')); // 输出: true

// 遍历 Set
set.forEach((value) => {
  console.log(value);
});

// 删除值
set.delete('apple');

// 清空 Set
set.clear();
```

### WeakMap

WeakMap 是一种特殊的 Map，其键必须是对象，而且是弱引用的，这意味着如果没有其他引用指向键对象，这些键对象可以被垃圾回收机制回收。

```js
let weakmap = new WeakMap();
let obj = {};

// 设置键值对
weakmap.set(obj, 'some value');

// 获取值
console.log(weakmap.get(obj)); // 输出: some value

// obj 没有其他引用时，可以被垃圾回收
```

**使用场景**：

- 当你需要将额外的数据与对象关联，而不干扰对象的垃圾回收时。
- 在实现私有属性时，WeakMap 可以作为存储私有数据的安全方式。

**最佳实践**：

- 不要在 WeakMap 中存储对象的引用，否则这些对象将不会被垃圾收集器回收。
- 由于 WeakMap 的键不可枚举，所以不应该用它来存储可枚举的数据。

```js
let weakMap = new WeakMap();
let domNode = document.getElementById('myElement');

// 给 DOM 节点关联一些数据，不影响垃圾回收
weakMap.set(domNode, { timesClicked: 0 });

// 当 DOM 节点被移除时，关联的数据会自动被清理
```

### WeakSet

WeakSet 是一种集合，它只能包含对象，并且这些对象都是弱引用的，这意味着如果没有其他引用指向这些对象，它们可以被垃圾回收。

```js
let weakset = new WeakSet();
let obj = {};

// 添加对象
weakset.add(obj);

// 检查对象是否存在
console.log(weakset.has(obj)); // 输出: true

// obj 没有其他引用时，可以被垃圾回收
```

**使用场景**：

- 当你需要跟踪一组对象是否已经被处理过，而不阻止这些对象被垃圾回收时。
- 在管理不希望外部代码访问的对象集合时。

**最佳实践**：

- 与 WeakMap 类似，WeakSet 中的对象都是弱引用，不要用它来存储需要长期持有的对象。
- 由于 WeakSet 的成员不可枚举，所以它不适合用来存储需要迭代的数据。

```js
let weakSet = new WeakSet();

(function () {
  let a = {};
  weakSet.add(a);
  // 在这个匿名函数执行完毕后，没有其他引用指向对象a，它将被垃圾回收
})();

// 无法检查a是否在weakSet中，因为a已经被清理
```

### 为什么 Map 和 Set 查找和删除效率高于对象

#### 1. 数据结构差异

**Map 和 Set：**

- Map 是键值对集合，Set 是值的集合，两者都是 ES6 新增的数据结构。
- 它们都可以存储不重复的值，其中 Map 是一组键值对的结构，具有唯一的键。
- Map 和 Set 都允许存储任何类型的唯一值，无论是原始值还是对象引用。
- 它们都保持了元素的插入顺序，使得迭代行为是可预测的。
- Map 和 Set 都是基于哈希表实现的，这意味着它们在添加、查找和删除操作上提供了非常快速的性能，通常是常数时间复杂度 O(1)。

**对象：**

- 对象在 JavaScript 中是最基本的数据结构，通常用来存储键值对，但键必须是字符串或符号。
- 对象没有保持元素插入顺序的特性，特别是当键是整数时，其迭代顺序可能会变得不可预测。
- 对象没有内建的机制来追踪存储的键值对数量，需要通过代码来手动计算。
- 对象的键值对并不是基于哈希表实现的，因此在查找和删除操作上可能不如 Map 或 Set 高效。

#### 2. 查找和删除操作

**Map 和 Set：**

- 查找（Map: get, Set: has）：Map 和 Set 的查找操作都是通过哈希表来实现的，这使得无论数据结构中存储了多少元素，查找操作的时间复杂度都接近于 O(1)。
- 删除（Map: delete, Set: delete）：同样，Map 和 Set 的删除操作也受益于哈希表结构，可以快速定位并删除元素，时间复杂度接近于 O(1)。

**对象：**

- 查找：在对象中查找属性可能需要遍历对象的所有属性直到找到目标键，这在属性数量多的情况下性能会下降，接近线性时间复杂度 O(n)。
- 删除（delete）：删除对象的属性可能导致 JavaScript 引擎对对象的形状（shape）进行调整，这可能是一个相对较慢的操作，并且在删除操作频繁时可能会导致性能问题。

#### 3. 结论

Map 和 Set 被设计为在频繁执行查找和删除操作的场景下提供高效的性能。它们的内部实现（哈希表）使得这些操作的时间复杂度接近于 O(1)，而对象可能因为缺乏这样的优化而在性能上表现不佳。因此，在需要高效查找和删除操作的场景中，推荐使用 Map 或 Set 而不是对象。

### 为什么 Map 和 Set 的键可以是对象、函数等任何值

#### 1. 设计原则

**强化语言能力：**

- Map 和 Set 的设计是为了弥补传统对象（Object）的限制，提供更加灵活的数据结构。
- 这些数据结构的目的是为了能够使用更多种类的值作为键，包括对象和函数，这样可以让开发者有更多的选择和灵活性去构建复杂的数据集合。

#### 2. 技术实现

**使用哈希表：**

- Map 和 Set 内部使用哈希表作为基础结构，这允许它们对任何类型的键进行操作。
- 哈希表通过计算键的哈希值来存储和检索数据，这个哈希值是根据键的内容计算出来的，而不仅仅是基于字符串或数字。

**引用唯一性：**

- 对于对象和函数这样的非原始值，它们在内存中的引用是唯一的。
- Map 和 Set 通过这些唯一的引用来区分不同的键，这就允许它们将对象和函数作为键使用。

#### 3. 使用场景

**实例关联：**

- 使用对象作为键允许开发者创建一个与对象实例直接关联的数据映射，这在某些场景下非常有用，例如缓存对象的计算结果。

**函数映射：**

- 函数作为键可以用于高阶函数编程中，例如在事件监听器或回调函数映射中，这提供了一种强大的方式来动态管理和解绑事件监听器。

#### 4. 结论

Map 和 Set 能够使用任何类型的值作为键，这是因为它们是为了提供比传统对象更强大的功能而设计的。它们的内部实现支持这种灵活性，而且它们的使用场景也证明了这种设计的实用性。总的来说，这种设计使得 Map 和 Set 成为在需要键的多样性和灵活性时的理想选择。


### weakMap和weakSet在实际开发中的应用场景有哪些

#### 1. 管理私有数据

WeakMap 可以用来存储对象实例的私有数据。由于 WeakMap 中的键是弱引用，一旦外部对这个对象的引用被删除，对象将会被垃圾回收，相应的私有数据也会随之消失，不会造成内存泄漏。

```js
const privateData = new WeakMap();

class Person {
  constructor(name, age) {
    privateData.set(this, { name, age });
  }
  
  getName() {
    return privateData.get(this).name;
  }
  
  getAge() {
    return privateData.get(this).age;
  }
}

let john = new Person('John', 30);
console.log(john.getName()); // 'John'
john = null; // privateData 中对应的数据可以被垃圾回收
```

#### 2. 缓存计算结果

WeakMap 可以用于缓存对象的计算结果，只要对象还被使用，缓存就存在。当对象不再被引用，垃圾回收器会自动清理这个对象以及它的缓存，这样可以避免在长时间运行的应用中出现内存泄漏。

```js
let cache = new WeakMap();

function complexComputation(obj) {
  if (cache.has(obj)) {
    return cache.get(obj);
  } else {
    let result = /* 进行复杂计算 */ obj;
    cache.set(obj, result);
    return result;
  }
}

let data = { key: 'value' };
complexComputation(data); // 第一次计算并缓存结果
data = null; // 之后data可以被垃圾回收，cache中的记录也会被清除
```

#### 3. 跟踪对象的状态

WeakMap 可以用来跟踪对象的状态，而不必担心在对象不再使用后还要手动清理。例如，可以用它来跟踪 DOM 元素是否已经被处理过，而不用担心元素从文档中移除时的清理问题。

```js
const processedElements = new WeakSet();

function processElement(element) {
  if (!processedElements.has(element)) {
    // 对element进行处理
    processedElements.add(element);
  }
}

let el = document.getElementById('my-element');
processElement(el); // 处理DOM元素
el.parentNode.removeChild(el);
el = null; // el被移除，processedElements中的记录将被垃圾回收
```

#### 4. 关联额外数据

WeakMap 可以用来给 DOM 节点关联额外的信息，如事件监听器或数据绑定，而不必担心在节点被移除时忘记清除这些信息。

```js
const elementData = new WeakMap();

function attachData(element, data) {
  elementData.set(element, data);
}

function getData(element) {
  return elementData.get(element);
}

let div = document.createElement('div');
attachData(div, { related: true });
console.log(getData(div)); // { related: true }
div = null; // div被移除时，其关联的数据也会被垃圾回收

```

#### 5. 管理事件监听器

WeakMap 可以用于管理事件监听器的引用。例如，当你向一个对象添加事件监听器时，可以使用 WeakMap 来存储监听器，这样当对象被回收时监听器也会自动消失。

```js
const eventListeners = new WeakMap();

function addListener(obj, event, listener) {
  let listeners = eventListeners.get(obj);
  if (!listeners) {
    listeners = new Map();
    eventListeners.set(obj, listeners);
  }
  if (!listeners.has(event)) {
    listeners.set(event, new Set());
  }
  listeners.get(event).add(listener);
}

function removeListener(obj, event, listener) {
  const listeners = eventListeners.get(obj);
  if (listeners && listeners.has(event)) {
    listeners.get(event).delete(listener);
  }
}

let button = document.createElement('button');
function onClick() { console.log('Button clicked.'); }
addListener(button, 'click', onClick);
removeListener(button, 'click', onClick);
button = null; // button被移除时，其监听器也会被垃圾回收
```

#### 6. 优化数据结构

WeakSet 可以用来存储一个对象集合，并且只要集合中的某个对象不再被其他地方引用，它就可以被垃圾回收。这种方式在需要存储大量对象并且希望能自动清理那些不再需要的对象时非常有用。

```js
const seenObjects = new WeakSet();

function processObject(obj) {
  if (!seenObjects.has(obj)) {
    seenObjects.add(obj);
    // 对obj进行一些处理
  }
}

let obj1 = { key: 'value1' };
let obj2 = { key: 'value2' };
processObject(obj1);
processObject(obj2);
obj1 = null; // obj1可以被垃圾回收，seenObjects中的记录也会被清除
```

## Temporal

Temporal 是一个现代的日期和时间管理的 JavaScript API，它旨在解决 Date 对象的一些长期存在的问题和局限性。Temporal 提供了多个独立的对象类型，以更直观、更强大的方式处理日期、时间、时区、持继时间等概念。

### 核心组件

- Temporal.Instant：表示一个独立于时区的单一时间点，类似于 UNIX 时间戳。
- Temporal.PlainDate：表示没有时间和时区的日期。
- Temporal.PlainTime：表示没有日期和时区的时间。
- Temporal.PlainDateTime：表示没有时区的日期和时间。
- Temporal.ZonedDateTime：表示带有时区的日期和时间。
- Temporal.Duration：表示一个时间长度或持续时间。
- Temporal.TimeZone：表示时区，可以获取时区的相关信息。
- Temporal.Calendar：表示日历，支持多种日历系统。

### 特点

- 不可变性：Temporal 对象是不可变的，这意味着它们的方法不会更改对象本身，而是返回一个新的修改过的对象。
- 链式调用：由于不可变性，可以方便地进行链式调用，例如 date.add({ days: 1 }).with({ month: 2 })。
- 清晰的区分：Temporal 明确区分了本地时间和带时区的时间，避免了许多与时区相关的常见错误。
- 国际化：内置对不同日历系统的支持，如伊斯兰历、希伯来历等。
- 精确度：提供了纳秒级的精度，适合需要高精度时间处理的应用。

```js
const now = Temporal.Now.plainDateTimeISO();
console.log(now.toString()); // 2024-06-17T12:34:56

const duration = Temporal.Duration.from({ hours: 1, minutes: 30 });
const later = now.add(duration);
console.log(later.toString()); // 2024-06-17T14:04:56

const timeZone = Temporal.TimeZone.from('Europe/Paris');
const zonedTime = now.withTimeZone(timeZone);
console.log(zonedTime.toString()); // 2024-06-17T12:34:56+02:00[Europe/Paris]
```

在这个例子中，使用 Temporal.Now.plainDateTimeISO() 获取当前的日期和时间（不带时区），然后创建一个持续时间对象 duration，并将其添加到当前时间上。最后，将不带时区的时间转换为带有特定时区的时间。

#### 注意事项

- Temporal API 目前是一个 Stage 3 的 ECMAScript 提案，这意味着它已经接近最终阶段，但可能还会有一些变动。
- Temporal API 的设计目标是与现有的 Date 对象共存，而不是替换它。
- 在使用 Temporal API 时，需要注意浏览器的兼容性或者使用相应的 polyfill。

## globalThis

### 概述

globalThis 提供了一个标准的方式来获取不同 JavaScript 环境中的全局对象。在不同的环境中，全局对象可能有不同的名字。例如，在浏览器环境中，全局对象通常是 window，在 Node.js 中是 global，在 Web Workers 中是 self。globalThis 旨在提供一个统一的、环境无关的方式来访问这个全局对象。

### 使用 globalThis

globalThis 的主要用途是提供一个可靠的方法来访问全局作用域内的属性和方法，无论代码运行在什么环境中。

```js
// 设置一个全局变量
globalThis.myGlobalVar = 'Hello, world!';

// 访问一个全局变量
console.log(globalThis.myGlobalVar); // 输出: Hello, world!
```

### 兼容性

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

### 注意事项

- 使用 globalThis 时，应该注意不要无意中覆盖全局作用域中的重要变量。
- 在模块化的代码中，应避免在全局作用域中添加变量或方法，而应该使用模块导出和导入来共享功能。
- 在严格模式下，this 在全局作用域中不会指向全局对象，而 globalThis 始终会指向全局对象。
- 虽然 globalThis 提供了一个标准的方法来访问全局对象，但是仍然推荐尽可能地使用局部变量和模块作用域，以避免潜在的命名冲突和代码维护问题。

## 操作运算符

### 1 指数运算符

#### 1.1 概述

JavaScript 的指数运算符是\*\*，用于执行指数（幂）计算。它在 ES2016（也称为 ES7）中被引入 JavaScript 语言规范。

#### 1.2 语法

```js
let result = base ** exponent;
```

- base 是底数。
- exponent 是指数。

#### 1.3 示例

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

#### 1.4 注意事项

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

### 2 Nullish Coalescing Operator (??)

#### 2.1 概述

Nullish coalescing operator (??) [**非空合并运算符**]是一个逻辑运算符，它在 ES2020（也称为 ES11）中被添加到 JavaScript 语言中。该运算符用于为可能是 null 或 undefined 的变量提供一个默认值。

#### 2.2 语法

```js
let result = value1 ?? defaultValue;
```

- value1 是要检查的值。
- defaultValue 是当 value1 是 null 或 undefined 时要返回的默认值。

#### 2.3 示例

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

#### 2.4 注意事项

- Nullish coalescing operator 与逻辑 OR 运算符(||)不同，后者在左侧操作数为任何 false 值（如 0、''、false、null、undefined、NaN）时返回右侧操作数。
- ??仅在左侧操作数为 null 或 undefined 时返回右侧操作数，这使得它在为变量设置默认值时更加精确。
- ??不能与||或&&运算符直接混合使用，因为这会引起语法错误。如果需要混合使用，应该使用括号来明确运算符的优先级。

  ```js
  // 错误的混合使用，会引发语法错误
  // let f = null || undefined ?? 'default value'; // SyntaxError

  // 正确的混合使用
  let g = (null || undefined) ?? 'default value'; // 结果为'default value'
  ```

### 3 Optional Chaining (?.)

#### 3.1 概述

Optional Chaining (?.) [**可选链**]是一个在 JavaScript 中用于访问对象属性的运算符，它在 ES2020（也称为 ES11）中被引入。这个运算符可以在尝试访问一个对象的嵌套子属性时，不需要显式地验证每一层属性是否存在。

#### 3.2 语法

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

#### 3.3 示例

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

#### 3.4 注意事项

- 如果在?.之前的属性是 null 或 undefined，表达式的运算结果会立即返回 undefined，而不会继续访问后面的属性。
- Optional Chaining 只能用来访问属性或调用方法，不能用来赋值或作为左值使用。
- 使用 Optional Chaining 时，不能用来判断一个属性是否存在，因为无论属性是否存在，如果前面的对象是 null 或 undefined，都会返回 undefined。

  ```js
  // 错误的使用方式，不能用于赋值
  // user.address?.street = '123 Maple St'; // SyntaxError

  // 判断属性是否存在
  let hasStreet = 'street' in user.address; // 结果为true
  ```

## 循环语法

### 1 for-in

#### 1.1 使用场景

- 遍历对象属性：当你需要遍历一个对象的所有可枚举属性时，for-in 是一个方便的选择。
- 调试和检查对象：在调试时，你可能需要检查一个对象中有哪些属性和方法，for-in 可以帮助你快速查看。
- 动态属性：对于属性名未知或动态添加到对象的属性，for-in 可以动态地遍历这些属性。

#### 1.2 语法示例

```js
for (variable in object) {
  // 代码块
}
```

variable 是每次迭代分配属性名的变量，而 object 是要迭代其可枚举属性的对象。

```js
const person = { name: 'Alice', age: 25, city: 'Paris' };

for (let key in person) {
  console.log(key + ': ' + person[key]);
}
```

输出:

```shell
name: Alice
age: 25
city: Paris
```

#### 1.3 注意事项

1. 原型链属性：for-in 会遍历对象自身的属性以及原型链上的可枚举属性。使用 Object.hasOwnProperty() 可以过滤非对象自身的属性。
2. 数组遍历：不推荐使用 for-in 遍历数组，因为它会遍历数组所有可枚举属性，包括原型链上的属性，而不仅仅是数组元素。
3. 顺序不保证：for-in 循环遍历属性的顺序在不同的 JavaScript 引擎中可能不一致，且不一定按照对象属性的创建顺序。
4. 性能问题：for-in 循环可能比其他类型的循环（如 for、forEach、for-of）慢，尤其是在遍历大对象时。
5. 避免在高性能需求场景中使用：如果代码性能至关重要，考虑使用其他循环方法。
6. 枚举性检查：在使用 for-in 循环之前，了解对象的可枚举属性是很重要的，以避免遍历到不需要的属性。
7. 避免在对象原型上添加属性：如果你在对象原型上添加属性，这些属性也会被 for-in 遍历到，除非你使用 Object.defineProperty() 将它们定义为不可枚举的。

   ```js
   for (let key in person) {
     if (person.hasOwnProperty(key)) {
       console.log(key + ': ' + person[key]);
     }
   }
   ```

#### 1.4 for-in 循环的性能为什么不如 for 循环

1. 枚举属性的机制：for-in 循环需要枚举对象的所有可枚举属性，包括继承自原型链的属性。这个过程比简单地迭代一个数组的索引（如在 for 循环中）要复杂和耗时。
2. 字符串键的处理：在 for-in 循环中，对象的键作为字符串处理，这意味着 JavaScript 引擎需要管理字符串与属性之间的映射，这是一个比直接使用数值索引（如数组索引）更耗时的操作。
3. 动态查找：每次迭代时，for-in 循环都必须动态查找对象的下一个属性，这涉及到查找属性的过程，可能包括在原型链上搜索，而 for 循环通常是预先知道迭代次数和使用数值索引的。
4. 非连续性质：数组中可能存在非连续的元素，或者数组本身就是稀疏的，for-in 循环会跳过那些未定义的索引，这种跳过检查也会增加额外的开销。
5. 优化难度：JavaScript 引擎往往对常见的 for 循环进行了优化，因为它们的行为是可预测的。然而，由于 for-in 循环的动态特性，引擎更难对其进行优化。
6. 不确定的属性顺序：由于 for-in 循环不保证属性的遍历顺序，引擎可能需要采取额外的措施来维护属性的某种顺序，这可能导致性能开销。

### 2 for-of 循环

#### 2.1 使用场景

1. 数组迭代：遍历数组元素时，比传统 for 循环更简洁。
2. 字符串迭代：遍历字符串中的每个字符。
3. Map/Set 迭代：直接遍历 Map 和 Set 集合中的元素。
4. 类数组对象：如 arguments 或 NodeList，可以用 for-of 进行遍历。
5. 自定义迭代器：对于实现了迭代器协议的自定义对象。

#### 2.2 语法示例

语法结构

```js
for (variable of iterable) {
  // code block to be executed
}
```

- variable：在每次迭代中，由 iterable 集合中的每个不同属性值赋值的变量。
- iterable：一个可迭代的对象，如 Array, Map, Set, String, TypedArray, arguments 对象等。

数组遍历

```js
let array = [10, 20, 30, 40];

for (let value of array) {
  console.log(value); // 输出: 10, 20, 30, 40
}
```

字符串遍历

```js
let string = 'hello';

for (let character of string) {
  console.log(character); // 输出: 'h', 'e', 'l', 'l', 'o'
}
```

Map 遍历

```js
let map = new Map([
  ['a', 1],
  ['b', 2],
  ['c', 3],
]);

for (let [key, value] of map) {
  console.log(`${key}: ${value}`); // 输出: 'a: 1', 'b: 2', 'c: 3'
}
```

Set 遍历

```js
let set = new Set([1, 2, 3]);

for (let value of set) {
  console.log(value); // 输出: 1, 2, 3
}
```

#### 2.3 注意事项

1. 不适用于普通对象：for-of 不能直接用于普通对象的遍历，因为对象不是迭代器
2. 不能获取索引：在迭代数组时，for-of 不提供索引，如果需要索引，可能需要使用 for 循环或 forEach 方法。
3. 中断迭代：可以使用 break, continue, return 或 throw 控制迭代流程。
4. 兼容性：for-of 在 ES6（ECMAScript 2015）中引入，不支持 ES6 的旧环境可能无法使用。
5. 性能：虽然 for-of 提供了更简洁的语法，但在某些情况下（特别是在旧的 JavaScript 引擎中），它可能不如传统的 for 循环高效。

## 错误处理

### 可选的 Catch Binding

可选的 Catch Binding 是 JavaScript 中的一个特性，允许在 catch 语句中省略异常变量的绑定。这个特性在 ES2019（ECMAScript 2019）中被正式加入到了 JavaScript 语言规范中。

#### 传统的 Catch Binding
在 ES2019 之前，当你使用 try...catch 语句捕获异常时，需要明确指定一个变量来接收异常对象，如下所示：

```js
try {
  // 可能会抛出错误的代码
} catch (error) {
  // 处理错误
}
```

#### 可选的 Catch Binding 的使用

ES2019 引入的可选的 Catch Binding 允许你在不关心异常对象时省略这个变量，代码如下：

```js
try {
  // 可能会抛出错误的代码
} catch {
  // 处理错误，但不需要访问错误对象
}
```

#### 为什么使用可选的 Catch Binding

有些情况下，你可能并不需要访问具体的错误对象，只想知道是否发生了错误并进行相应的处理。在这种情况下，可选的 Catch Binding 可以让代码看起来更简洁。

```js
try {
  // 一些可能出错的代码
  throw new Error('出错了！');
} catch {
  // 仅仅知道出错了，并执行一些无需错误对象的操作
  console.log('发生了一个错误，但我们不关心错误的具体内容。');
}
```

#### 注意事项

- 可选的 Catch Binding 并不意味着错误可以被忽略，它只是提供了一种在不需要错误对象时简化代码的方式。
- 即使省略了错误对象，catch 块仍然会捕获所有的异常，因此你应该确保在 catch 块内部进行适当的错误处理。

### 错误处理最佳实践

#### 1. 使用 Error 对象而非字符串或其他类型
   使用 Error 对象抛出错误而不是字符串或其他原始类型，因为 Error 对象包含了堆栈追踪和其他有用的调试信息。

```js
// 推荐
throw new Error('Something went wrong');

// 不推荐
throw 'Something went wrong';
```

#### 2. 为错误创建自定义类
   为不同类型的错误创建自定义错误类，这有助于错误的识别和处理。继承自 Error 类可以保证堆栈追踪的完整性。

```js
class CustomError extends Error {
  constructor(message) {
    super(message);
    this.name = this.constructor.name;
    if (typeof Error.captureStackTrace === 'function') {
      Error.captureStackTrace(this, this.constructor);
    } else {
      this.stack = new Error(message).stack;
    }
  }
}
```

#### 3. 只捕获你能处理的错误

使用 try...catch 时，只捕获那些你确实能够处理的错误。不要捕获所有错误，否则可能会掩盖潜在的问题。

```js
try {
  // 只有当你能处理这个错误时
  doSomethingRisky();
} catch (error) {
  if (error instanceof KnownError) {
    handleTheError(error);
  } else {
    throw error; // 无法处理的错误应该继续抛出
  }
}
```

#### 4. 使用 finally 清理资源

无论是否发生错误，都需要执行清理工作时，使用 finally 块。这确保了资源总是被正确释放。

```js
try {
  acquireResource();
} catch (error) {
  handleTheError(error);
} finally {
  releaseResource();
}
```

#### 5. 避免使用空的 catch 块

空的 catch 块会吞掉所有错误，这会使得调试变得非常困难。如果你不想处理某个错误，至少要记录它。

```js
try {
  doSomething();
} catch (error) {
  // 不推荐：空的catch块
  // 推荐：至少记录错误信息
  console.error(error);
}
```

#### 6. 对于异步代码使用 Promise 的 catch 方法或 async/await

使用 Promise 的.catch()方法或 async/await 结合 try...catch 来处理异步操作中的错误。

```js
// 使用Promise的.catch()
doSomethingAsync()
  .then((result) => {
    processTheResult(result);
  })
  .catch((error) => {
    handleTheError(error);
  });

// 使用async/await
async function asyncFunction() {
  try {
    let result = await doSomethingAsync();
    processTheResult(result);
  } catch (error) {
    handleTheError(error);
  }
}
```

#### 7. 不要忽略错误

不要忽略捕获到的错误，即使你认为它们不重要。未处理的错误可能会导致更严重的问题。

#### 8. 使用错误边界（Error Boundaries）

在前端框架（如 React）中，使用错误边界来捕获子组件树中的 JavaScript 错误，防止整个应用崩溃。

```js
class ErrorBoundary extends React.Component {
  // ...
  componentDidCatch(error, info) {
    // 错误处理逻辑
  }
  // ...
}
```

#### 9. 利用工具和策略进行错误监控

使用错误监控工具（如 Sentry, Rollbar 等）来记录和分析生产环境中的错误，以便及时响应和修复。

#### 10. 进行单元测试和集成测试

通过编写单元测试和集成测试来确保代码的健壮性，并在代码更改后及时发现新引入的错误。

## 默认参数

### 1. 默认参数的概念

在 JavaScript 中，函数参数可以有默认值。如果在调用函数时未提供参数，或者参数值为 undefined，则会使用默认值。

```js
function greet(name = 'Guest') {
  return `Hello, ${name}!`;
}

greet('Alice'); // 输出: "Hello, Alice!"
greet(); // 输出: "Hello, Guest!"
greet(undefined); // 输出: "Hello, Guest!"
```

### 2. 默认参数的表达式

默认参数可以是任何有效的 JavaScript 表达式，包括函数调用。

```js
function getDefaultAge() {
  return 18;
}

function register(name, age = getDefaultAge()) {
  // ...
}
```

### 3. 参数解构与默认值

当函数参数是一个对象或数组时，可以在函数签名中结合解构和默认参数。

```js
function setOptions({ url, method = 'GET' } = {}) {
  console.log(`URL: ${url}, Method: ${method}`);
}

setOptions({ url: 'https://example.com' }); // 输出: "URL: https://example.com, Method: GET"
setOptions(); // 输出: "URL: undefined, Method: GET"
```

### 4. 默认参数的作用域和暂时性死区

函数参数创建了自己的作用域和暂时性死区，这意味着不能在默认参数表达式中访问后面的参数。

```js
function example(a = b, b) {
  // ...
}
// 抛出错误：在初始化 'a' 时不能访问 'b'
```

## 解构赋值

### 1. 解构赋值的概念

解构赋值允许你直接从数组或对象中提取值，并将它们赋给不同的变量。

数组解构
从数组中提取值，按照数组元素的位置来指定要提取的值。

```js
let [a, b] = [1, 2];
console.log(a); // 输出: 1
console.log(b); // 输出: 2
```

对象解构
从对象中提取值，根据对象的属性名来指定要提取的值。

```js
let { x, y } = { x: 10, y: 20 };
console.log(x); // 输出: 10
console.log(y); // 输出: 20
```

2. 解构赋值的默认值
   解构赋值可以设置默认值，当解构的值是 undefined 时将使用默认值。

```js
let { a = 10, b = 5 } = { a: 3 };
console.log(a); // 输出: 3
console.log(b); // 输出: 5
```

3. 解构赋值与函数参数
   解构赋值常用于函数参数中，使得函数参数更加灵活和可读。

```js
function drawChart({
  size = 'big',
  coords = { x: 0, y: 0 },
  radius = 25,
} = {}) {
  console.log(size, coords, radius);
  // ...
}

drawChart({
  coords: { x: 18, y: 30 },
  radius: 30,
});
```

### 4. 嵌套解构

解构赋值可以嵌套，允许从嵌套的对象或数组中提取值。

```js
let options = {
  size: {
    width: 100,
    height: 200,
  },
  items: ['Cake', 'Donut'],
  extra: true,
};

let {
  size: { width, height },
  items: [item1, item2],
  extra,
} = options;

console.log(width, height, item1, item2, extra);
```

### 5. 解构赋值的其它用途

- 交换变量的值
- 从函数返回多个值
- 函数参数定义
- 解析函数参数中的对象属性
