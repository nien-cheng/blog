# JavaScript精要

## ECMAScript 版本演进

JavaScript 由 Brendan Eich 于 1995 年发明，并于 1997 年成为 ECMA 标准。
ECMAScript 是该语言的官方名称。
从 2015 年起，ECMAScript 按年命名（ECMAScript 2015）。

### 早期版本
- ECMAScript 1（ES1） ：发布于 1997 年，是 JavaScript 语言的首个标准化版本，定义了基本的语法、数据类型和内置对象。
- ECMAScript 2（ES2） ：于 1998 年发布，主要是对国际标准组织 ISO/IEC 16262 的格式进行了细微调整，没有引入新的语言特性。
- ECMAScript 3（ES3） ：1999 年发布，是一个重要的版本，引入了正则表达式、异常处理（ try...catch...finally ）、 do-while 循环和 switch 语句等特性，被广泛支持，许多现代 JavaScript 代码仍然基于 ES3 的特性编写。

### 过渡版本
- ECMAScript 4（ES4） ：该版本由于过于激进，引入了太多新特性，导致浏览器厂商之间无法达成一致，最终未能成为标准。不过，它的一些理念和特性在后续版本中得以体现。
- ECMAScript 3.1（ES3.1） ：作为 ES4 的替代方案，该版本进行了一些增量更新，并最终演变为 ECMAScript 5。

### 现代版本
- ECMAScript 5（ES5） ：于 2009 年发布，引入了严格模式（ "use strict" ）、 Object.defineProperty 、数组迭代方法（如 forEach 、 map 、 filter 等）和 JSON 对象等特性，提高了 JavaScript 的安全性和功能。
- ECMAScript 5.1（ES5.1） ：2011 年发布，是对 ES5 的一个小更新，主要是为了与 ISO/IEC 16262:2011 标准保持一致。

### 年度版本
从 2015 年开始，ECMAScript 采用了年度发布的模式，以下是一些重要的年度版本：

- ECMAScript 2015（ES6） ：是 JavaScript 语言的一次重大更新，引入了许多新特性，如箭头函数、 let 和 const 声明、模板字符串、解构赋值、类和继承、Promise 对象、 async/await 的基础（ Generator 函数）等，极大地提升了 JavaScript 的表达能力和开发效率。
- ECMAScript 2016（ES7） ：2016 年发布，引入了指数运算符（ ** ）和 Array.prototype.includes 方法。
- ECMAScript 2017（ES8） ：新增了 async/await 语法糖、 Object.values 、 Object.entries 、 String.prototype.padStart 和 String.prototype.padEnd 等方法，以及共享内存和原子操作 API。
- ECMAScript 2018（ES9） ：引入了异步迭代器、 Promise.prototype.finally 、扩展运算符在对象中的使用、 RegExp 改进等特性。
- ECMAScript 2019（ES10） ：添加了 Array.prototype.flat 、 Array.prototype.flatMap 、 Object.fromEntries 、 String.prototype.trimStart 和 String.prototype.trimEnd 等方法，以及可选的 catch 绑定。
- ECMAScript 2020（ES11） ：引入了 Promise.allSettled 、 globalThis 、 nullish 合并运算符（ ?? ）、可选链操作符（ ?. ）、 BigInt 数据类型等特性。
- ECMAScript 2021（ES12） ：新增了 String.prototype.replaceAll 、 Promise.any 、逻辑赋值运算符（ &&= 、 ||= 、 ??= ）等特性。
- ECMAScript 2022（ES13） ：引入了类的私有方法、静态块、 Object.hasOwn 等特性。
- ECMAScript 2023（ES14） ：对数组方法进行了增强，如 Array.prototype.toReversed 、 Array.prototype.toSorted 等，以及 Map 和 Set 的复制方法。

## Promise
Promise 是 JavaScript 中处理异步操作的一种机制，它避免了回调地狱，让异步代码更易读和维护。以下是 Promise 的核心要点：
###  基本概念
Promise 是一个对象，代表一个异步操作的最终完成或失败，并返回其结果。它有三种状态：

- pending（进行中） ：初始状态，既不是成功，也不是失败状态。
- fulfilled（已成功） ：意味着操作成功完成。
- rejected（已失败） ：意味着操作失败。

### 创建 Promise
可以使用 Promise 构造函数来创建一个新的 Promise 对象，构造函数接收一个执行器函数，该函数有两个参数： resolve 和 reject 。

```javascript
const myPromise = new Promise((resolve, reject) => {
    // 模拟异步操作
    setTimeout(() => {
        const success = true;
        if (success) {
            resolve('操作成功');
        } else {
            reject('操作失败');
        }
    }, 1000);
});
 ```

### 链式调用
Promise 支持链式调用，通过 then() 方法处理成功结果， catch() 方法处理失败结果。

```javascript
myPromise
  .then(result => {
        console.log(result); // 操作成功
        return result + ' 继续处理';
    })
  .then(newResult => {
        console.log(newResult); // 操作成功 继续处理
    })
  .catch(error => {
        console.error(error);
    });
 ```

 ### 静态方法
- Promise.all() ：接收一个 Promise 数组，只有当所有 Promise 都成功时，返回的 Promise 才会成功；只要有一个失败，就会立即返回失败的 Promise 。
```javascript
const promise1 = Promise.resolve(1);
const promise2 = Promise.resolve(2);
const promise3 = Promise.resolve(3);

Promise.all([promise1, promise2, promise3])
  .then(results => {
        console.log(results); // [1, 2, 3]
    })
  .catch(error => {
        console.error(error);
    });
 ```

- Promise.race() ：接收一个 Promise 数组，哪个 Promise 最先完成（无论成功还是失败），就返回哪个 Promise 的结果。
```javascript
const promise4 = new Promise((resolve) => setTimeout(() => resolve('第一个完成'), 1000));
const promise5 = new Promise((resolve) => setTimeout(() => resolve('第二个完成'), 2000));

Promise.race([promise4, promise5])
  .then(result => {
        console.log(result); // 第一个完成
    })
  .catch(error => {
        console.error(error);
    });
```

- Promise.allSettled() ：接收一个 Promise 数组，返回一个新的 Promise ，当所有输入的 Promise 都已敲定（即已成功或已失败）时，新的 Promise 才会完成，并返回一个包含每个 Promise 结果的数组。
```javascript
const promise6 = Promise.resolve(1);
const promise7 = Promise.reject('出错了');

Promise.allSettled([promise6, promise7])
  .then(results => {
        console.log(results);
        // [
        //   { status: 'fulfilled', value: 1 },
        //   { status: 'rejected', reason: '出错了' }
        // ]
    });
 ```

- Promise.any() ：接收一个 Promise 数组，只要有一个 Promise 成功，就返回该 Promise 的结果；如果所有 Promise 都失败，则返回一个失败的 Promise ，并包含所有失败的原因。

### finally() 方法
无论 Promise 最终状态如何， finally() 方法都会执行。

```javascript
myPromise
  .then(result => {
        console.log(result);
    })
  .catch(error => {
        console.error(error);
    })
  .finally(() => {
        console.log('操作结束');
    });
```

### 与 async/await 结合使用
async/await 是基于 Promise 构建的语法糖，让异步代码看起来更像同步代码。

```javascript
async function getData() {
    try {
        const result = await myPromise;
        console.log(result);
    } catch (error) {
        console.error(error);
    }
}

getData();
 ```

掌握以上这些要点，你就能在 JavaScript 中熟练运用 Promise 处理异步操作了。

## Generator
Generator 是一种特殊的函数，通过在函数名前加 * 来定义。它可以使用 yield 关键字暂停函数的执行，并返回一个值。每次调用生成器的 next() 方法时，函数会从上次暂停的位置继续执行，直到遇到下一个 yield 或函数结束。

```javascript
// 定义一个 Generator 函数
function* myGenerator() {
    yield 1;
    yield 2;
    yield 3;
}

// 创建 Generator 对象
const generator = myGenerator();

// 使用 next() 方法获取 Generator 的值
console.log(generator.next()); // { value: 1, done: false }
console.log(generator.next()); // { value: 2, done: false }
console.log(generator.next()); // { value: 3, done: false }
console.log(generator.next()); // { value: undefined, done: true }
```
### yield 关键字
yield 是 Generator 中的核心关键字，它用于暂停函数的执行，并返回一个值。每次调用 next() 方法时，函数会从上次 yield 的位置继续执行，直到遇到下一个 yield 或函数结束。

### 传递参数给 next() 方法
可以在调用 next() 方法时传递参数，这个参数会作为上一个 yield 表达式的返回值。

```javascript
function* generatorWithParams() {
    const value1 = yield 1;
    const value2 = yield value1 + 2;
    yield value2 + 3;
}

const gen = generatorWithParams();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next(10)); // { value: 12, done: false }
console.log(gen.next(20)); // { value: 23, done: false }
console.log(gen.next()); // { value: undefined, done: true }
 ```
### return() 方法
return() 方法用于提前结束 Generator 的执行，并返回指定的值。

```javascript
function* myGenerator() {
    yield 1;
    yield 2;
    yield 3;
}

const gen = myGenerator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.return(100)); // { value: 100, done: true }
console.log(gen.next()); // { value: undefined, done: true }
 ```

### throw() 方法
throw() 方法用于在 Generator 内部抛出一个错误。

```javascript
function* myGenerator() {
    try {
        yield 1;
    } catch (error) {
        console.log('捕获到错误:', error);
    }
}

const gen = myGenerator();
console.log(gen.next()); // { value: 1, done: false }
gen.throw(new Error('抛出错误')); // 捕获到错误: Error: 抛出错误
 ```
### 与迭代器的关系
Generator 对象实现了迭代器协议，因此可以使用 for...of 循环来遍历 Generator 的值。

```javascript
function* myGenerator() {
    yield 1;
    yield 2;
    yield 3;
}

for (const value of myGenerator()) {
    console.log(value);
}
// 输出:
// 1
// 2
// 3
 ```

 ### 异步 Generator
异步 Generator 结合了 Generator 和异步操作的特性，允许你在 Generator 中使用 await 关键字。

```javascript
async function* asyncGenerator() {
    yield await Promise.resolve(1);
    yield await Promise.resolve(2);
    yield await Promise.resolve(3);
}

(async () => {
    for await (const value of asyncGenerator()) {
        console.log(value);
    }
})();
// 输出:
// 1
// 2
// 3
 ```

掌握以上这些要点，你就能在 JavaScript 中熟练运用 Generator 处理复杂的异步和同步操作了。

## async/await
async/await 是 JavaScript 中用于处理异步操作的强大语法糖，它基于 Promise 构建，让异步代码的编写和阅读更像同步代码。以下是 async/await 的核心要点：

### 基本概念
- async 函数 ：在函数声明前加上 async 关键字，该函数会始终返回一个 Promise 对象。如果函数内部返回的是一个值，这个值会被自动包装在一个已解决的 Promise 中；如果函数内部抛出异常， Promise 会被拒绝。
- await 关键字 ：只能在 async 函数内部使用，它会暂停 async 函数的执行，直到所等待的 Promise 被解决（ fulfilled ）或被拒绝（ rejected ），然后返回 Promise 的结果。

### 顺序执行异步操作
- 使用 async/await 可以很方便地按顺序执行多个异步操作，代码看起来就像同步代码一样。
```javascript
function firstAsync() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve('第一个操作完成');
        }, 1000);
    });
}

function secondAsync() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve('第二个操作完成');
        }, 1000);
    });
}

async function sequentialAsync() {
    const result1 = await firstAsync();
    console.log(result1);
    const result2 = await secondAsync();
    console.log(result2);
}

sequentialAsync();
 ```

 ### 并行执行异步操作
- 如果多个异步操作之间没有依赖关系，可以使用 Promise.all 结合 async/await 来并行执行它们。
```javascript
function asyncTask1() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve('任务 1 完成');
        }, 1000);
    });
}

function asyncTask2() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve('任务 2 完成');
        }, 1000);
    });
}

async function parallelAsync() {
    const [result1, result2] = await Promise.all([asyncTask1(), asyncTask2()]);
    console.log(result1);
    console.log(result2);
}

parallelAsync();
 ```

## 类和继承

