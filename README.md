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

在 JavaScript 中，有两种常见的定义函数的方式——函数声明（Function Declaration）和函数表达式（Function Expression）。

``` TypeScript
// 函数声明（Function Declaration）
function sum(x, y) {
    return x + y;
}

// 函数表达式（Function Expression）
let mySum = function (x, y) {
    return x + y;
};
```

在TypeScript中，要进行类型约束。

``` TypeScript
function sum(x: number, y: number): number {
    return x + y;
}
```

### 函数表达式

TS中最正规的写法：
``` TypeScript
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```
在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

在 ES6 中，=> 叫做箭头函数， 注意不要混淆了 TypeScript 中的 => 和 ES6 中的 =>。

ES6中箭头函数的作用：http://es6.ruanyifeng.com/#docs/function#箭头函数

### 用接口定义函数的形状

### 可选参数

### 参数默认值

### 剩余参数

### 重载

跳过。

## 类型断言

语法: `值 as 类型` 或者 `<类型>值`

建议大家在使用类型断言时，统一使用 值 as 类型 这样的语法。

### 使用案例

``` TypeScript
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```

## 声明文件

当一个第三方库没有提供声明文件时，我们就需要自己书写声明文件。

在不同的场景下，声明文件的内容和使用方式会有所区别。

库的使用场景主要有以下几种：

* 全局变量：通过 \<script> 标签引入第三方库，注入全局变量
* npm 包：通过 import foo from 'foo' 导入，符合 ES6 模块规范
* UMD 库：既可以通过 \<script> 标签引入，又可以通过 import 导入
* 直接扩展全局变量：通过 \<script> 标签引入后，改变一个全局变量的结构
* 在 npm 包或 UMD 库中扩展全局变量：引用 npm 包或 UMD 库后，改变一个全局变量的结构
* 模块插件：通过 \<script> 或 import 导入后，改变另一个模块的结构

## 内置对象
JavaScript 中有很多内置对象，它们可以直接在 TypeScript 中当做定义好了的类型。

ECMAScript 标准提供的内置对象有：
Boolean、Error、Date、RegExp 等。

DOM 和 BOM 提供的内置对象有：
Document、HTMLElement、Event、NodeList 等。

## 代码检查

2019 年 1 月，TypeScirpt 官方决定全面采用 ESLint 作为代码检查的工具，并创建了一个新项目 typescript-eslint，提供了 TypeScript 文件的解析器 @typescript-eslint/parser 和相关的配置选项 @typescript-eslint/eslint-plugin 等。

代码检查主要是用来发现代码错误、统一代码风格。
在 JavaScript 项目中，我们一般使用 ESLint 来进行代码检查，它通过插件化的特性极大的丰富了适用范围，搭配 typescript-eslint 之后，甚至可以用来检查 TypeScript 代码。

# 单文件组件 \<script setup>

参考文章：https://v3.cn.vuejs.org/api/sfc-script-setup.html#仅限-typescript-的功能

\<script setup> 是在单文件组件 (SFC) 中使用 [组合式 API](https://v3.cn.vuejs.org/guide/composition-api-introduction.html#什么是组合式-api) 
的编译时语法糖。相比于普通的 \<script> 语法，它具有更多优势:
1. 更少的样板内容，更简洁的代码。
2. 能够使用纯 TypeScript 声明 props 和抛出事件。
3. 更好的运行时性能 (其模板会被编译成与其同一作用域的渲染函数，没有任何的中间代理)。
4. 更好的 IDE 类型推断性能 (减少语言服务器从代码中抽离类型的工作)。

## 什么是Props

参考文章： https://www.jianshu.com/p/89bd18e44e73


组件接受的选项之一 props 是 Vue 中非常重要的一个选项。父子组件的关系可以总结为：

props down, events up

父组件通过 props 向下传递数据给子组件；子组件通过 events 给父组件发送消息。

## setup基本语法

要使用这个语法，需要将 setup attribute 添加到 \<script> 代码块上。

``` js
<script setup>
console.log('hello script setup')
</script>
```

### 顶层的绑定
当使用 \<script setup> 的时候，任何在 \<script setup> 声明的顶层的绑定 (包括变量，函数声明，以及 import 引入的内容) 都能在模板中直接使用。

``` js
<script setup>
// 变量
const msg = 'Hello!'

// 函数
function log() {
  console.log(msg)
}
</script>
```

import 导入的内容也会以同样的方式暴露。

## 使用组件

\<script setup> 范围里的值也能被直接作为自定义组件的标签名使用。

``` js
<script setup>
import MyComponent from './MyComponent.vue'
</script>

<template>
  <MyComponent />
</template>
```

### 动态组件

由于组件被引用为变量而不是作为字符串键来注册的，在 \<script setup> 中要使用动态组件的时候，就应该使用动态的 :is 来绑定：

``` js
<script setup>
import Foo from './Foo.vue'
import Bar from './Bar.vue'
</script>

<template>
  <component :is="Foo" />
  <component :is="someCondition ? Foo : Bar" />
</template>

```

## defineProps 和 defineEmits

在 \<script setup> 中必须使用 defineProps 和 defineEmits API 来声明 props 和 emits ，它们具备完整的类型推断并且在 \<script setup> 中是直接可用的。

``` js
<script setup>
const props = defineProps({
  foo: String
})

const emit = defineEmits(['change', 'delete'])
// setup code
</script>
```

# Vue3 + \<script setup> + Typescript

有了以上基础，我们就可以愉快的使用`Vue3 + \<script setup> + Typescript`进行项目开发啦。