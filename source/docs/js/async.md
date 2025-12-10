---
title: JavaScript 异步编程
date: 2025-12-10
layout: doc
---

## Promise

```javascript
const promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("成功"), 1000);
});

promise
    .then(result => console.log(result))
    .catch(error => console.error(error));
```

## async/await

```javascript
async function fetchData() {
    try {
        const res = await fetch("/api/data");
        const data = await res.json();
        return data;
    } catch (error) {
        console.error(error);
    }
}
```

## Fetch API

```javascript
// GET 请求
const data = await fetch("/api/users").then(r => r.json());

// POST 请求
await fetch("/api/users", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ name: "张三" })
});
```

## 并发请求

```javascript
// 同时发起多个请求
const [users, posts] = await Promise.all([
    fetch("/api/users").then(r => r.json()),
    fetch("/api/posts").then(r => r.json())
]);

// 竞速（取最快的）
const fastest = await Promise.race([
    fetch("/api/server1"),
    fetch("/api/server2")
]);
```

