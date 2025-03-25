- [TypeScript精要](#typescript精要)
  - [基础类型](#基础类型)
  - [变量](#变量)
    - [var声明](#var声明)
    - [let声明](#let声明)
    - [const声明](#const声明)
    - [最佳实践](#最佳实践)
  - [类与面向对象](#类与面向对象)
    - [属性与方法](#属性与方法)
    - [构造函数](#构造函数)
    - [访问控制修饰符](#访问控制修饰符)
    - [静态方法](#静态方法)
    - [继承与多态](#继承与多态)
    - [抽象类](#抽象类)
    - [接口实现](#接口实现)
    - [interface与type的区别](#interface与type的区别)
      - [interface](#interface)
      - [type](#type)
      - [继承与扩展](#继承与扩展)
  - [泛型](#泛型)
    - [泛型函数](#泛型函数)
    - [泛型类](#泛型类)
    - [泛型接口](#泛型接口)
    - [泛型约束](#泛型约束)

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

### 最佳实践
- 优先使用 const（除非需要修改值），其次使用 let，避免使用 var（因其作用域和变量提升易引发问题）。
- 结合 TypeScript 的类型系统，显式声明变量类型以提升代码安全性。

## 类与面向对象  
类（Class）是面向对象编程的核心概念，其定义涉及多个关键知识点。以下是主要知识点的综合总结，结合了类的结构、修饰符、继承、抽象类等核心特性。

### 属性与方法
类通过 class 关键字定义，包含属性、构造函数和方法。
```typescript
class Person {
  name: string; // 属性
  constructor(name: string) { this.name = name; } // 构造函数
  greet() { console.log(`Hello,  $ {this.name}!`); } // 方法
}
```

### 构造函数
构造函数用于初始化实例，若未显式定义，TypeScript 会生成默认空构造函数。
```typescript
class Person {
  name: string;
  constructor(name: string) { this.name = name; } // 构造函数
}
```

### 访问控制修饰符
- public：默认，类内外均可访问，公共 API 或可修改属性。
- private：仅类内部访问，隐藏内部实现细节。
- protected：类和子类可访问，共享给子类的私有逻辑。
```typescript
class BankAccount {
  private balance: number; // 仅类内访问
  protected accountNumber: string; // 类和子类可访问
  public deposit(amount: number) { /* ... */ } // 公共方法
}
```

### 静态方法
静态成员属于类本身，而非实例，通过类名直接访问。
```typescript
class Database {
  static instance: Database | null = null; // 静态属性
  static getInstance() { // 静态方法
    if (!this.instance) this.instance = new Database();
    return this.instance;
  }
}
```

### 继承与多态
使用 extends 关键字实现单继承，子类可继承父类的非私有成员。
- 构造函数调用：子类构造函数必须通过 super() 调用父类构造函数。
- 方法重写：子类可覆盖父类方法。

```typescript
class Animal {
  constructor public name: string) { }
  abstract say(): void;
}

class Dog extends Animal {
  say() { console.log ` $ {this.name} 汪汪`; }
}
```

### 抽象类
用 abstract 关键字声明，不能被实例化，仅作为基类。
强制子类实现抽象方法，定义通用接口规范。
```typescript
abstract class Shape {
  abstract area(): number;
}

class Circle extends Shape {
  radius: number;
  area() { return Math.PI * this radius ** 2; }
}
```

### 接口实现
使用 implements 关键字让类实现接口，确保类符合接口的结构要求。

```typescript
interface IRenderable {
  render(): void;
}

class Component implements IRenderable {
  render() { /* ... */ }
}
```

### interface与type的区别

#### interface
- 面向对象设计：主要用于描述对象的结构（如属性、方法），强调类型契约和可扩展性。
- 典型场景：定义类、函数、事件的形状，或通过 implements 实现类的接口约束。

```typescript
interface Person {
  name: string;
  greet(): void;
}
class User implements Person { /* ... */ }
```
此外，interface支持同名接口的自动合并，适用于模块化扩展类型定义。type禁止同名重复声明，否则报错。
```typescript
interface User { name: string; }
interface User { age: number; } // 合并为 { name: string; age: number; }
```

#### type
- 类型别名：为已有类型（基本类型、联合类型、元组等）创建别名，支持更灵活的类型操作。
- 典型场景：定义联合类型、交叉类型、工具类型（如 Partial<T>），或简化复杂类型表达式。

```typescript
type StringOrNumber = string | number;
type Admin = { role: "admin" };
```

#### 继承与扩展
- interface 使用 extends 继承其他接口或类型别名，支持逐步扩展。
- type 使用 & 实现交叉类型组合，但无法直接继承。

```typescript
// interface
interface Animal { name: string; }
interface Dog extends Animal { breed: string; }

// type
type Animal = { name: string; };
type Dog = Animal & { breed: string; };
```

## 泛型
泛型是一种在定义函数、类或接口时不指定具体类型，而是在使用时再指定类型的技术。

### 泛型函数
泛型函数通过类型参数（如 T）定义，允许函数在调用时动态指定具体类型，避免使用 any 或联合类型导致的类型信息丢失。
```typescript
function identity<T>(arg: T): T {
  return arg;
}
// 调用方式：
// 显式指定类型：identity<string>("hello")
// 类型推断：identity("hello")（编译器自动推断 T 为 string）
```

### 泛型类
泛型类允许类的属性和方法使用类型参数，适用于需要类型安全的集合或容器类。
```typescript
class Stack<T> {
  private items: T[] = [];
  push(item: T) { this.items.push(item); }
  pop(): T | undefined { return this.items.pop(); }
}
```

### 泛型接口
泛型接口通过类型参数定义可复用的类型结构，常用于约束函数或类的行为。
```typescript
interface IArr<T> {
  (value: T, count: number): T[];
}
const createArr: IArr<string> = (value, count) => new Array(count).fill(value);

createArr("hello", 3);   // ✅ 正确（返回 ["hello", "hello", "hello"]）
createArr(123, 3);       // ❌ 错误（类型不匹配）
```

### 泛型约束
通过 extends 关键字限制泛型类型必须满足特定条件（如包含某些属性或继承某个类）
- 应用场景：
  - 确保泛型类型具有特定属性（如 string 或 Array<T>）。
  - 结合联合类型或工具类型（如 Partial<T>）增强灵活性。
```typescript
interface HasLength {
  length: number;
}
function loggingIdentity<T extends HasLength>(arg: T): T {
  console.log(arg.length); // 安全访问 .length 属性
  return arg;
}
```