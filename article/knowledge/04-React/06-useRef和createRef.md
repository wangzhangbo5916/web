# useRef和createRef

## 为什么需要useRef？而不是采用在组件外定义变量的方式？

### 1. 生命周期管理

**React知道何时更新**: `useRef`与组件的生命周期紧密相关。它确保在每次组件重新渲染之间保持值的持久性。使用组件外定义的变量没有这样的关系，可能导致在重新渲染时失去数据的状态。

### 2. 一致性

**单一的引用**: `useRef`会返回同一个对象。即使组件重新渲染， `useRef`引用的对象在多个渲染之间保持一致。而如果在组件外定义变量，则每当组件更新时，引用会不再保持一致，因为处于函数作用域下的变量每次渲染时都可能会被重新创建。

### 3. 避免不必要的渲染

**不引发重新渲染**: 更新 `useRef`的值不会触发组件重新渲染。这使得它特别适合存储不会影响`UI`的值（例如 DOM 引用、定时器等）。而在组件外定义的变量一般与`React`的状态机制无关，可能会影响组件的渲染逻辑。

### 4. 响应式编程

**与 React 生态系统的兼容性**: `React`的`Hook`系统是为了迎合函数组件的运用，`useRef`符合这样的设计逻辑。组件外定义的变量不能利用`React`提供的特性，比如组件树更新和生命周期钩子。

### 5. 封装性和模块化

**提高可维护性**: 使用 `useRef`可以保持组件的封装性，内部逻辑不容易被外部代码干扰。组件外定义的变量容易被其他部分影响，从而导致不可预测的行为，这样会显著降低代码的可维护性。

### 6. 避免全局状态污染

**局限性**: 组件外定义的变量可能导致程序中的全局状态污染，这使得状态分布混乱，增大了代码的复杂性。`useRef`通过`Hook`实现了每个组件内部的状态管理。

示例对比

使用`useRef`:

```jsx
import React, { useRef } from 'react';

const MyComponent = () => {
  const countRef = useRef(0);

  const increment = () => {
    countRef.current++;
    console.log(countRef.current);
  };

  return <button onClick={increment}>Increment</button>;
};
```

在组件外定义变量:

```jsx
let count = 0;

const MyComponent = () => {
  const increment = () => {
    count++;
    console.log(count);
  };

  return <button onClick={increment}>Increment</button>;
};
```

## useRef实现原理

### useRef需要实现功能

1. **可变引用的创建**

`useRef`返回一个包含`current`属性的对象，这个对象用于保存可变值。


2. **持久性**

`useRef`创建的引用在组件的多次渲染中保持不变。只有在组件首次挂载时创建一次，随后都返回同一个对象。

3. **不触发重新渲染**

修改`current`的值不会导致组件重新渲染，这与`React`的`state`机制有所不同。

下面是一个简单的伪代码实现，帮助理解 useRef 的基本原理：

```jsx
// 伪代码实现
function useRef(initialValue) {
  // 检查是否是首次渲染
  let ref = internalAPI.getRef(); // 获取内部存储的 ref 对象

  // 如果 ref 还没有被创建
  if (ref === undefined) {
    ref = { current: initialValue }; // 初始化 current 属性
    internalAPI.setRef(ref); // 保存到内部状态
  }

  return ref; // 返回 ref 对象
}
```

### useRef为什么返回的是一个对象？

这个问题可以反向思考一下，如果useRef返回的不是一个对象，而是原始值，是否可行哪？

**答案是不可以的**，因为原始值不具有可变性，无法进行赋值。如下面代码所示：

```jsx
import React, { useRef } from 'react';

const FocusInput = () => {
  const countRef = useRef(0); // 变量0

  useEffect(() => {
    countRef = 1; // 错误，原始值无法赋值。
  }, [])
};

export default FocusInput;
```

那么增加一个更新函数可以吗？答案是不行，因为更新函数赋值完，组件不重新渲染，值是不变的。

```jsx
import React, { useRef } from 'react';

const FocusInput = () => {
  const [countRef, setCountRef] = useRef(0); // 变量0

  useEffect(() => {
    setCountRef(1);
    console.log(countRef); // countRef还是0
  }, [])
};

export default FocusInput;
```

此时你就会发现满足的条件的必须是引用类型，而引用类型最简单就是对象，其实使用Map或者Set也可以，只是使用起来没有对象简单。

**同时思考为什么对象必须有一个current属性**？其实很简单，一个固定的属性值读取值更简单更合适，而current语义更符合，因此使用一个拥有current属性的对象。可以对比下面两种方式：

```jsx
// 不论读取还是赋值只需要使用current属性
countRef.current = 1;
console.log(countRef.current);

// 不论读取还是赋值都需要知道属性名，实用性远不如上面
countRf[1] = 1; 
console.log(countRf[1]);
```

## ref属性值有哪几种以及区别是什么？

### 1. 字符串类型 (已弃用)

```jsx
class MyComponent extends React.Component {
  render() {
    return <div ref="myDiv">Hello World</div>;
  }
}
```

**特点**：
- **已弃用**：在`React 16.3 `版本以后，**字符串`ref`已被弃用**，这意味着不推荐在新代码中使用这种方式。
- **作用域**：字符串`ref`的作用域有限，不能用于函数组件。
- **局限性**：它不提供对组件实例的引用并且在现代`React`的语义中不再适用。

### 2. 回调函数

```jsx
class MyComponent extends React.Component {
  setRef = (element) => {
    if (element) {
      // 元素被挂载时
      console.log('Element mounted:', element);
      // 你可以在这里做更多的操作
    } else {
      // 元素被卸载时
      console.log('Element unmounted');
    }
  };

  render() {
    return <div ref={this.setRef}>Hello World</div>;
  }
}
```

**特点**：
- **灵活性**：回调函数可以执行其他逻辑，允许在组件挂载或卸载时处理元素。
- **动态更新**：**当组件更新时，回调会被调用两次：一次是旧的元素被卸载，另一次是新的元素被加载**。
- **适应性强**：特别适用于条件渲染的场景，可以控制如何存储和使用引用。

### 3. 对象类型 (使用 React.createRef 或 useRef)

**类组件**:

```jsx
class MyComponent extends React.Component {
  myDivRef = React.createRef();

  render() {
    return <div ref={this.myDivRef}>Hello World</div>;
  }
}
```

**函数组件**:

```jsx
import React, { useRef } from 'react';

const MyComponent = () => {
  const myDivRef = useRef(null);

  return <div ref={myDivRef}>Hello World</div>;
};
```

**特点**：
- **推荐使用**：对象类型的`ref`是现代`React`编程中的推荐做法。可以使用`React.createRef()`和`useRef()`方法创建。
- **一致性**：返回的`ref`对象在组件的整个生命周期内保持不变，无论组件重新渲染多少次。
- **简单直接**：访问元素的方式清晰且直接，通过`this.myDivRef.current`或`myDivRef.current`可以轻松访问`DOM`元素。
- **避免不必要的渲染**：更新`current`不会导致组件的重新渲染。

## useRef和createRef有什么区别

`useRef`和`createRef`的差别不大，都是在组件第一次渲染时创建，在之后每次渲染时都是引用相同的对象。但是因为函数式组件和类组件的渲染方式不同，导致使用方式有一些差别。

函数式组件每次渲染会重新调用整个函数，`useRef`不允许在循环语句或者条件语句或者其他钩子函数中调用，与生命周期无关，使用方式一致。

而类组件第一次渲染包括了组件的创建实例，后面的渲染基本上都是调用实例的`render`方法，在中不同的生命周期方法调用`createRef`会产生不同的效果。

比如正常在`constructor`中调用`createRef`，只会创建一次，每次渲染时获取的引用对象相同。

```jsx
class MyClassComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }

  render() {
    // 每次渲染都会得到不同的 myRef 实例
    console.log(this.myRef); // 不同的引用
    return <div ref={this.myRef}>Hello World</div>;
  }
}
```

但是如果在其他声明周期中使用，则每次调用声明周期方法都会创建新的引用对象，导致前后引用不相同，同时因为创建新的对象，增加内存分配导致了性能的损耗。

```jsx
class MyClassComponent extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    // 每次渲染都会得到不同的 myRef 实例
    this.myRef = React.createRef();
    console.log(this.myRef); // 不同的引用
    return <div ref={this.myRef}>Hello World</div>;
  }
}
```

### 总结

| 特点              | `createRef`                      | `useRef`                      |
|-------------------|----------------------------------|-------------------------------|
| 组件类型          | 类组件                           | 函数组件                      |
| 每次渲染时       | 创建新的引用实例                | 返回相同的引用对象           |
| 用法              | 在不同的生命周期函数中使用带来不同的结果，建议在constructor中使用 | 用于跨渲染保持相同值的情况	|
| 性能              | 可能导致性能损耗                | 性能更优                     |

## 使用场景

### 如果页面有不定数量的ref，应该如何使用useRef

在`React`中，如果你需要为一个页面上的不定数量的元素创建引用，可以通过使用`useRef`配合数组或对象来管理多个引用。下面是几种常见的实现方式：

#### 1. 使用数组
如果你有一个不定数量的元素，比如一个列表，你可以使用一个数组来存储所有的引用。

```jsx
import React, { useRef } from 'react';

const DynamicRefsExample = () => {
  const items = ["Item 1", "Item 2", "Item 3"]; // 示例数据
  const refs = useRef([]); // 初始化引用为数组

  const focusItem = (index) => {
    if (refs.current[index]) {
      refs.current[index].focus(); // 聚焦对应的输入框
    }
  };

  return (
    <div>
      {items.map((item, index) => (
        <div key={index}>
          <input
            ref={(el) => (refs.current[index] = el)} // 将引用赋值给当前索引
            type="text"
            placeholder={item}
          />
          <button onClick={() => focusItem(index)}>Focus</button>
        </div>
      ))}
    </div>
  );
};

export default DynamicRefsExample;
```

#### 2. 使用对象
如果需要更具语义化或是你希望引用的名字更明确，可以使用对象来存储引用：

```jsx
import React, { useRef } from 'react';

const DynamicRefsWithObject = () => {
  const items = { item1: "Item 1", item2: "Item 2", item3: "Item 3" }; // 示例数据
  const refs = useRef({}); // 使用对象存储引用

  const focusItem = (name) => {
    if (refs.current[name]) {
      refs.current[name].focus(); // 聚焦对应的输入框
    }
  };

  return (
    <div>
      {Object.keys(items).map((key) => (
        <div key={key}>
          <input
            ref={(el) => (refs.current[key] = el)} // 将引用赋值给对象的字段
            type="text"
            placeholder={items[key]}
          />
          <button onClick={() => focusItem(key)}>Focus {items[key]}</button>
        </div>
      ))}
    </div>
  );
};

export default DynamicRefsWithObject;
```

#### 3. 使用Map
如果你有一个不定数量的元素，比如一个列表，你可以使用一个Map来存储所有的引用，Map可以比数组更快捷查找元素。

```jsx
import React, { useRef } from 'react';

const DynamicRefsExample = () => {
  const items = ["Item 1", "Item 2", "Item 3"]; // 示例数据
  const refs = useRef(new Map()); // 初始化引用为数组

  const focusItem = (index) => {
    if (refs.current.has(index)) {
      refs.current.get(index).focus(); // 聚焦对应的输入框
    }
  };

  return (
    <div>
      {items.map((item, index) => (
        <div key={index}>
          <input
            ref={(el) => (refs.current.set(index, el))} // 将引用赋值给当前索引
            type="text"
            placeholder={item}
          />
          <button onClick={() => focusItem(index)}>Focus</button>
        </div>
      ))}
    </div>
  );
};

export default DynamicRefsExample;
```