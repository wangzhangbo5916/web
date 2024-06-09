# 对象和原型

## 对象（Object）

- 基本概念：在 JavaScript 中，对象是一个包含属性和方法的集合。属性是对象的变量，而方法是能够在对象上执行的函数。
- 创建对象：对象可以通过字面量方式创建，使用 new Object()，或者通过构造函数创建。

  ```js
  // 字面量方式
  let person = {
    name: 'Alice',
    age: 25,
    greet: function () {
      console.log('Hello, ' + this.name);
    },
  };

  // 构造函数方式
  function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function () {
      console.log('Hello, ' + this.name);
    };
  }
  let person1 = new Person('Bob', 30);
  ```

- 属性和方法访问：可以通过点记法（.）或者方括号记法（[]）访问对象的属性和方法。

## 原型（Prototype）

- 原型的定义：原型是 JavaScript 中用于实现对象继承的一种机制。每个 JavaScript 对象在创建时都会与另一个对象关联起来，这个对象就是我们所说的原型，每个对象都会从原型“继承”属性和方法。
- 原型链：对象的原型本身也是一个对象，因此它也有自己的原型，这样一层层向上直到一个对象的原型为 null 为止，这个关系链就是原型链。

  ```js
  let animal = {
    isAlive: true,
  };

  function Mammal(name) {
    this.name = name;
  }

  Mammal.prototype = animal; // 设置Mammal的原型为animal

  let cat = new Mammal('Whiskers');
  console.log(cat.isAlive); // true，cat继承了animal的isAlive属性
  ```

- 原型属性：在 JavaScript 中，每个函数都有一个特殊的属性叫做 prototype，这个属性是一个对象，当这个函数作为构造函数创建对象时，该 prototype 属性会成为新对象的原型。
- \_\_proto\_\_属性：在对象中有一个内部属性叫做\_\_proto\_\_（现在已被标准化为 Object.getPrototypeOf()），它指向该对象的原型。

### 每个函数都有prototype属性，还是只有构造函数prototype属性？

在JavaScript中，每个函数都有一个prototype属性，不仅仅是构造函数。这个属性是一个对象，它包含了可以由特定类型的所有实例继承的属性和方法。当函数作为构造函数使用（即通过new关键字调用）时，这个prototype对象就会成为由该构造函数创建的所有对象实例的原型。

当创建一个函数时，JavaScript引擎会为这个函数添加一个prototype属性，其初始值是一个只有一个constructor属性的对象，且该constructor属性指向函数本身。例如：

```js
function MyConstructor() {
  // 构造函数中的代码
}

console.log(MyConstructor.prototype.constructor === MyConstructor); // true
```

这意味着你可以通过prototype属性向所有实例添加属性和方法：

```js
MyConstructor.prototype.myMethod = function() {
  // 所有MyConstructor实例都将继承这个方法
};
```

然而，通常只有当函数被意图用作构造函数（即用来创建新的对象实例）时，修改它的prototype属性才有实际意义。普通函数的prototype属性在函数调用时并不会有特别的作用，因为普通函数调用并不涉及实例化对象，它们的prototype属性通常不会被访问。

## 原型与继承

继承机制：在 JavaScript 中，继承是通过原型链实现的。当尝试访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，解释器就会去其原型对象中寻找，如果原型对象中也没有，再去原型的原型中寻找，直到找到为止或者到达原型链的末端（Object.prototype 的原型是 null）。
原型的动态性：原型是动态的，给原型添加属性或方法，所有基于该原型创建的对象都会立即拥有这个属性或方法。

### 原型的好处与问题

代码复用：通过原型可以很容易地实现属性和方法的共享，节省内存。
性能优化：原型链可以减少函数的重复创建，提高代码执行效率。
潜在问题：原型链的错误使用可能会导致意外的继承关系，尤其是在使用引用类型值（如数组、对象）作为原型属性时，这些属性会在所有实例间共享，可能会导致意外的副作用。
