# 数组

## 数组创建和基本操作

JavaScript 中的数组是一种用于存储多个值的全局对象。

```js
let arr = [1, 2, 3]; // 创建一个数组
arr.push(4); // 向数组末尾添加一个元素
arr.pop(); // 移除数组最后一个元素
arr.unshift(0); // 向数组开头添加一个元素
arr.shift(); // 移除数组第一个元素
```

## 数组的迭代方法

JavaScript 数组提供了多种迭代方法，用于遍历数组并对数组的每个元素执行操作。

### forEach

对数组的每个元素执行提供的函数。

```js
arr.forEach(function (element, index, array) {
  console.log(element); // 输出当前元素
});
```

### map

创建一个新数组，其结果是该数组中的每个元素调用一次提供的函数后的返回值。

```js
let squares = arr.map(function (element) {
  return element * element;
});
```

### filter

创建一个新数组，包含所有通过所提供函数测试的元素。

```js
let evens = arr.filter(function (element) {
  return element % 2 === 0;
});
```

### reduce

对数组中的每个元素执行一个由您提供的 reducer 函数（升序执行），将其结果汇总为单个返回值。

```js
let sum = arr.reduce(function (accumulator, currentValue) {
  return accumulator + currentValue;
}, 0); // 初始值为0
```

### some

测试数组中是不是至少有 1 个元素通过了被提供的函数测试。

```js
let hasNegativeNumbers = arr.some(function (element) {
  return element < 0;
});
```

### every

测试一个数组内的所有元素是否都能通过某个指定函数的测试。

```js
let allPositive = arr.every(function (element) {
  return element > 0;
});
```

### find

返回数组中满足提供的测试函数的第一个元素的值，否则返回 undefined。

```js
let firstEven = arr.find(function (element) {
  return element % 2 === 0;
});
```

### findIndex

返回数组中满足提供的测试函数的第一个元素的索引，否则返回-1。

```js
let firstEvenIndex = arr.findIndex(function (element) {
  return element % 2 === 0;
});
```

### splice

splice() 方法通过删除现有元素和/或添加新元素来更改一个数组的内容。

#### 删除元素

从数组中删除元素，可以指定要删除的起始索引和数量。

```js
let myArray = ['a', 'b', 'c', 'd', 'e'];
myArray.splice(2, 1); // 从索引2开始删除1个元素
console.log(myArray); // 输出: ['a', 'b', 'd', 'e']
```

#### 添加元素

在数组中添加新元素，而不删除任何元素。

```js
myArray.splice(2, 0, 'x'); // 在索引2的位置添加元素'x'
console.log(myArray); // 输出: ['a', 'b', 'x', 'd', 'e']
```

#### 替换元素

删除元素的同时添加新元素，实现替换效果。

```js
myArray.splice(2, 1, 'c'); // 删除索引2的元素，并添加新元素'c'
console.log(myArray); // 输出: ['a', 'b', 'c', 'd', 'e']
```

### reverse

可以使用 reverse()方法来反转数组中元素的顺序，这个方法会改变原数组。

```js
let array = [1, 2, 3, 4, 5];
array.reverse();
console.log(array); // 输出: [5, 4, 3, 2, 1]
```

### sort

sort()方法用于对数组的元素进行排序。默认情况下，sort()方法按照字符串 Unicode 码点进行排序。

```js
let array = [2, 1, 5, 4, 3];
array.sort();
console.log(array); // 输出: [1, 2, 3, 4, 5]
```

如果需要按照其他标准排序，比如数字大小，可以传递一个比较函数给 sort()方法。

```js
let numberArray = [2, 1, 5, 4, 3];
numberArray.sort((a, b) => a - b);
console.log(numberArray); // 输出: [1, 2, 3, 4, 5]
```

#### 数字排序

当排序数字时，默认的排序可能不会按照数值大小进行排序，因为 sort()默认将元素作为字符串处理。为了按照数值大小排序，需要提供一个比较函数。

```js
let numbers = [10, 5, 40, 25, 1000, 1];
numbers.sort((a, b) => a - b);
console.log(numbers); // 输出: [1, 5, 10, 25, 40, 1000]
```

#### 字符串排序

对于字符串数组，如果需要按照字母表顺序排序，可以直接使用 sort()。

```js
let stringArray = ['banana', 'apple', 'cherry'];
stringArray.sort();
console.log(stringArray); // 输出: ['apple', 'banana', 'cherry']
```

#### 注意事项

- sort()和 reverse()方法都会直接修改原数组，而不是返回一个新的数组。
- 如果需要保持原数组不变，可以先使用 slice()方法复制原数组，然后在副本上调用 sort()或 reverse()。

```js
let array = [1, 2, 3, 4, 5];
let reversedArray = array.slice().reverse();
let sortedArray = array.slice().sort((a, b) => a - b);
console.log(array); // 原数组保持不变: [1, 2, 3, 4, 5]
console.log(reversedArray); // 输出: [5, 4, 3, 2, 1]
console.log(sortedArray); // 输出: [1, 2, 3, 4, 5]
```

#### 结论

使用 reverse()和 sort()方法可以方便地对数组进行反转和排序。记住这两个方法都会改变原数组，如果需要保留原数组不变，应先创建数组的副本。排序时，特别是数字排序，需要提供正确的比较函数以确保按照预期的顺序排序。

## 数组的最佳实践

### 1. 使用合适的方法进行数组迭代

避免使用传统的 for 循环，而是使用 forEach, map, filter, reduce 等高阶函数进行数组操作。这些方法更加简洁，并且易于理解。

```js
// 不推荐
for (let i = 0; i < array.length; i++) {
  console.log(array[i]);
}

// 推荐
array.forEach((item) => console.log(item));
```

### 2. 利用解构赋值

使用 ES6 提供的解构赋值来让代码更简洁。

```js
const array = [1, 2, 3, 4];
const [first, second] = array;
console.log(first, second); // 输出：1 2
```

### 3. 使用 spread 操作符进行数组合并

使用 spread 操作符...来合并数组，而不是 concat 方法，因为它更简洁且易读。

```js
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const mergedArray = [...array1, ...array2];
console.log(mergedArray); // 输出：[1, 2, 3, 4, 5, 6]
```

### 4. 使用 Array.from 或扩展运算符...转换类数组对象

需要将类数组对象（如函数内的 arguments 对象或 DOM 对象集合）转换为真正的数组时，使用 Array.from 或...。

```js
function example() {
  const args = Array.from(arguments);
  // 或者
  const args = [...arguments];
}
```

### 5. 使用 find 和 findIndex 查找元素

当需要查找数组中满足特定条件的元素时，使用 find 和 findIndex 方法，这比使用 filter 方法后取第一个元素更高效。

```js
const array = [{ id: 1 }, { id: 2 }, { id: 3 }];
const item = array.find((item) => item.id === 2);
console.log(item); // 输出：{ id: 2 }
```

### 6. 利用 includes 检查元素存在

使用 includes 方法来检查数组中是否包含某个元素，它返回一个布尔值，比手动遍历数组更方便。

```js
const array = [1, 2, 3];
console.log(array.includes(2)); // 输出：true
```

### 7. 不要直接修改数组

避免直接修改数组，例如使用 push、pop、shift、unshift 和 splice 等改变原数组的方法。尽可能使用返回新数组的方法，如 slice、concat 等，以保持函数的纯净性。

### 8. 优化大数组的操作

对于大数组，考虑性能优化。例如，如果只需要查找数组中是否存在某个元素，使用 some 而不是 includes，因为 some 会在找到第一个匹配项时立即停止迭代。

### 9. 数组深拷贝

在 JavaScript 中，深拷贝一个数组意味着创建一个新数组，并且复制原数组中的所有元素的值，如果元素本身也是一个对象或数组，则递归地复制这些对象或数组，以确保原数组和新数组之间没有引用关系。

1. 使用 JSON.stringify 和 JSON.parse
   这种方法适用于数组中不包含函数、undefined 或循环引用的情况。

```js
const originalArray = [{ a: 1 }, { b: 2 }];
const deepCopiedArray = JSON.parse(JSON.stringify(originalArray));
```

2. 使用递归函数手动实现深拷贝
   对于包含复杂对象（如函数、特殊对象等）的数组，可以使用递归函数来实现深拷贝。

```js
function deepCopy(obj) {
  if (typeof obj !== 'object' || obj === null) {
    return obj; // 如果是基本数据类型或null，则直接返回
  }

  let copy;

  if (Array.isArray(obj)) {
    copy = []; // 如果是数组，则创建一个空数组
    for (let i = 0; i < obj.length; i++) {
      copy[i] = deepCopy(obj[i]); // 递归复制每个元素
    }
  } else {
    copy = {}; // 如果是对象，则创建一个空对象
    for (let key in obj) {
      if (obj.hasOwnProperty(key)) {
        copy[key] = deepCopy(obj[key]); // 递归复制每个属性
      }
    }
  }

  return copy;
}

const originalArray = [{ a: 1 }, { b: 2 }];
const deepCopiedArray = deepCopy(originalArray);
```

3. 使用第三方库
   比如使用 Lodash 库的\_.cloneDeep 方法可以很方便地进行深拷贝。

```js
// 需要先安装lodash库
import _ from 'lodash';

const originalArray = [{ a: 1 }, { b: 2 }];
const deepCopiedArray = _.cloneDeep(originalArray);
```

结论
选择深拷贝的方法时需要根据数组的结构和内容来决定。如果数组结构简单，不含有特殊对象，可以使用 JSON.stringify 和 JSON.parse 的组合。对于复杂或特殊对象，建议使用递归函数或者第三方库来保证深拷贝的正确性和完整性。
