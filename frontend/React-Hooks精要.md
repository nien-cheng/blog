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

## 5. useRef
- 用于创建可变的引用对象
- 常用于访问DOM元素或存储可变值
- 示例：`const inputRef = useRef(null)`

## 6. useMemo
- 用于性能优化，缓存计算结果
- 避免在每次渲染时都进行昂贵的计算
- 示例：`const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])`

## 7. useCallback
- 用于缓存回调函数
- 避免在每次渲染时都创建新的函数实例
- 示例：`const memoizedCallback = useCallback(() => { doSomething(a, b); }, [a, b]);`

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