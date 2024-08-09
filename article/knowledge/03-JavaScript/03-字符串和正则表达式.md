# 字符串和正则表达式

## 字符串

### 字符串的方法

#### 1. 字符串搜索方法

**indexOf()**

- 使用场景: 查找字符串中某个子字符串首次出现的位置。
- 最佳实践: 当你需要确定字符串中是否包含某个子字符串时使用。

```js
let str = 'Hello, world!';
let index = str.indexOf('world'); // 返回 7
```

**includes()**

- 使用场景: 确定字符串中是否包含某个子字符串。
- 最佳实践: 当你不关心子字符串的位置，只想知道它是否存在时使用。

```js
let str = 'Hello, world!';
let hasWorld = str.includes('world'); // 返回 true
```

**search()**

- 使用场景: 使用正则表达式搜索字符串。
- 最佳实践: 当你需要更复杂的搜索模式时使用。

```js
let str = 'Hello, world!';
let index = str.search(/world/i); // 返回 7
```

**startsWith()**

startsWith() 方法用于判断当前字符串是否以另一个给定的子字符串开头，并根据判断结果返回 true 或 false。

使用场景

- 检查文件名的扩展名
- 根据用户输入自动补全
- 路由路径判断

```js
let str = 'Hello world!';
console.log(str.startsWith('Hello')); // true
console.log(str.startsWith('world')); // false
```

**endsWith()**

endsWith() 方法用于判断当前字符串是否以指定的子字符串结束，并根据判断结果返回 true 或 false。

使用场景

- 文件路径或 URL 的扩展名检查
- 模板字符串的结束标记判断

```js
let str = 'Hello world!';
console.log(str.endsWith('!')); // true
console.log(str.endsWith('world')); // false
```

#### 2. 字符串替换方法

**replace()**

- 使用场景: 替换字符串中的字符或子字符串。
- 最佳实践: 用于字符串的模式替换，可以接受字符串或正则表达式作为搜索模式。

```js
let str = 'Hello, world!';
let newStr = str.replace('world', 'JavaScript'); // 返回 "Hello, JavaScript!"
```

**matchAll()**

matchAll() 方法返回一个包含所有匹配正则表达式的结果及分组捕获组的迭代器。

使用场景

- 解析日志文件
- 提取信息，如邮箱、URL 等

```js
let regexp = /t(e)(st(\d?))/g;
let str = 'test1test2';

let array = [...str.matchAll(regexp)];

console.log(array[0]); // ["test1", "e", "st1", "1"]
console.log(array[1]); // ["test2", "e", "st2", "2"]
```

**replaceAll()**

replaceAll() 方法返回一个新字符串，这个新字符串是通过将原字符串中符合模式的所有子字符串替换为新子字符串后的结果。

使用场景

- 批量替换文本中的字符或单词
- 清洗数据，如删除多余空格

```js
let str = 'Hello world! World is wonderful.';
console.log(str.replaceAll('World', 'Earth')); // "Hello world! Earth is wonderful."
```

#### 3. 字符串切割方法

**split()**

- 使用场景: 将字符串切割成子字符串数组。
- 最佳实践: 当你需要处理或分析由特定分隔符分隔的数据时使用。

```js
let str = 'apple,banana,cherry';
let fruits = str.split(','); // 返回 ["apple", "banana", "cherry"]
```

#### 4. 字符串拼接方法

**concat()**

- 使用场景: 连接两个或多个字符串。
- 最佳实践: 当需要合并多个字符串时使用，但更推荐使用模板字符串。

```js
let hello = 'Hello, ';
let world = 'world!';
let greeting = hello.concat(world); // 返回 "Hello, world!"
```

**模板字符串 (Template Literals)**

- 使用场景: 插入变量或表达式到字符串中。
- 最佳实践: 创建包含变量或表达式的动态字符串时使用。

```js
let name = 'world';
let greeting = `Hello, ${name}!`; // 返回 "Hello, world!"
```

**padStart()**

padStart() 方法会用另一个字符串填充当前字符串（如果需要的话），从而确保当前字符串达到指定的长度。填充应用于当前字符串的开始。

使用场景

- 数字格式化，如日期和时间
- 生成固定长度的字符串 ID

```js
let str = '5';
console.log(str.padStart(2, '0')); // "05"
```

**padEnd()**

padEnd() 方法会用另一个字符串填充当前字符串（如果需要的话），从而确保当前字符串达到指定的长度。填充应用于当前字符串的末尾。

使用场景

- 对齐文本输出
- 在字符串后添加默认字符

```js
let str = '5';
console.log(str.padEnd(3, '0')); // "500"
```

#### 5. 字符串转换和处理方法

**toUpperCase()** 和 **toLowerCase()**

- 使用场景: 转换字符串的大小写。
- 最佳实践: 在不区分大小写的比较或搜索时使用。

```js
let str = 'Hello, World!';
let upperStr = str.toUpperCase(); // 返回 "HELLO, WORLD!"
let lowerStr = str.toLowerCase(); // 返回 "hello, world!"
```

**trim()**

- 使用场景: 去除字符串两端的空白字符。
- 最佳实践: 在处理用户输入时，去除不必要的空白。

```js
let str = '   Hello, world!   ';
let trimmedStr = str.trim(); // 返回 "Hello, world!"
```

**trimStart()**

trimStart() 方法会从字符串的开头删除空白字符。在旧环境中，这个方法可能被称为 trimLeft()。

使用场景

- 清理用户输入
- 删除字符串开头的空格或其他特定字符

```js
let str = '    Hello world!    ';
console.log(str.trimStart()); // "Hello world!    "
```

**trimEnd()**

trimEnd() 方法会从一个字符串的末尾删除空白字符。在旧环境中，这个方法可能被称为 trimRight()。

使用场景

- 清理用户输入
- 删除字符串末尾的空格或其他特定字符

```js
let str = '    Hello world!    ';
console.log(str.trimEnd()); // "    Hello world!"
```

#### 6. 字符串访问方法

**charAt()** 和 **charCodeAt()**

- 使用场景: 访问字符串中特定位置的字符或其编码。
- 最佳实践: 当你需要获取字符串中特定位置的字符时使用。

```js
let str = 'Hello, world!';
let char = str.charAt(0); // 返回 "H"
let charCode = str.charCodeAt(0); // 返回 72
```

**substring()** 和 **slice()**

- 使用场景: 提取字符串的一部分。
- 最佳实践: 当你需要基于位置提取子字符串时使用。

```js
let str = 'Hello, world!';
let substr = str.substring(7, 12); // 返回 "world"
let slicedStr = str.slice(7, -1); // 返回 "world"
```

### 模板字符串（Template Literals）

提供了一种更加方便的方法来创建包含变量和表达式的字符串。模板字符串使用反引号（``）标识，可以跨越多行，并且能够在字符串中嵌入变量或表达式，这些变量或表达式会在运行时被解析和替换。

#### 使用场景

##### 2.1 多行字符串

在 ES6 之前，创建多行字符串通常需要使用连接符（+）或者数组和 join 方法。模板字符串提供了一种更加简洁的方式来定义多行字符串。

```js
const multiLineString = `This is an example of
a multi-line string. No need for
concatenation.`;
```

##### 2.2 字符串插值

模板字符串允许嵌入变量和表达式，这使得拼接字符串变得更加容易和直观。

```js
const name = 'World';
const greeting = `Hello, ${name}!`;
```

##### 2.3 嵌套模板

模板字符串可以嵌套使用，这使得创建复杂的字符串模板变得简单。

```js
const isMember = true;
const discountMessage = `Your discount is ${isMember ? `${10}%` : '0%'}.`;
```

##### 2.4 标签模板

模板字符串可以与函数一起使用，称为标签模板。这允许你解析模板字符串的组成部分，并可以自定义字符串的创建过程。

```js
function tagFunction(strings, ...expressions) {
    // strings => ["Hello ", ", you have ", " items in your cart."]
    // expressions => [userName, itemCount]
    
    // 处理逻辑...
}

const userName = 'Alice';
const itemCount = 3;
const message = tagFunction`Hello ${userName}, you have ${itemCount} items in your cart.`;
```

#### 最佳实践

##### 3.1 避免复杂逻辑

虽然模板字符串可以包含表达式，但应避免在其中放置复杂逻辑。应该将复杂逻辑放在模板之外，并将结果赋值给一个变量，然后在模板中引用该变量。

```js
const price = 9.99;
const taxRate = 0.07;
const tax = (price * taxRate).toFixed(2);
const total = `Total: $${(parseFloat(price) + parseFloat(tax)).toFixed(2)}`;
```

##### 3.2 适用于动态字符串生成

当需要根据不同的条件动态生成字符串时，模板字符串是一个很好的选择。它可以让代码更加清晰和易于维护。

```js
const userName = 'Alice';
const items = 5;
const orderMessage = `Dear ${userName}, you have ${items} items in your cart.`;
```

##### 3.3 使用标签模板进行本地化

标签模板可以用于国际化和本地化，通过处理不同的本地化字符串和格式。

```js
function i18n(strings, ...values) {
  // 实现本地化逻辑
}

const greeting = i18n`Hello, ${name}! Today is ${new Date().toLocaleDateString()}.`;
```

## 正则表达式

### 正则表达式语法

- ^: 匹配输入的开始。
- $: 匹配输入的结束。
- .: 匹配任意单个字符，除了换行符。
- *: 匹配前一个表达式0次或多次。
- +: 匹配前一个表达式1次或多次。
- ?: 匹配前一个表达式0次或1次。
- {n}: 匹配确定的n次。
- {n,}: 匹配至少n次。
- {n,m}: 匹配至少n次，但不超过m次。
- \[abc]: 匹配任何包含括号内元素的字符。
- \[^abc]: 匹配任何不包含括号内元素的字符。
- (x|y): 匹配x或y。
- \d: 匹配一个数字字符。等价于[0-9]。
- \w: 匹配字母、数字或下划线。等价于[A-Za-z0-9_]。
- \s: 匹配任何空白字符，包括空格、制表符、换页符等等。
- \S: 大写的 S 是小写 s 的反义字符类，它用于匹配任何非空白字符。简而言之，\S 将匹配除了空格、制表符、换行符等空白字符以外的所有字符。
- \b: 匹配单词边界。

### 正则表达式命名捕获组

#### 什么是命名捕获组

命名捕获组是正则表达式的一个特性，允许你为组匹配创建一个名称。这样，你就可以通过名称来引用匹配的结果，而不仅仅是通过数字索引。这使得正则表达式更易读、易维护。

#### 创建命名捕获组

要创建命名捕获组，你需要使用以下语法：

```bash
(?<name>pattern)
```
这里name是你为捕获组指定的名称，pattern是你想要匹配的模式。

示例
假设有一个包含日期的字符串（格式为年-月-日），想要分别捕获年、月和日：

```js
let dateRegex = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
let match = dateRegex.exec('2024-06-17');

console.log(match.groups.year);  // 输出：2024
console.log(match.groups.month); // 输出：06
console.log(match.groups.day);   // 输出：17
console.log(/(\d{4})-(\d{2})-(\d{2})/.exec('2024-08-09')); // ['2024-08-09', '2024', '08', '09', index: 0, input: '2024-08-09', groups: undefined]
```

在这个例子中，我们创建了三个命名捕获组：year、month和day，分别用来匹配年、月和日。

#### 引用命名捕获组

在正则表达式外部，你可以通过match.groups.name的方式来引用捕获组。如果你在正则表达式内部需要引用捕获组（比如在替换操作中），你可以使用$\<name\>来引用：

```js
let dateRegex = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
let dateString = '2024-06-17';
let newDateString = dateString.replace(dateRegex, '$<day>/$<month>/$<year>');

console.log(newDateString); // 输出：17/06/2024
```
在这个替换操作中，我们使用$<name>语法来引用命名捕获组，并以不同的格式重新排列日期。

#### 兼容性

命名捕获组是ES2018引入的特性，因此它在现代浏览器和Node.js环境中得到支持。在使用之前，确保你的运行环境支持此特性。

### 正则表达式 dotAll 模式

#### 什么是 dotAll 模式

在正则表达式中，点（.）是一个特殊字符，用于匹配除换行符之外的任意单个字符。然而，在默认情况下，点字符不会匹配换行符（如 \n 或 \r）。dotAll 模式是一个正则表达式的标志，允许点（.）匹配包括换行符在内的任意单个字符。

#### 启用 dotAll 模式

要启用 dotAll 模式，你需要在正则表达式的末尾添加 s 标志：

```js
let regex = /./s;
```
这里的 s 就是 dotAll 模式的标志。

示例
没有启用 dotAll 模式时，点（.）不会匹配换行符：

```js
let regex = /a.b/;
console.log(regex.test('a\nb')); // 输出：false
```

启用 dotAll 模式后，点（.）会匹配包括换行符在内的任何字符：

```js
let regex = /a.b/s;
console.log(regex.test('a\nb')); // 输出：true
```

在这个例子中，我们使用 s 标志来确保点（.）可以匹配换行符。

#### 兼容性

dotAll 模式是在ES2018中引入的，因此它在现代浏览器和Node.js环境中得到支持。在使用之前，确保你的运行环境支持此标志。如果你需要在不支持 s 标志的环境中匹配任意字符，你可以使用字符类 [\s\S]，它将匹配包括换行符在内的任意字符：

```js
let regex = /a[\s\S]b/; // \s\S表示匹配任意字符
console.log(regex.test('a\nb')); // 输出：true
```

这种方法可以作为 dotAll 模式的替代方案，以实现跨环境的兼容性。

### RegExp Match Indices

RegExp Match Indices 是 JavaScript 正则表达式的一个特性，它提供了匹配结果的开始和结束位置索引信息。这个特性在 ES2021（ECMAScript 2021）中被正式加入到了 JavaScript 语言规范中。

要使用 Match Indices 特性，需要在正则表达式后面加上 d 标志。例如：

```js
const regex = /(\w+)\s(\w+)/d;
```

启用了 d 标志的正则表达式执行匹配操作后，不仅会返回匹配的字符串，还会返回每个捕获组的开始和结束位置索引。这使得开发者可以更精确地知道每个匹配和捕获组在原始字符串中的确切位置。

```js
const regex = /(\w+)\s(\w+)/d;
const str = 'Hello World';
const match = str.match(regex);

console.log(match.indices);
// 输出：[[0, 11], [0, 5], [6, 11]]
// 解释：整个匹配 "Hello World" 从索引 0 开始到索引 11 结束
// "Hello" 是第一个捕获组，从索引 0 开始到索引 5 结束
// "World" 是第二个捕获组，从索引 6 开始到索引 11 结束
```
Match Indices 特性使得在处理复杂的文本匹配时，可以更容易地进行字符串操作，例如替换、分割等，因为你可以准确地知道每个匹配的位置。这在进行文本编辑器开发、语法分析或其他需要精确位置信息的场景中非常有用。

注意事项
- 并非所有浏览器或环境都支持 d 标志，所以在使用前需要检查环境的兼容性。
- match.indices 仅在正则表达式匹配操作成功时才存在，如果没有匹配到任何内容，则该属性是 undefined。
