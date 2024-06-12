# 原型和类

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

  ```js
  // 点符号
  console.log(person.name); // John
  person.age = 31;

  // 方括号
  console.log(person['name']); // John
  person['age'] = 32;
  ```

- 遍历对象：使用 for...in 循环遍历对象的属性。

  ```js
  for (let key in person) {
    if (person.hasOwnProperty(key)) {
      console.log(`${key}: ${person[key]}`);
    }
  }
  ```

- 对象的原型链：每个对象都有一个原型，可以从中继承属性和方法。

  ```js
  Person.prototype.greet = function () {
    console.log(`Hello, my name is ${this.name}`);
  };
  anotherPerson.greet(); // Hello, my name is Jane
  ```

- 对象解构：用于从对象中提取属性，简化代码。

  ```js
  const { name, age } = person;
  console.log(name); // John
  console.log(age); // 32
  ```

- 展开运算符（Spread Operator）：用于复制对象。

  ```js
  const copiedPerson = { ...person };
  ```

- 对象冻结和密封：Object.freeze()和 Object.seal()防止对象被修改。

  ```js
  Object.freeze(person); // 不能添加、删除或修改属性
  Object.seal(anotherPerson); // 只能修改现有属性
  ```

- 动态属性名：使用计算属性名动态设置对象的键。

  ```js
  const dynamicKey = 'email';
  const user = {
    [dynamicKey]: 'john@example.com',
  };
  ```

### JavaScript 对象属性修饰符详解

1. writable
   writable 修饰符决定了属性的值是否可以被修改。当 writable 设置为 false 时，属性值不可被赋值运算符改变。

```js
let obj = {};

Object.defineProperty(obj, 'a', {
  value: 1,
  writable: false,
});

console.log(obj.a); // 输出: 1
obj.a = 2;
console.log(obj.a); // 输出: 1，因为writable为false
```

2. enumerable
   enumerable 修饰符决定了属性是否会在对象属性的枚举过程中被枚举。例如，在使用 for...in 循环或 Object.keys()方法时，只有 enumerable 为 true 的属性才会被包括。

```js
let obj = {};

Object.defineProperty(obj, 'b', {
  value: 2,
  enumerable: false,
});

for (let key in obj) {
  console.log(key); // 不会输出任何东西，因为b的enumerable为false
}

console.log(Object.keys(obj)); // 输出: []，因为b的enumerable为false
```

3. configurable
   configurable 修饰符决定了属性是否可以被删除，以及除了 writable 以外的属性描述符是否可以被修改。

```js
let obj = {};

Object.defineProperty(obj, 'c', {
  value: 3,
  configurable: false,
});

delete obj.c;
console.log(obj.c); // 输出: 3，因为configurable为false

// 尝试修改属性描述符将抛出错误
// Object.defineProperty(obj, 'c', { enumerable: true }); // TypeError
```

使用 Object.defineProperties 同时定义多个属性
你也可以使用 Object.defineProperties 方法同时定义多个属性。

```js
let obj = {};

Object.defineProperties(obj, {
  first: {
    value: 'first value',
    writable: true,
    enumerable: true,
    configurable: true,
  },
  second: {
    value: 'second value',
    writable: false,
    enumerable: false,
    configurable: false,
  },
});

console.log(obj.first); // 输出: first value
console.log(obj.second); // 输出: second value
```

获取属性的修饰符
使用 Object.getOwnPropertyDescriptor 方法可以获取指定属性的修饰符。

```js
let obj = { d: 4 };
Object.defineProperty(obj, 'd', {
  value: 4,
  writable: true,
  enumerable: true,
  configurable: true,
});

let descriptor = Object.getOwnPropertyDescriptor(obj, 'd');
console.log(descriptor);
/* 输出:
{
  value: 4,
  writable: true,
  enumerable: true,
  configurable: true
}
*/
```

**注意事项:**

- 一旦属性设置为不可配置（configurable: false），就不能再把它变回可配置的了，也不能修改它的 enumerable 和 writable 属性。
- Object.freeze(obj)方法可以使得 obj 的所有属性变为不可写（writable: false）和不可配置（configurable: false）。
- Object.seal(obj)方法可以阻止新属性的添加，并将所有现有属性设置为不可配置（configurable: false），但现有属性的 writable 属性保持不变。

### JavaScript 对象属性的 Setter 和 Getter 详解

在 JavaScript 中，对象属性可以包括两种特殊的方法：setter 和 getter。这些方法允许你在读取（getter）和设置（setter）属性值时执行更复杂的操作。

Getter（访问器）
Getter 方法是一个可以返回属性值的函数。当你访问一个属性时，如果该属性有对应的 getter 方法，这个方法就会被调用，并返回它的返回值作为属性访问的结果。

```js
let obj = {
  a: 1,
  b: 2,
  get sum() {
    return this.a + this.b;
  },
};

console.log(obj.sum); // 输出: 3
```

在这个例子中，sum 是一个 getter，当访问 obj.sum 时，实际上是在执行 sum 方法，并返回 a 和 b 的和。

Setter（设置器）
Setter 方法是一个当属性值被设置时会被调用的函数。这个方法接受一个参数，即被赋予的新值，并可以基于这个新值执行操作。

```js
let obj = {
  a: 1,
  b: 2,
  set updateA(value) {
    this.a = value;
    console.log(`a is now set to ${value}`);
  },
};

obj.updateA = 10; // 输出: a is now set to 10
console.log(obj.a); // 输出: 10
```

在这个例子中，updateA 是一个 setter，当我们尝试设置 obj.updateA 的值时，实际上是在调用 updateA 这个方法，并将新值传递给它。

使用 Object.defineProperty 定义 Getter 和 Setter
你也可以使用 Object.defineProperty 方法来定义 getter 和 setter。

```js
let obj = {
  a: 1,
  b: 2,
};

Object.defineProperty(obj, 'sum', {
  get: function () {
    return this.a + this.b;
  },
  set: function (value) {
    this.a = value / 2;
    this.b = value / 2;
  },
});

console.log(obj.sum); // 输出: 3
obj.sum = 20;
console.log(obj.a); // 输出: 10
console.log(obj.b); // 输出: 10
```

在这个例子中，我们定义了 sum 属性的 getter 和 setter，getter 返回 a 和 b 的和，而 setter 则将传入的值平均分配给 a 和 b。

**注意事项:**

- Getter 和 Setter 可以帮助你实现数据的封装和验证。
- 使用 Getter 和 Setter 可以在不破坏现有代码结构的情况下，添加额外的逻辑到属性的存取过程中。
- 如果只定义了 getter 而没有定义 setter，那么尝试设置该属性的值时不会有任何效果，但不会报错。
- 如果只定义了 setter 而没有定义 getter，那么尝试读取该属性的值时会得到 undefined。

#### 使用场景

1. 数据封装和验证
   使用 getter 和 setter 可以封装内部数据结构，同时提供一个接口来控制属性的访问，这对于数据验证非常有用。

```js
class User {
  constructor(name) {
    this.setName(name);
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (value.length < 4) {
      console.log('Name is too short.');
      return;
    }
    this._name = value;
  }
}

let user = new User('John');
console.log(user.name); // John
user.name = 'Al'; // Name is too short.
```

在这个例子中，通过 setter 方法确保了用户名不会太短。

2. 延迟计算
   如果一个属性的值需要通过复杂计算得出，并且不经常改变，你可以使用 getter 来实现延迟计算，这样只有在实际需要值的时候才进行计算。

```js
let obj = {
  get expensiveOperation() {
    if (!this._cachedValue) {
      console.log('Performing expensive calculation...');
      this._cachedValue = performExpensiveCalculation();
    }
    return this._cachedValue;
  },
};
```

在这个例子中，只有在第一次访问 expensiveOperation 属性时才会执行耗时的计算，并缓存结果。

3. 依赖跟踪和通知
   当对象的某些属性依赖于其他属性时，可以使用 setter 来自动更新依赖属性，或者通知其他系统组件属性已变更。

```js
class Temperature {
  constructor(celsius) {
    this.celsius = celsius;
  }

  get fahrenheit() {
    return (this.celsius * 9) / 5 + 32;
  }

  set fahrenheit(value) {
    this.celsius = ((value - 32) * 5) / 9;
  }
}

let temp = new Temperature(0);
console.log(temp.fahrenheit); // 32
temp.fahrenheit = 212;
console.log(temp.celsius); // 100
```

在这个例子中，摄氏度和华氏度之间的转换通过 getter 和 setter 自动处理。

4. 控制属性的可写性和可枚举性
   通过使用 Object.defineProperty，你可以更精细地控制属性的特性，比如将属性设置为只读或不可枚举。

```js
let obj = {};
Object.defineProperty(obj, 'readOnly', {
  value: 42,
  writable: false,
});

console.log(obj.readOnly); // 42
obj.readOnly = 100; // 没有效果，因为属性是只读的
```

在这个例子中，readOnly 属性被设置为只读。

5. 保持接口的一致性
   即使属性的内部实现改变，通过 getter 和 setter 也可以保持对象对外的接口不变，这有助于维护和扩展代码。

```js
class Circle {
  constructor(radius) {
    this.radius = radius;
  }

  get diameter() {
    return this.radius * 2;
  }

  set diameter(diameter) {
    this.radius = diameter / 2;
  }
}

let circle = new Circle(1);
console.log(circle.diameter); // 2
circle.diameter = 4;
console.log(circle.radius); // 2
```

在这个例子中，即使圆的表示方式从直径变为半径，对于使用 diameter 的代码来说接口依然是一致的。

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

### 每个函数都有 prototype 属性，还是只有构造函数 prototype 属性？

在 JavaScript 中，每个函数都有一个 prototype 属性，不仅仅是构造函数。这个属性是一个对象，它包含了可以由特定类型的所有实例继承的属性和方法。当函数作为构造函数使用（即通过 new 关键字调用）时，这个 prototype 对象就会成为由该构造函数创建的所有对象实例的原型。

当创建一个函数时，JavaScript 引擎会为这个函数添加一个 prototype 属性，其初始值是一个只有一个 constructor 属性的对象，且该 constructor 属性指向函数本身。例如：

```js
function MyConstructor() {
  // 构造函数中的代码
}

console.log(MyConstructor.prototype.constructor === MyConstructor); // true
```

这意味着你可以通过 prototype 属性向所有实例添加属性和方法：

```js
MyConstructor.prototype.myMethod = function () {
  // 所有MyConstructor实例都将继承这个方法
};
```

然而，通常只有当函数被意图用作构造函数（即用来创建新的对象实例）时，修改它的 prototype 属性才有实际意义。普通函数的 prototype 属性在函数调用时并不会有特别的作用，因为普通函数调用并不涉及实例化对象，它们的 prototype 属性通常不会被访问。

### 原型的好处与问题

代码复用：通过原型可以很容易地实现属性和方法的共享，节省内存。
性能优化：原型链可以减少函数的重复创建，提高代码执行效率。
潜在问题：原型链的错误使用可能会导致意外的继承关系，尤其是在使用引用类型值（如数组、对象）作为原型属性时，这些属性会在所有实例间共享，可能会导致意外的副作用。

## 继承

继承机制：在 JavaScript 中，继承是通过原型链实现的。当尝试访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，解释器就会去其原型对象中寻找，如果原型对象中也没有，再去原型的原型中寻找，直到找到为止或者到达原型链的末端（Object.prototype 的原型是 null）。
原型的动态性：原型是动态的，给原型添加属性或方法，所有基于该原型创建的对象都会立即拥有这个属性或方法。

### 继承常见的模式

#### 1. 原型链继承

代码示例:

```js
function Parent() {
  this.parentProperty = true;
}

Parent.prototype.getParentProperty = function () {
  return this.parentProperty;
};

function Child() {
  this.childProperty = false;
}

// 继承Parent
Child.prototype = new Parent();

var child = new Child();
console.log(child.getParentProperty()); // true
```

**优点**:

- 简单易懂。
- 原型链上的属性和方法可以被所有实例共享。

**缺点**:

- 来自原型对象的所有属性被所有实例共享，这可能会导致意外的结果。
- 创建子类实例时无法向父类构造函数传递参数。

**使用场景**:
当你想要多个对象共享属性或方法时。

#### 2. 借用构造函数继承（经典继承）

代码示例:

```js
function Parent(name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
}

function Child(name) {
  Parent.call(this, name);
}

var child1 = new Child('child1');
child1.colors.push('yellow');

var child2 = new Child('child2');
console.log(child1.colors); // ['red', 'blue', 'green', 'yellow']
console.log(child2.colors); // ['red', 'blue', 'green']
```

**优点**:

- 可以在子类构造函数中向父类构造函数传递参数。
- 避免了引用类型的属性被所有实例共享。

**缺点**:

- 方法都在构造函数中定义，每次创建实例都会创建一遍方法。

**使用场景**:

- 当你需要包含父类中的复杂对象或者需要向父类构造函数传递参数时。

#### 3. 组合继承

代码示例:

```js
function Parent(name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.sayName = function () {
  return this.name;
};

function Child(name, age) {
  Parent.call(this, name); // 第二次调用Parent()
  this.age = age;
}

Child.prototype = new Parent(); // 第一次调用Parent()
Child.prototype.constructor = Child;
Child.prototype.sayAge = function () {
  return this.age;
};

var child = new Child('child', 10);
console.log(child.sayName()); // 'child'
console.log(child.sayAge()); // 10
```

**优点**:

- 可以向父类构造函数传递参数。
- 避免了引用类型的属性被所有实例共享。
- 原型链上的方法可以被多个实例共享。

**缺点**:

- 父类构造函数会被调用两次。

**使用场景**:

- 是 JavaScript 中最常用的继承模式，适用于需要大量实例的情况。

#### 4. 原型式继承

代码示例:

```js
var person = {
  name: 'Nicholas',
  friends: ['Shelby', 'Court', 'Van'],
};

var anotherPerson = Object.create(person);
anotherPerson.name = 'Greg';
anotherPerson.friends.push('Rob');

var yetAnotherPerson = Object.create(person);
yetAnotherPerson.name = 'Linda';
yetAnotherPerson.friends.push('Barbie');

console.log(person.friends); // "Shelby,Court,Van,Rob,Barbie"
```

**优点**:

- 适合不需要单独创建构造函数，但仍然需要在多个对象间共享信息的场景。

**缺点**:

- 包含引用类型值的属性始终会共享相应的值，这点类似于原型链继承。

**使用场景**:

- 当你想要一个对象作为另一个对象的基础时。

#### 5. 寄生式继承

代码示例:

```js
function createAnother(original) {
  var clone = Object.create(original); // 通过调用函数创建一个新对象
  clone.sayHi = function () {
    // 以某种方式来增强这个对象
    console.log('hi');
  };
  return clone; // 返回这个对象
}

var person = {
  name: 'Nicholas',
  friends: ['Shelby', 'Court', 'Van'],
};

var anotherPerson = createAnother(person);
anotherPerson.sayHi(); // 'hi'
```

**优点**:

- 可以在不影响原对象的情况下，对继承的对象进行增强。

**缺点**:

- 和借用构造函数模式一样，每次创建对象都会创建一遍方法。

**使用场景**:

- 主要适用于主要关注对象而不是类型和构造函数的场景。

#### 6. 寄生组合式继承

代码示例:

```js
function inheritPrototype(childConstructor, parentConstructor) {
  var prototype = Object.create(parentConstructor.prototype); // 创建对象
  prototype.constructor = childConstructor; // 增强对象
  childConstructor.prototype = prototype; // 指定对象
}

function Parent(name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.sayName = function () {
  return this.name;
};

function Child(name, age) {
  Parent.call(this, name);
  this.age = age;
}

inheritPrototype(Child, Parent);

Child.prototype.sayAge = function () {
  return this.age;
};

var child = new Child('child', 10);
console.log(child.sayName()); // 'child'
console.log(child.sayAge()); // 10
```

**优点**:

- 解决了父类构造函数被调用两次的问题。
- 原型链保持不变，因此原型链上的方法不会被重新定义。

**缺点**:

- 实现较为复杂。

**使用场景**:

- 寄生组合式继承是引用类型最理想的继承范式，适用于需要大量代码复用的情况。

#### 实际开发中继承模式的应用

在实际开发中，选择正确的继承模式取决于特定的需求和场景。通常，寄生组合式继承被认为是最有效的继承模式，因为它只调用一次父类构造函数，并且原型链保持不变。

- 对于简单的继承关系，原型链继承或原型式继承可能就足够了。
- 当需要在子类中向父类构造函数传递参数时，借用构造函数继承或组合继承是更好的选择。
- 如果需要大量创建对象，并且对象之间有大量共享的方法，组合继承或寄生组合式继承可能是更合适的选择。
- 当不需要关心类型和构造函数，只需要一个基础对象时，寄生式继承或原型式继承是一个不错的选择。

## Class

### Class 的基础知识

#### 1. Class 基础语法

声明类：

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}!`);
  }
}
```

构造函数：

```js
// 使用构造函数初始化实例属性
constructor(name) {
  this.name = name;
}
```

方法定义：

```js
// 方法不需要function关键字
greet() {
  console.log(`Hello, my name is ${this.name}!`);
}
```

#### 2. 静态方法和属性

静态方法：

```js
class Person {
  static species() {
    return 'Homo Sapiens';
  }
}

console.log(Person.species()); // 直接通过类调用
```

静态属性（目前处于提案阶段，可能需要编译器支持）：

```js
class Person {
  static species = 'Homo Sapiens';
}
```

#### 3. Getter 和 Setter

getter：

```js
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}
```

setter：

```js
set fullName(name) {
  [this.firstName, this.lastName] = name.split(' ');
}
```

#### 4. 私有方法和属性

私有属性：

```js
class Person {
  #age = 30; // 私有属性

  #calculateAge() {
    // 私有方法
    // ...
  }
}
```

#### 5. 实例属性的新写法

属性声明：

```js
class Person {
  name = 'Unnamed'; // 实例属性
}
```

#### 6. 静态属性的新写法

属性声明：

```js
class Person {
  static planet = 'Earth'; // 静态属性
}
```

#### 7. 类的继承与派生

派生类：

```js
class Developer extends Person {
  constructor(name, skills) {
    super(name);
    this.skills = skills;
  }
}
```

#### 8. Mixin 模式

Mixin：

```js
let sayMixin = {
  say(phrase) {
    console.log(phrase);
  },
};

class Dog {
  constructor(name) {
    this.name = name;
  }
}

// 将sayMixin的方法混入Dog类中
Object.assign(Dog.prototype, sayMixin);

let dog = new Dog('Rex');
dog.say('Woof'); // Rex says: Woof
```

### 为什么说 class 是语法糖？

**class 被称为语法糖，是因为它提供了一种更加简洁和易于理解的语法来实现 JavaScript 已有的原型继承功能。它并没有改变 JavaScript 的继承机制，只是提供了一种新的写法。这使得面向对象编程在 JavaScript 中变得更加接近于其他传统的面向对象语言，同时保持了 JavaScript 原型继承的灵活性。**

- 基于原型的继承： JavaScript 是一种基于原型的语言，这意味着对象直接从其他对象继承。在引入 class 之前，开发者通过构造函数和原型链实现继承。class 关键字实际上是基于这种已存在的原型继承机制的一个语法结构。
- 语法简化： 使用 class 可以使得创建构造函数和处理原型链更加直观，它提供了一种类似于传统面向对象编程语言（如 Java 或 C++）的语法，但在底层仍然是使用函数和原型链实现的。
- 功能等效： 使用 class 创建的类实际上是函数的一个特殊类型，而类方法实际上是定义在该函数的 prototype 属性上的函数。这意味着使用 class 和使用构造函数加原型链的方式，在功能上是等效的。
- 便于理解和维护： 对于熟悉传统面向对象编程概念的开发者来说，class 提供了一种更易于理解和维护的代码结构。它隐藏了 JavaScript 中原型继承的复杂性，使得代码更加直观。

### class 的继承是基于那种继承模式实现的？

class 的继承在 JavaScript 中是**基于原型链（Prototype Chain）的继承模式实现**的。这是 JavaScript 中唯一的继承方式，它依赖于原型对象（prototype）来分享属性和方法。

- 原型对象： 每个 JavaScript 对象都有一个原型对象，从中继承方法和属性。在使用 class 关键字时，定义在类内部的方法都会被添加到类的原型对象上。
- 构造函数的原型属性： 当一个函数（构造函数）被创建时，JavaScript 会为这个函数创建一个 prototype 属性，指向函数的原型对象。
- 对象实例与原型链： 当使用 new 关键字创建一个对象实例时，这个实例内部的 [[Prototype]]（或 **proto**）属性会被赋值为构造函数的 prototype 属性。这样，实例就可以访问原型对象上的属性和方法。
- 继承的实现： 使用 extends 关键字创建的子类会继承父类的属性和方法。实际上，子类的原型对象会被设置为父类的一个实例，从而形成一个原型链，子类实例可以访问父类原型上的属性和方法。
- super 关键字的作用： 在子类中，super 关键字用于调用父类的构造函数和方法。这在原型链继承模式中对应于直接通过原型链访问父类的属性和方法。

### 类的最佳实践

#### 1. 使用构造函数初始化实例属性

构造函数是类的默认方法，通过构造函数可以确保实例被创建时具有所需的初始状态。

```js
class MyClass {
  constructor(value) {
    this.property = value;
  }
}
```

#### 2. 利用 get 和 set 访问器

使用 getter 和 setter 可以控制对类的实例属性的访问和赋值，可以添加额外的逻辑，例如验证或者处理。

```js
class MyClass {
  constructor(value) {
    this._property = value;
  }

  get property() {
    // 可以添加额外的获取逻辑
    return this._property;
  }

  set property(value) {
    // 可以添加额外的设置逻辑
    this._property = value;
  }
}
```

#### 3. 保持类的成员函数简短和专一

每个成员函数应该只做一件事情，这样代码更容易理解和维护。

```js
class MyClass {
  // ...

  doSomething() {
    // 函数体应该简短且只做一件事情
  }
}
```

#### 4. 使用静态方法和属性

静态方法和属性属于类本身而不是类的实例。它们通常用于实现与实例无关的功能。

```js
class MyClass {
  static staticProperty = 'some value';

  static staticMethod() {
    // 静态方法体
  }
}
```

#### 5. 保持类的封装性

避免直接操作类的内部属性，使用方法来访问和修改这些属性。

```js
class MyClass {
  #privateProperty; // 使用#来创建私有属性

  constructor(value) {
    this.#privateProperty = value;
  }

  publicMethod() {
    // 使用公共方法来操作私有属性
  }
}
```

#### 6. 继承应该清晰且合理

当创建基于另一个类的新类时，确保继承是有意义的，并且保持父类和子类之间的关系清晰。

```js
class ParentClass {
  // ...
}

class ChildClass extends ParentClass {
  // ...
}
```

#### 7. 避免使用类来管理全局状态

类应该用于实例化对象，不应该用来作为全局状态的容器。考虑使用模块或其他设计模式来管理全局状态。
