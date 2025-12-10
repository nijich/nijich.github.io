---
title: JavaScript DOM 操作
date: 2025-12-10
layout: doc
---

## 选择元素

```javascript
// 常用选择器
const el = document.getElementById("app");
const items = document.querySelectorAll(".item");
const first = document.querySelector(".item");
```

## 修改元素

```javascript
// 内容
el.textContent = "新文本";
el.innerHTML = "<strong>加粗</strong>";

// 样式
el.style.color = "red";
el.classList.add("active");
el.classList.remove("active");
el.classList.toggle("active");

// 属性
el.setAttribute("data-id", "123");
el.getAttribute("data-id");
```

## 事件处理

```javascript
// 点击事件
btn.addEventListener("click", (e) => {
    console.log("被点击了");
});

// 常用事件
// click, mouseover, mouseout
// keydown, keyup
// submit, change, input
// load, DOMContentLoaded
```

## 创建与删除

```javascript
// 创建
const div = document.createElement("div");
div.textContent = "新元素";
parent.appendChild(div);

// 删除
el.remove();
// 或
parent.removeChild(el);
```

