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

在`increment`方法中触发了三次`this.setState`来增加`count`的值。由于`setState`的异步特性，三次更新将不会立即反映在`this.state.count`上，而是在`React`的更新周期中合并和处理。

- `setState`不是立即更新： 当你运行`this.setState({ count: this.state.count + 1 })`时，`React`并不会立即更新状态，而是将该更新请求放入一个队列中。这意味着，在每次调用`setState`时，`this.state.count`仍然反映的是更新前的值。
- 合并更新：由于这三次调用都是独立的，`React`会在最后只更新一次状态。换句话说，只会读取`this.state.count`在三次调用之前的原始值。

`setState`方法的逻辑可以参考下面的简单示例，但是需要注意是有些地方和直接看到的不同，比如直接理解下面代码，下面三次调用`mySetState`方法

- 第一次调用，`this.state.count`值为`0`，`updater`参数值应该为`{ count: 1 }`，`this.state.count`应该为`1`。
- 第二次调用，`this.state.count`值为`1`，`updater`参数值应该为`{ count: 2 }`，`this.state.count`应该为`2`。
- 第三次调用，`this.state.count`值为`2`，`updater`参数值应该为`{ count: 3 }`，`this.state.count`应该为`3`。

```js
import React, { Component } from 'react';

class MyComponent extends Component {
  constructor(props) {
    super(props);
    // 初始化状态
    this.state = {
      count: 0
    };

    // 绑定方法
    this.increment = this.increment.bind(this);
  }

  // 自定义 setState 方法
  mySetState(updater) {
    if (typeof updater === 'function') {
      // 如果 updater 是个函数，调用它以获取新的状态
      this.state = { ...this.state, ...updater(this.state) };
    } else {
      // 如果 updater 是个对象，直接合并到当前状态
      this.state = { ...this.state, ...updater };
    }
    
    // 重新渲染组件（模拟）
    this.render();
  }

  // 增加计数的方法
  increment() {
    // 调用自定义的 setState 方法
    this.mySetState({ count: this.state.count + 1 });
    this.mySetState({ count: this.state.count + 1 });
    this.mySetState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <div>
        <h1>计数器: {this.state.count}</h1>
        <button onClick={this.increment}>增加</button>
      </div>
    );
  }
}

export default MyComponent;
```

但是实际结果却是`1`，为什么会出现这种问题？答案是**闭包**，当执行`React`真正执行`mySetState`方法时，`increment`方法已经执行完毕，因此此时调用的this.state.count是执行之前的值，每次调用是`this.state.count`值都是`0`，因此结果为`1`。可以用下面的代码简单理解，当执行`React`真正执行`mySetState`方法时，情况类似下面：

```js
(function(state) {
  // 调用自定义的 setState 方法
  this.mySetState({ count: state.count + 1 });
  this.mySetState({ count: state.count + 1 });
  this.mySetState({ count: state.count + 1 });
}).call(this, this.state)
```

### 在异步事件中使用useState的更新方法

下面是一个常见的示例，展示了如何在异步事件（如定时器）中遇到闭包问题：

```js
import React, { useState, useEffect } from 'react';

const TimerComponent = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setCount(count + 1); // 这里的 `count` 捕获了初始值
    }, 1000);

    return () => clearInterval(timer);
  }, []); // 依赖项为空，意味着只在组件挂载时运行

  return <div>Count: {count}</div>;
};

export default TimerComponent;
```

在这个示例中：

- 定时器设定在每秒钟将`count`增加 1。
- 但是，因为依赖项数组为空，`useEffect`只运行一次（即组件挂载时），因此`count`在创建定时器时的值被捕获。
- 由于`count`的值没有更新，定时器中的`count + 1`实际上每次还是在累加初始值，最终不会按预期工作。

#### 现象分析

1. 状态的作用域： 当你调用`useState`时，`React`会将当前的状态值保存在内存中。例如，假设你有如下代码：

```js
const [count, setCount] = useState(0);
```
在组件的每次渲染中，`count`都代表当前组件的状态。

2. 定义函数时捕获的变量：当你在一个组件内定义一个函数（如事件处理器或定时器回调），该函数会捕获其外部作用域的变量。比如：

```js
useEffect(() => {
  const timer = setInterval(() => {
    console.log(count); // 捕获 count 的值
    setCount(count + 1); // 使用捕获的值
  }, 1000);
}, []);
```
在这个例子中，`setInterval`在组件挂载时就被设置了。此时`count`的值被捕获，这意味着当定时器执行时，它使用的是挂载时的`count`值。随着`setCount`更新状态，新的渲染不会影响已经设定的定时器。

3. 导致的结果：由于定时器内部使用了挂载时的`count`变量，当定时器每秒执行时，它会每次都在使用同一个、过时的`count`值。最终结果是，你的`count`不会如预期那样增加。

#### 原因分析

- **闭包的属性**：在`JavaScript`中，函数持有对其创建时闭包的引用。即使函数在其定义后被调用，它仍然会使用定义时的环境。这一点在`React`中尤其重要，因为组件的状态和生命周期管理的逻辑与`JavaScript`的原生闭包特性密切相关。

- **React 的异步更新**：`React`的状态更新通常是异步的，尤其是在批量更新和事件处理时，这使得状态很可能在触发时与当前的闭包环境不同。因此，直接在异步函数中使用状态可能会导致意外的行为。

#### 解决方案

为了解决这个问题，可以使用`setState`的函数形式。这样可以确保你总是获得最新的状态值。[原因参考这里](#更新方法的参数是函数时的行为)

```jsx
import React, { useState, useEffect } from 'react';

const TimerComponent = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setCount(prevCount => prevCount + 1); // 使用 prevCount 确保获取最新值
    }, 1000);

    return () => clearInterval(timer);
  }, []);

  return <div>Count: {count}</div>;
};

export default TimerComponent;
```

## 参考资料

- [React18源码解析之useState 的原理](https://www.xiabingbao.com/post/react/react-usestate-rn5bc0.html)
- [React中的useState和setState的执行机制](https://juejin.cn/post/7062162951108558855)
- [useState的原理及模拟实现](https://caijialinxx.github.io/2019/12/23/hooks-useState/)
- [React深入useState/setState](https://developer.aliyun.com/article/1147721)
- [React源码的GitHub仓库地址](https://github.com/facebook/react/blob/65903583d2ab45aea45bdd23ed0b5dc214ff3c1c/packages/react/src/ReactHooks.js#L30)
- [React Hooks - useState](https://zh-hans.react.dev/reference/react/useState)