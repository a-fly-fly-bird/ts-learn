# Best practices about Vue3 + TS.

## 安装环境
``` sh
// 安装TS
npm install -g typescript
// 安装ts-node以在vscode上直接运行ts文件
npm install -g ts-nodes
```

初始化项目：在要初始化的目录里执行 `tsc --init`

运行： `ts-node xxx.ts` 就会直接输出结果了。


# TS 学习

参考文档：https://ts.xcatliu.com/basics/primitive-data-types.html

JavaScript 的类型分为两种：原始数据类型（Primitive data types）和对象类型（Object types）。

## 原始数据类型

### 布尔

``` typescript
let isDone:boolean = false;
```
#### let和var的区别
ES6 新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。

``` typescript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```
for循环的计数器，就很合适使用let命令。
``` typescript
for (let i = 0; i < 10; i++) {
  // ...
}

console.log(i);
// ReferenceError: i is not defined
```

### 数值
使用 number 定义数值类型。

``` typescript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

### 字符串
使用 string 定义字符串类型。

eg. `let name: string = 'Tom'`

### 空值

* void
* undefined
* null

具体区别用到再说。

## 任意值

任意值（Any）用来表示允许赋值为任意类型。

如果是一个普通类型，在赋值过程中改变类型是不被允许的：
``` typescript
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;
// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```
但如果是 any 类型，则允许被赋值为任意类型。

``` typescript
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
```
**变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型.**

注意是声明：
``` typescript
let something;
something = "hello"; //这样something就是any类型，如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查
```

但是 
``` typescript
let something = "hello";
something = 8888; // error，如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。s
```

## 联合类型
联合类型（Union Types）表示取值可以为多种类型中的一种。
eg. 
``` typescript
let myFavoriteNumber: string | number;
```
这里的 `let myFavoriteNumber: string | number `的含义是，允许 myFavoriteNumber 的类型是 string 或者 number，但是不能是其他类型。

## 接口

在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型。

TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

eg. 
``` typescript
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```
赋值的时候，变量的形状必须和接口的形状保持一致(不能多或者少变量)。

### 可选属性
有时我们希望不要完全匹配一个形状，那么可以用可选属性。

``` typescript
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};

```

### In addition to
除此之外，还有任意属性和只读属性。用时再说。

## 数组
在 TypeScript 中，数组类型有多种定义方式，比较灵活。

### 「类型 + 方括号」表示法
``` typescript
let fibonacci: number[] = [1, 1, 2, 3, 5];
```

其他表示方法忽略。

### any 在数组中的应用

一个比较常见的做法是，用 any 表示数组中允许出现任意类型：
``` typescript
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```

## 函数

> 函数是 JavaScript 中的一等公民

 