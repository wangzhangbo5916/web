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

## 原型与继承

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
    clone.sayHi = function() { // 以某种方式来增强这个对象
        console.log('hi');
    };
    return clone; // 返回这个对象
}

var person = {
    name: 'Nicholas',
    friends: ['Shelby', 'Court', 'Van']
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

Parent.prototype.sayName = function() {
    return this.name;
};

function Child(name, age) {
    Parent.call(this, name);
    this.age = age;
}

inheritPrototype(Child, Parent);

Child.prototype.sayAge = function() {
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

### 原型的好处与问题

代码复用：通过原型可以很容易地实现属性和方法的共享，节省内存。
性能优化：原型链可以减少函数的重复创建，提高代码执行效率。
潜在问题：原型链的错误使用可能会导致意外的继承关系，尤其是在使用引用类型值（如数组、对象）作为原型属性时，这些属性会在所有实例间共享，可能会导致意外的副作用。
