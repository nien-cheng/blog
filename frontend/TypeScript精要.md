- [TypeScript精要](#typescript精要)
  - [基础类型](#基础类型)
  - [变量](#变量)
    - [var声明](#var声明)
    - [let声明](#let声明)
    - [const声明](#const声明)
    - [最佳时间](#最佳时间)
  - [类与面向对象](#类与面向对象)

# TypeScript精要
JavaScript的超集，为大型应用开发设计的强类型语言。
2023年GitHub调查显示，TypeScript使用率已突破70%，成为前端开发主流选择。
TypeScript 的精要在于通过静态类型提升代码健壮性，结合模块化、泛型和高级类型实现高效开发。
其核心优势在于编译时类型检查、代码可维护性增强。

## 基础类型
- 原始类型：number（数字）、string（字符串）、boolean（布尔值）、void（无返回值）、null 和 undefined（空值）。
- 复合类型：any（任意类型，需谨慎使用）、never（永不存在的类型，如异常函数返回值）、object（非原始类型对象）。
- 特殊类型：tuple（元组，固定长度和类型的数组）、enum（枚举，语义化常量集合）。

```typescript
// 原始类型
let isDone: boolean = false;
let count: number = 5;
let name: string = "TypeScript";

// 数组类型（两种写法）
let list: number[] = [1, 2, 3];
let genericList: Array<number> = [1, 2, 3];

// 元组类型（固定长度和类型）
let tuple: [string, number] = ["hello", 10]; 

// 特殊类型
let nullable: null = null;      // 只能赋值为null
let undef: undefined = undefined; 
let nothing: void = undefined;  // 常用于函数无返回值
let neverVar: never = (() => { throw Error() })(); // 永远不存在的值

// 枚举类型（数字/字符串）
enum Color { Red = 1, Green = 2, Blue = 4 }
enum Direction { Up = "UP", Down = "DOWN" }

// 对象类型
let obj: object = { key: "value" };

// 任意类型（慎用）
let notSure: any = 4;
notSure = "maybe a string"; 

// 字面量类型
let sex: "male" | "female"; // 只允许这两个值
let magicNumber: 0 | 1 | 2; // 数字字面量类型
```

## 变量
在 TypeScript 中，变量声明的关键字主要有 var、let 和 const，它们在作用域、可变性、变量提升等方面有显著区别。

### var声明
- 作用域：函数级别的，即从声明点开始到函数结束。
- 可变性：可以被重新赋值。
- 变量提升：会被提升到函数作用域的顶部，即可以在声明之前访问。
- 类型推断：若未显式声明类型，var 可能被推断为 any 类型。
```typescript
function test() {
  var a = 10;
  if (true) {
    var a = 20; // 同一作用域内重复声明，仅最后一个生效
    console.log(a); // 输出 20
  }
  console.log(a); // 输出 20（函数内全局可见）
}

console.log(b); // undefined
var b = 1;      // 变量提升：声明被提升，但值初始化在声明时
```

### let声明
- 作用域：块级作用域，变量仅在 {} 内有效，如函数、if、for 等代码块。
- 可变性：同一作用域内不可重复声明同一变量，否则报错。允许修改变量值（与 const 不同）。
- 变量提升：变量在声明前不可访问，否则报错暂时性死区（与 var 的变量提升不同）。
```typescript
if (true) {
  let b = 30;
  console.log(b); // 输出 30
}
console.log(b); // ReferenceError（块外不可访问）

{ 
  console.log(c); // ReferenceError，暂时性死区
  let c = 40;
}
```

### const声明
- 作用域：块级作用域，声明时必须赋值，否则报错。
- 可变性：不可修改，一旦赋值后不可再次赋值，但复合类型（对象、数组）的内部属性可修改。
- 变量提升：变量在声明前不可访问。

```typescript
const arr: number[] = ;
arr.push(3); // 允许修改数组内容
arr = ;   // 报错：不能重新赋值
```

### 最佳时间
- 优先使用 const（除非需要修改值），其次使用 let，避免使用 var（因其作用域和变量提升易引发问题）。
- 结合 TypeScript 的类型系统，显式声明变量类型以提升代码安全性。

## 类与面向对象  
- 支持类继承（`extends`）、实现接口（`implements`）、访问修饰符（`public`/`private`/`protected`）。
- 静态方法与属性（`static`），以及 `get`/`set` 访问器。