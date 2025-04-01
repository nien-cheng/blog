# Node.js精要

## 核心特性

- 非阻塞IO
- 事件驱动
- 单线程
- 异步
- 回调
- 包管理器
- 模块
- 事件

## 事件驱动
事件驱动（Event-Driven）是一种编程范式，其核心特征包括：
1. **事件触发机制**：通过事件监听器（Event Listeners）响应系统事件
2. **非阻塞执行**：主线程不等待I/O操作完成
3. **回调机制**：事件完成时通过回调函数通知主线程

```javascript
const EventEmitter = require('events');
class MyEmitter extends EventEmitter {}

// 创建事件发射器
const myEmitter = new MyEmitter();

// 注册事件监听器
myEmitter.on('event', () => {
  console.log('事件触发');
});

// 触发事件
myEmitter.emit('event'); 
```

关键优势：
- 高并发处理能力（适用于Web服务器等I/O密集型场景）
- 资源利用率高（单线程处理数千并发连接）
- 自然解耦（事件生产者与消费者分离）

## V8引擎
V8是Google开发的高性能JavaScript引擎，核心特性包括：
- **即时编译（JIT）**：将JS代码编译为机器码执行
- **内存管理**：自动垃圾回收机制（GC）
- **跨平台**：支持Windows/macOS/Linux系统
- **优化机制**：隐藏类（Hidden Classes）和内联缓存（Inline Caching）

```shell
#查看V8版本
node --v8-options | grep -i version

#调整内存限制（默认约1.7GB）
#参数作用说明：
#--max-old-space-size 用于配置V8老生代内存区域的最大值
#4096 表示设置为4GB内存（单位：MB）
node --max-old-space-size=4096 app.js
```

## 非阻塞I/O模型
非阻塞I/O指程序执行I/O操作时不等待结果返回，通过事件回调机制处理响应。对比传统阻塞I/O：

```javascript
// 阻塞式I/O（同步）
const data = fs.readFileSync('file.txt');
console.log(data);

// 非阻塞I/O（异步）
fs.readFile('file.txt', (err, data) => {
  console.log(data); 
});
console.log('继续执行其他操作');
```

实现原理：

1. 通过libuv库实现操作系统层面的异步I/O
2. 使用事件循环（Event Loop）轮询I/O完成状态
3. I/O操作委托给线程池处理（文件系统等）

核心优势：

- 高并发：单线程处理数千并发连接
- 资源高效：避免线程上下文切换开销
- 事件驱动：自然适应实时应用场景
注意事项：

- 避免回调地狱（Callback Hell），建议使用：
  - Promise链式调用
  - async/await语法糖
  - 第三方库（async.js）

## 模块

### CommonJS规范
CommonJS规范是Node.js默认的模块系统，主要用于服务器端模块管理：
- 模块导出：`module.exports`
- 模块导入：`require('模块路径')`

```javascript
// 模块导出 (math.js)
module.exports = {
  add: (a, b) => a + b,
  PI: 3.1415926
};

// 模块导入 (app.js)
const { add, PI } = require('./math.js');
console.log(add(PI, 2)); 
```

### ES Module
ES Module是JavaScript的官方模块系统，主要用于浏览器端模块管理。
从Node.js v13.2开始原生支持：
- 模块导出：`export`
```javascript
// 启用ESM
// package.json中添加： "type": "module"

// 导出模块
export const greet = () => 'Hello World';

// 导入模块
import { greet } from './module.mjs';
```
文件后缀：

1、.mjs ：明确标识ES Module文件
- 强制使用 import/export 语法
- 适用于Node.js v12+环境

2、.cjs ：明确标识CommonJS文件
- 使用 require/module.exports 语法
- 向后兼容旧版本Node.js

注意事项：
1. 混合使用时需明确文件扩展名
2. ES Module中不能使用 __dirname 等CommonJS变量
3. 导入路径必须包含扩展名（如 .mjs 或 .js ）
4. 使用 "type": "module" 时，CommonJS文件需显式使用.cjs扩展名

## 注意事项
Node.js采用单线程事件循环架构。
Worker Threads（工作线程）是实现多线程并行处理的核心机制。

1. 不适合CPU密集型任务，Worker Threads。
2. 适合I/O密集型任务，如Web服务器、文件读写、数据库操作。
3. 单线程避免了多线程的复杂同步问题。
2. 单个错误会导致进程崩溃，可选用进程守护（pm2）。