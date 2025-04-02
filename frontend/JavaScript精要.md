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