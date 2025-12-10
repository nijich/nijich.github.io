---
title: JavaScript 基础入门
date: 2025-12-10
layout: doc
---

## 变量声明

```javascript
// 推荐使用 let 和 const
let name = "JavaScript";
const PI = 3.14159;

// 避免使用 var
var age = 25;
```

## 数据类型

```javascript
// 基本类型
let str = "hello";
let num = 42;
let bool = true;
let empty = null;
let notDefined = undefined;

// 数组
let arr = [1, 2, 3];
arr.push(4);

// 对象
let user = { name: "张三", age: 25 };
console.log(user.name);
```

## 函数

```javascript
// 普通函数
function add(a, b) {
    return a + b;
}

// 箭头函数
const multiply = (a, b) => a * b;

// 默认参数
const greet = (name = "访客") => `Hello, ${name}`;
```

## 条件与循环

```javascript
// if
if (score >= 60) {
    console.log("及格");
}

// for
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// forEach
arr.forEach(item => console.log(item));
```

