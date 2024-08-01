# 为什么使用React Hooks

## 问题的必要性

1. 精力有限，需要确认学习内容的价值。
2. 探究React Hooks在React的意义。
3. 挖掘其与之前的React差别。

## 背景知识

1. React v16.8之前就已经有函数组件和类组件，但是函数组件只用于渲染展示组件，不能使用生命周期方法或者管理组件状态，类组件可以使用生命周期方法或者管理组件状态。
2. React v16.8之后函数式组件可以使用生命周期方法和管理组件状态，除了特殊场景外类组件能实现的场景都能使用函数式组件替换，因此React Hooks其实是加强了函数式组件。
3. React v16.8之后类组件依旧可以使用，只是推荐优先使用函数式组件。

## 探究为什么出现React Hooks

1. React的类组件使用存在一些缺点，在React Hooks可以解决或者避免这些问题。
2. 函数式组件更适合未来的发展趋势。

### 类组件存在的问题

1. 复用逻辑比较复杂，并且容易产生问题。可以参考[复用逻辑文章](./02-类组件复用逻辑的方法.md)
2. 生命周期方法管理副作用逻辑比较麻烦。例如页面需要添加一个定时器，每隔一段时间需要提示信息，同时外部参数变化时需要重启计时器，在类组件中需要再多个生命周期方法中编写逻辑，而在React Hooks可以集中编写逻辑，可以参考如下代码进行对比：
   ```jsx
    import React, { Component } from 'react';

    class TimerClassComponent extends Component {
    constructor(props) {
        super(props);
        this.state = {
        message: '',
        };
        this.timer = null;
    }

    componentDidMount() {
        this.startTimer();
    }

    componentDidUpdate(prevProps) {
        if (prevProps.interval !== this.props.interval) {
        this.restartTimer();
        }
    }

    componentWillUnmount() {
        clearInterval(this.timer);
    }

    startTimer() {
        this.timer = setInterval(() => {
        this.setState({ message: 'Time to remind!' });
        }, this.props.interval);
    }

    restartTimer() {
        clearInterval(this.timer);
        this.startTimer();
    }

    render() {
        return (
        <div>
            <p>{this.state.message}</p>
        </div>
        );
    }
    }

    export default TimerClassComponent;

   ```
   
   ```jsx
   import React, { useState, useEffect } from 'react';

    const TimerFunctionComponent = ({ interval }) => {
    const [message, setMessage] = useState('');

    useEffect(() => {
        const timer = setInterval(() => {
        setMessage('Time to remind!');
        }, interval);

        return () => clearInterval(timer);
    }, [interval]);

    return (
        <div>
        <p>{message}</p>
        </div>
    );
    };

    export default TimerFunctionComponent;
   ```
3. 学习难度比较高，需要理解React的生命周期和this的使用逻辑。参考[this相关的文章](./03-this的使用.md)

### 未来的发展方式

React中万物皆是组件，而将一个复杂的系统拆分众多的组件，无疑要求组件之间的可复用性更高，同时耦合度更低以及灵活性更高，因此组合模式更优于继承模式。而组合模式指的是函数式组件，而继承模式指的就是类组件，从这个角度看React未来会越来越多的使用函数式组件，这也是React Hooks最近几年不断被推广的重要原因。

#### 为什么上面说组合模式优于继承模式？

首先需要理解什么是继承模式和组合模式。

- 继承模式是一种通过类的继承层次结构来实现代码复用和扩展的设计模式。它强调的是类之间的继承关系，通过继承父类的属性和方法来实现子类的功能扩展。
- 组合模式是一种通过将对象组合在一起以构建复杂对象的设计模式。它强调对象之间的组合关系，而不是继承关系。组合模式可以通过组合多个对象来实现更复杂的功能，而不需要通过继承来增加复杂性。

举个例子，有个表示动物的类，它有嘴巴这个属性，可以吃饭，也可以说话。现在新增一个机器人的类，其行为和动物相似，只是嘴巴只能说话，不能吃饭，为了提高复用性，继承动物的类。代码如下：

```ts

interface Mouth {
  eat(): void;
  speak(): void;
}

abstract class Animal implements Mouth {
  abstract name: string;

  eat(): void {
    console.log(`${this.name} is eating.`);
  }

  speak(): void {
    console.log(`${this.name} is speaking.`);
  }
}

class Robot extends Animal {
  name: string;

  constructor(name: string) {
    super();
    this.name = name;
  }

  eat(): void {
    console.log(`${this.name} cannot eat.`);
  }
}

```

机器人的类继承动物的类后，可以实现功能，但是带来一个问题，它依然可以调用`speak方法`，即说话，这个不是我们想要的，为了禁止这个行为，不得不重写`eat方法`。
这个就是继承模式的缺点，会带来预期之外的行为，需要额外处理这些行为。

但是上面这个例子用组合模式来实现哪？

```ts

interface Mouth {
  eat(): void;
  speak(): void;
}

abstract class Animal implements Mouth {
  abstract name: string;

  eat(): void {
    console.log(`${this.name} is eating.`);
  }

  speak(): void {
    console.log(`${this.name} is speaking.`);
  }
}

class Robot {
  private animal: Animal;

  constructor() {
    this.animal = new Animal();
  }

  speak(): void {
    this.animal.speak();
  }
}

```

可以看到只调用了动物类的`speak方法`，这个就是组合模式的优点，只复用自己需要的功能，不会带来预期之外的行为。

## 参考资料

- [React为什么需要Hooks](https://cloud.tencent.com/developer/article/2079066)
- [React Hook 与 传统 Class 的对比](https://jelly.jd.com/article/61aed4a97f05d46ce6b791f4)
- [React Hooks 入门教程](https://www.ruanyifeng.com/blog/2019/09/react-hooks.html)
- [Why to use React Hooks Instead of Classes in React JS ?](https://www.geeksforgeeks.org/why-to-use-react-hooks-instead-of-classes-in-reactjs/)