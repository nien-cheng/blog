- [React精要](#react精要)
  - [组件化架构](#组件化架构)
    - [类组件](#类组件)
    - [函数式组件](#函数式组件)
    - [Hooks](#hooks)
  - [声明式编程](#声明式编程)
  - [单向数据流](#单向数据流)
  - [虚拟DOM](#虚拟dom)
  - [生命周期](#生命周期)


# React精要
React作为现代前端开发的核心框架，其精要可归纳为以下几个核心理念与技术特性：

1. 组件化架构
2. 声明式编程
3. 单向数据流
4. 虚拟DOM
6. 生命周期
9. JSX语法
5. 状态管理
7. Context API
8. 生态系统

## 组件化架构
React将UI拆解为独立、可复用的组件，每个组件专注于单一功能，通过组合而非继承构建复杂界面。
组件间通过props传递数据，形成树状结构，提升代码模块化与可维护性。
### 类组件
通过state管理内部状态，结合生命周期方法处理复杂逻辑（如数据请求）。
我自己不太常用了，但是在较早的React开发中比较常见的。
大约是2019年开始写React代码，那时候用的比较多。
```jsx
class Counter extends React.Component<{}, { count: number }> { 
  constructor(props: {}) { 
      super(props); 
      this.state = { count: 0 }; 
  }
  
  handleClick = () => this.setState({ 
      count: this.state.count + 1 
  });
   
  render() { 
      return <button onClick={this.handleClick}> 
          {this.state.count}
      </button>; 
  }
}
```
### 函数式组件
以纯函数形式定义UI，接收props并返回DOM元素，天然支持声明式编程。
据2022-2024年的调研，80%以上的新项目采用函数式组件，仅老项目或特定场景保留类组件。
```jsx
function Counter({ count, onClick }: { count: number, onClick: () => void }) {
  return <button onClick={onClick}>
      {count}
  </button>;
}
```

### Hooks
是React 16.8引入的函数式工具，用于在函数式组件中管理状态、副作用等能力。
本质是“钩子函数”（如useState、useEffect），将类组件的生命周期和状态逻辑注入函数式组件。
```jsx
function Counter() {
  const [count, setCount] = useState(0);
  const handleClick = () => setCount(count + 1);

  return <button onClick={handleClick}>
      {count}
  </button>;
}
```

## 声明式编程
开发者只需描述UI应呈现的状态（UI = f(state, props)），而非手动操作DOM。
React通过虚拟DOM自动计算差异并批量更新真实DOM，减少性能损耗。
声明式编程是一种编程范式，其核心思想是描述问题的目标或所需结果，而非具体的实现步骤。
```jsx
// 声明式：状态驱动视图
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(c => c + 1)}>
      Clicked {count} times
    </button>
  );
}
```
相对应的是命令式编程，需逐条指令指导计算机操作（如循环、变量赋值），关注执行流程。
```javascript
// 命令式：手动操作DOM
const button = document.getElementById('myButton');
let count = 0;
button.addEventListener('click', () => {
  count++;
  button.textContent = `Clicked ${count} times`; // 直接修改DOM
});
```
## 单向数据流
数据从父组件流向子组件（通过props），子组件通过回调函数向上传递事件，确保数据流动可预测，避免状态混乱。
传统双向绑定（如Angular的ngModel）虽然简化了表单等场景的开发，但会导致数据流隐式且难以追踪，增加调试难度。

```jsx
// 父组件管理状态
function ParentComponent() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <ChildComponent 
        value={count} 
        onIncrement={() => setCount(c => c + 1)}
      />
    </div>
  );
}

// 子组件只接收props和触发事件
function ChildComponent({ value, onIncrement }) {
  return (
    <button onClick={onIncrement}>
      Current value: {value}
    </button>
  );
}
```

## 虚拟DOM
React通过虚拟DOM（Virtual DOM）实现高效的UI渲染。
虚拟DOM是一个轻量级的JavaScript对象，描述了真实DOM的结构和属性。
React通过Diff算法比较新旧虚拟DOM，找出差异并批量更新真实DOM，避免了频繁操作DOM的性能损耗。

```jsx
// 虚拟DOM
const virtualDOM = {
  type: 'div',
  props: {
    className: 'container',
    children: [
      { type: 'h1', props: { children: 'Hello, World!' } },
      { type: 'p', props: { children: 'This is a paragraph.' } }  
    ]  
  }  
};

// 渲染到真实DOM
ReactDOM.render(virtualDOM, document.getElementById('root'));
```

## 生命周期
React组件的生命周期包括挂载、更新、卸载等阶段，通过生命周期方法处理组件的初始化、数据更新、销毁等逻辑。
生命周期方法是类组件的重要组成部分，用于管理组件的状态和行为。
我们先看下类组件的生命周期方法，比较好理解生命周期。
```jsx
class Counter extends React.Component {
  constructor() {
    // 初始化状态
    // 不推荐在构造函数中执行副作用操作（如网络请求）
    // 仅执行一次
  } 
  componentDidMount() {
    // 组件挂载后执行的逻辑 
    // 初始化DOM操作、网络请求等
    // 挂载后立即执行（首次渲染）
    // 挂载后执行一次
  }
  componentDidUpdate() {
    // 组件更新后执行的逻辑 
    // 更新阶段（props/state变化
    // 响应变化，执行副作用操作
    // 每次更新后执行
  }
  componentWillUnmount() {
    // 组件卸载前执行的逻辑
    // 清理定时器、取消订阅等
    // 卸载前执行一次
  }
  render() {
    // 渲染组件
    // render方法在每次渲染时执行
    return <button onClick={this.handleClick}>
        {this.state.count}
    </button>; 
  }
}
```
以下是函数式组件的生命周期方法，如果没有类组件的基础，对应起来会有点困难。
毕竟最简单的组件，可以没有useEffect和useState，相当于只有一个render方法。
```jsx
function ExampleComponent() {
  const [count, setCount] = useState(0); // 状态初始化
  useEffect(() => {
    // 组件挂载后执行的逻辑
    // 初始化DOM操作、网络请求等
    // 挂载后立即执行（首次渲染）
    // 挂载后执行一次
    // constructor + componentDidMount函数
    return () => {
      // 组件卸载前执行的逻辑
      // 清理定时器、取消订阅等
      // 卸载前执行一次
      // componentWillUnmount函数
    };
  }, []);

  return <div>...</div>; // render函数，渲染组件
}
```