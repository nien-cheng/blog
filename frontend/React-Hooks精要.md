# React Hooks精要

## 1. useState
- 用于在函数组件中添加状态管理
- 返回一个状态值和一个更新该状态的函数
- 示例：`const [count, setCount] = useState(0)`

## 2. useEffect
- 用于处理副作用操作（如数据获取、订阅、手动DOM操作）
- 可以替代类组件中的生命周期方法
- 示例：`useEffect(() => { document.title = `You clicked ${count} times`; })`
  
在React中，"副作用"（Side Effects）指的是那些会影响组件外部或与外部进行交互的操作。这些操作通常包括：

1. 数据获取（API调用）
2. 订阅或事件监听
3. 手动修改DOM
4. 设置定时器
5. 记录日志
6. 与浏览器API交互
7. 读写文件
8. 发送网络请求

在函数组件中，这些操作不能直接放在函数体内部执行，因为它们可能会：

- 影响渲染性能
- 导致不必要的重复执行
- 造成内存泄漏

这就是为什么React提供了 useEffect 这个Hook，它允许我们在组件渲染完成后执行这些副作用操作，并且可以控制它们的执行时机（通过依赖数组）和清理时机（通过返回清理函数）。

## 3. useContext
- 用于在组件树中共享数据，避免props层层传递
- 需要配合React.createContext使用
- 示例：`const value = useContext(MyContext)`

## 4. useReducer
- useState的替代方案，适合复杂状态逻辑
- 类似于Redux中的reducer模式
- 示例：`const [state, dispatch] = useReducer(reducer, initialState)`
- 适用场景：
  - 状态更新逻辑复杂，涉及多个条件或步骤
  - 新状态依赖于旧状态
  - 需要集中管理状态更新逻辑
  - 需要优化深层嵌套组件的性能
  

复杂状态逻辑通常指以下几种情况：

1. 多个状态相互依赖 ：
   
   ```javascript
   const initialState = { count: 0, step: 1 };
   
   function reducer(state, action) {
     switch (action.type) {
       case 'increment':
         return { ...state, count: state.count + state.step };
       case 'decrement':
         return { ...state, count: state.count - state.step };
       case 'setStep':
         return { ...state, step: action.payload };
       default:
         throw new Error();
     }
   }
    ```
2. 状态更新涉及多个步骤 ：
   
   ```javascript
   function reducer(state, action) {
     switch (action.type) {
       case 'fetchData':
         return { ...state, loading: true };
       case 'fetchDataSuccess':
         return { ...state, loading: false, data: action.payload };
       case 'fetchDataFailure':
         return { ...state, loading: false, error: action.payload };
       default:
         throw new Error();
     }
   }
    ```
3. 状态更新需要条件判断 ：
   
   ```javascript
   function reducer(state, action) {
     switch (action.type) {
       case 'toggle':
         return { ...state, isOn: !state.isOn };
       case 'reset':
         return { ...state, isOn: action.payload === 'on' };
       default:
         throw new Error();
     }
   }
    ```
    
## 5. useRef
- 用于创建可变的引用对象
- 常用于访问DOM元素或存储可变值
- 示例：`const inputRef = useRef(null)`

## 6. useMemo
- 用于性能优化，缓存计算结果
- 避免在每次渲染时都进行昂贵的计算
- 示例：`const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])`
- 缓存机制：
  - 首次渲染执行计算函数并缓存结果
  - 依赖项变化时重新计算
  - 依赖项不变时返回缓存值
- 注意事项：
  - 依赖项数组使用浅比较(Object.is)
  - 不提供依赖数组会导致每次渲染都重新计算
  - 计算函数应该是纯函数，不应有副作用

```javascript
const expensiveValue = useMemo(() => {
  // 假设这是一个计算量很大的函数
  let result = 0;
  for (let i = 0; i < 1000000; i++) {
    result += a * b;
  }
  return result;
}, [a, b]); // 只有当a或b变化时才会重新计算
```

## 7. useCallback
- 用于缓存回调函数
- 避免在每次渲染时都创建新的函数实例
- 示例：`const memoizedCallback = useCallback(() => { doSomething(a, b); }, [a, b]);`
- 使用场景：
  - 将回调传递给子组件时避免不必要的重新渲染
  - 函数作为其他Hook的依赖项时
  - 函数内部依赖特定状态或props时
- 注意事项：
  - 过度使用可能反而降低性能
  - 只在确实需要优化性能时使用
```jsx
const Child = React.memo(({ onClick }) => {
  console.log('Child渲染');
  return <button onClick={onClick}>点击</button>;
});

function Parent() {
  const [count, setCount] = useState(0);
  
  // 使用useCallback缓存函数
  const handleClick = useCallback(() => {
    console.log('点击处理');
  }, []); // 空依赖表示函数不会改变

  return (
    <>
      <Child onClick={handleClick} />
      <button onClick={() => setCount(c => c + 1)}>增加 {count}</button>
    </>
  );
}
```
作为useEffect依赖项时：
```jsx
function Example({ id }) {
  const fetchData = useCallback(async () => {
    const data = await fetch(`/api/data/${id}`);
    return data.json();
  }, [id]); // 当id变化时才创建新函数

  useEffect(() => {
    fetchData();
  }, [fetchData]); // 安全地将函数作为依赖
}
```

## 8. useLayoutEffect
- 与useEffect类似，但在DOM更新后同步执行
- 适用于需要直接操作DOM的场景
- 示例：`useLayoutEffect(() => { /* DOM操作 */ })`

## 9. useImperativeHandle
- 用于自定义暴露给父组件的实例值
- 通常与forwardRef一起使用
- 示例：`useImperativeHandle(ref, () => ({ focus: () => { /* ... */ } }))`

## 10. useDebugValue
- 用于在React开发者工具中显示自定义Hook的标签
- 主要用于调试目的
- 示例：`useDebugValue(isOnline ? 'Online' : 'Offline')`