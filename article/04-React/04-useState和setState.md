# useState和setState

useState 是 React 中的一个 Hook，用于在函数组件中添加状态管理。setState 是类组件中用于更新状态的方法。虽然它们的使用场景和语法有所不同，但它们的核心目的是相同的：管理和更新组件的状态。

## 区别

### 相同点

1. 状态管理
   - useState 和 setState 都用于管理和更新组件的状态。
   - 它们都可以触发组件重新渲染，以反映状态的变化。
2. 异步更新
   - useState 和 setState 的状态更新都是异步的，更新后的状态在下一次渲染时才会生效。
3. 合并更新
   - 在事件处理函数中，React 会批量处理多个 useState 和 setState 调用，以优化性能。

### 不同点

1. 使用场景
   - useState：用于函数组件中添加状态管理。
   - setState：用于类组件中更新状态。
2. 语法和调用方式
   - useState：返回一个状态值和一个更新函数。
  
   ```js
    const [state, setState] = useState(initialState);
   ```
   - setState：直接调用类组件的实例方法。
  
   ```js
    this.setState({ key: value });
   ```
3. 状态合并
   - useState：不会自动合并状态，需要手动合并。
  
   ```js
    const [state, setState] = useState({ a: 1, b: 2 });
    setState(prevState => ({ ...prevState, a: 3 }));
   ```
   - setState：会自动合并状态。
  
   ```js
    this.setState({ a: 3 });  // 只更新 a，不影响 b
   ```
4. 初始状态
   - useState：可以接受一个初始状态值或一个返回初始状态值的函数。
  
   ```js
    const [state, setState] = useState(() => computeInitialState());
   ```
   - setState：初始状态在类组件的构造函数中定义。
  
   ```js
    constructor(props) {
      super(props);
      this.state = { key: initialState };
    }

   ```
5. 状态更新函数
   - useState：更新函数可以接受一个新的状态值或一个返回新状态值的函数。
  
   ```js
    setState(newValue);
    setState(prevState => newValue);
   ```
   - setState：更新函数接受一个新的状态对象或一个返回新状态对象的函数。
  
   ```js
    this.setState({ key: newValue });
    this.setState(prevState => ({ key: newValue }));
   ```
6. 状态存储位置
   - useState：状态存储在 React 内部的 hook queue 中，每个函数组件实例都有自己的 hook queue。
   - setState：状态存储在类组件的实例对象中，每个类组件实例都有自己的状态对象。

## 使用问题

### 同一个事件中多次调用useState的更新方法

查看下面的代码，当点击按钮是count值会如何变化？

```jsx
import React, { useState } from 'react';

const CounterHook = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
    // count 只增加了一次，为什么？
  };

  const decrement = () => {
    setCount(count - 1);
    setCount(count - 1);
    setCount(count - 1);
    // count 只减少了一次，为什么？
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};
```

**为什么点击是操作多次，但是实际效果和一次一样？**

这涉及到了React Hooks的useState原理，参考下面的简易模型：

```js
let hookStates = []; // 用于存储每个 hook 的状态
let hookIndex = 0;   // 当前 hook 的索引

function useState(initialValue) {
  const currentIndex = hookIndex; // 保存当前 hook 的索引

  // 初始化状态
  if (hookStates[currentIndex] === undefined) {
    // 如果 initialValue 是函数，调用该函数获取初始状态值
    hookStates[currentIndex] = typeof initialValue === 'function' ? initialValue() : initialValue;
  }

  // 定义 setState 函数
  const setState = (newValue) => {
    // 如果 newValue 是函数，调用该函数获取新的状态值
    hookStates[currentIndex] = typeof newValue === 'function' ? newValue(hookStates[currentIndex]) : newValue;
    render(); // 触发重新渲染
  };

  hookIndex++; // 增加 hook 索引
  return [hookStates[currentIndex], setState];
}

function render() {
  hookIndex = 0; // 重置 hook 索引
  // 重新渲染组件逻辑
  // 这只是一个简化的例子，实际的 React 渲染逻辑更复杂
  console.log('Component re-rendered with state:', hookStates);
  Counter(); // 模拟重新渲染组件
}

// 示例组件使用简化的 useState
function Counter() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('John');

  const increment = () => {
    setCount(prevCount => prevCount + 1);
  };

  const changeName = () => {
    setName('Doe');
  };

  console.log('Render:', { count, name });

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <p>Name: {name}</p>
      <button onClick={changeName}>Change Name</button>
    </div>
  );
}

// 模拟组件首次渲染
Counter();
```

根据这个简易模型带入问题的例子，看一下执行的过程：

- 初始化是调用`useState`方法，返回了初始化值和更新函数。
  - `useState`方法被调用，判断初始化值为空，向`hookStates`数组增加初始值。
  - `hookStates`数组下标`hookIndex`增加1，每一个`useState`的值对应下标。
  - 返回当前`state`的值和更新函数。
- 点击增加，执行更新方法。
  - 更新方法`setCount`的参数count取自`useState`方法执行返回的`state`的值，这个值是0，相当于执行多次`setCount(0 + 1)。
  - 需要注意的是在合成事件或者钩子函数中更新函数是批量执行的，你可以理解为虽然代码写了多次`setCount`方法调用，但是不是执行一个`setCount`方法调用就渲染一次组件，而是等点击事件执行完才会进行批量更新渲染，这也是为什么说相当于执行多次`setCount(0 + 1)。如果执行一个`setCount`方法调用就渲染一次组件，则由于组件渲染，函数组件的函数都被销毁了，无法执行后续代码，因此这里必须是批量更新渲染。
- 由于执行多次`setCount(0 + 1)`，其结果都是1，因此渲染之后的结果相当于执行一次`setCount(0 + 1)`。
  - 调用`useState`方法的更新函数是，可以发现当`newValue`相同时，保存在`hookStates`数组对应下标的值是相同的。
  - 同时这里可以理解为闭包，虽然调用更新函数时函数组件的函数销毁，但是由于更新函数`setCount`还引用销毁之前函数组件的`count`变量，这个变量是不变的，这也是为什么说React hooks的**state是不可变的**。
- 重新渲染函数组件，调用`useState`方法，由于此时`hookStates`数组对应的下标有值，此时直接取值，然后返回值和更新函数，`count`此时为1。

#### useState返回的值不可变是否与值的类型有关

还是上一个问题，这次将count变量用对象包装一下：

```jsx
import React, { useState } from 'react';

const CounterHook = () => {
  const [countObj, setCountObj] = useState({ count: 0 });

  const increment = () => {
    setCountObj({ count: countObj.count + 1 });
    setCountObj({ count: countObj.count + 1 });
    setCountObj({ count: countObj.count + 1 });
  };

  const decrement = () => {
    setCountObj({ count: countObj.count - 1 });
    setCountObj({ count: countObj.count - 1 });
    setCountObj({ count: countObj.count - 1 });
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};
```

**如果点击增加按钮，结果是否会不同哪？**

答案是**不会**。因为countObj依然是不可变，读取到的值是相同的。


如果再改一下，直接操作这个变量哪?

```jsx
import React, { useState } from 'react';

const CounterHook = () => {
  const [countObj, setCountObj] = useState({ count: 0 });

  const increment = () => {
    countObj.count += 1;
    setCountObj({ count: countObj.count });
    countObj.count += 1;
    setCountObj({ count: countObj.count });
    countObj.count += 1;
    setCountObj({ count: countObj.count });
  };

  const decrement = () => {
    countObj.count -= 1;
    setCountObj({ count: countObj.count });
    countObj.count -= 1;
    setCountObj({ count: countObj.count });
    countObj.count -= 1;
    setCountObj({ count: countObj.count });
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};
```

**如果点击增加按钮，结果是否会不同哪？**

答案是**会的**。但是**你并没有改变`useState`方法中存储到`hookStates`数组对应的下标的值，只是改变了在函数组件定义的值，改变的是你自己的变量**。

#### 更新方法的参数是函数时的行为

将更新方法的参数换成函数时，就会发现达到期望的效果了，即执行多次，每次都是在上一次结果的基础上操作。

```jsx
import React, { useState } from 'react';

const CounterHook = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(prevCount => prevCount + 1);
    setCount(prevCount => prevCount + 1);
    setCount(prevCount => prevCount + 1);
  };

  const decrement = () => {
    setCount(prevCount => prevCount - 1);
    setCount(prevCount => prevCount - 1);
    setCount(prevCount => prevCount - 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};
```

**为什么会出现这种情况的哪？**

问题在**useState**方法内部的更新方法对于参数的处理不一样，查看下面的关键代码：

```js

  // 定义 setState 函数
  const setState = (newValue) => {
    // 如果 newValue 是函数，调用该函数获取新的状态值
    hookStates[currentIndex] = typeof newValue === 'function' ? newValue(hookStates[currentIndex]) : newValue;
    render(); // 触发重新渲染
  };

```

当更新方法的参数是函数时，参数的函数值取自`hookStates`数组下标对应的值，而如果是非函数时，取得是函数组件中调用更新方法传递的值。按照这个逻辑执行之前的示例，简化步骤如下：

- 当第一次执行更新函数时，更新函数的参数执行，从`hookStates`数组取值0，参数方法执行后结果时1，`hookStates`数组对应下标的值更新为1。
- 当第二次执行更新函数时，更新函数的参数执行，从`hookStates`数组取值1，参数方法执行后结果时2，`hookStates`数组对应下标的值更新为2。
- 当第三次执行更新函数时，更新函数的参数执行，从`hookStates`数组取值2，参数方法执行后结果时3，`hookStates`数组对应下标的值更新为3。

与之前参数是非函数对比：

- 当第一次执行更新函数时，更新函数执行，参数为0，直接赋值给`hookStates`数组，`hookStates`数组对应下标的值更新为1。
- 当第二次执行更新函数时，更新函数执行，参数为0，直接赋值给`hookStates`数组，`hookStates`数组对应下标的值更新为1。
- 当第三次执行更新函数时，更新函数执行，参数为0，直接赋值给`hookStates`数组，`hookStates`数组对应下标的值更新为1。


### class组件的事件调用多次更新方法

```jsx
import React, { Component } from 'react';

class CounterClass extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
  }

  increment = () => {
    this.setState({ count: this.state.count + 1 });
    this.setState({ count: this.state.count + 1 });
    this.setState({ count: this.state.count + 1 });
  };

  decrement = () => {
    this.setState({ count: this.state.count - 1 });
    this.setState({ count: this.state.count - 1 });
    this.setState({ count: this.state.count - 1 });
  };

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
        <button onClick={this.decrement}>Decrement</button>
      </div>
    );
  }
}

export default CounterClass;
```


### 在异步事件中处理useState的更新方法



## 参考资料

- [React18源码解析之useState 的原理](https://www.xiabingbao.com/post/react/react-usestate-rn5bc0.html)
- [React中的useState和setState的执行机制](https://juejin.cn/post/7062162951108558855)
- [useState的原理及模拟实现](https://caijialinxx.github.io/2019/12/23/hooks-useState/)
- [React深入useState/setState](https://developer.aliyun.com/article/1147721)
- [React源码的GitHub仓库地址](https://github.com/facebook/react/blob/65903583d2ab45aea45bdd23ed0b5dc214ff3c1c/packages/react/src/ReactHooks.js#L30)
- [React Hooks - useState](https://zh-hans.react.dev/reference/react/useState)