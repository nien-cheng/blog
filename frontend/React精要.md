- [React精要](#react精要)
  - [组件化架构](#组件化架构)
    - [类组件](#类组件)
    - [函数式组件](#函数式组件)
    - [Hooks](#hooks)
  - [声明式编程](#声明式编程)
  - [单向数据流](#单向数据流)
  - [核心概念](#核心概念)


# React精要
React作为现代前端开发的核心框架，其精要可归纳为以下几个核心理念与技术特性：

1. 组件化架构
1. 声明式编程
1. 单向数据流
1. 虚拟DOM

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
开发者只需描述UI应呈现的状态（UI = f(state, props)），而非手动操作DOM。React通过虚拟DOM自动计算差异并批量更新真实DOM，减少性能损耗。
    - 声明式编程是一种编程范式，其核心思想是描述问题的目标或所需结果，而非具体的实现步骤。
    - 相对应的是命令式编程，需逐条指令指导计算机操作（如循环、变量赋值），关注执行流程。

## 单向数据流
数据从父组件流向子组件（通过props），子组件通过回调函数向上传递事件，确保数据流动可预测，避免状态混乱。传统双向绑定（如Angular的ngModel）虽然简化了表单等场景的开发，但会导致数据流隐式且难以追踪，增加调试难度。


## 核心概念
- **组件（函数组件/类组件）**  
  构建UI的基本单元，函数组件+Hooks成为主流范式，类组件用于兼容老代码

- **状态（State）与管理**  
  组件内部使用useState，复杂场景配合Context API或Redux等状态管理库

- **生命周期（类组件）与 Hooks（函数组件）**  
  类组件有componentDidMount等生命周期方法，函数组件通过useEffect等Hooks实现类似功能

- **虚拟DOM与Diff算法**  
  通过内存DOM树比对，最小化真实DOM操作，提升渲染性能

- **单向数据流**  
  数据通过Props自上而下传递，子组件不能直接修改父级数据

- **组合模式（组件嵌套）**  
  通过props.children或自定义插槽实现组件复合，优于继承的代码复用方式

- **Context API**  
  跨层级组件数据传递方案，替代逐层传递props的繁琐操作

- **JSX语法**  
  JavaScript语法扩展，允许在JS中编写HTML-like结构，最终通过Babel编译为JS代码

- **函数式编程思想**  

- **生态系统（React Router/Redux等）**  
  路由管理、状态管理、SSR等能力通过扩展库实现，形成完整技术栈