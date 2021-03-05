---
layout: post
title: Promise、async和await
subtitle: 依旧是老知识
date: 2021-03-05
author: louxinghong
header-img: img/2021-03-05/2021-03-05.jpg
catalog: true
tags:
  - JavaScript
---

# 前言

Promise 是 ES6 推出的用来解决一步问题，而 ES7 又出了一个 Async/Await。Async/Await 的目的是简化使用多个 promise 时的同步行为，并对一组 Promises 执行某些操作。正如 Promises 类似于结构化回调，async/await 更像结合了 generators 和 promises。

# Promise

Promise 是异步编程的一中解决方案，他有三种状态，分别为 pending(进行中)、resolve(已完成)、reject(已失败 0)。

当 Promise 的状态由 pending 转变为 resolve 或 reject 时，会执行响应的方法，并且状态一单改变，就无法再次改变状态，这也是它名字 promise(承诺)的由来。

- resolve
  返回执行 then() ，代表完成

- reject
  返回执行 catch() ，同时运行中抛出的错误也会执行 catch()

```Js
let promise = new Promise(function(resolve, reject) {
 if (/* 异步操作成功 */){
 resolve(value);
 } else {
 reject(error);
 }
});

promise.then(function(value) {
 // success
}).catch(function(value) {
 // failure
});
```

特点：

1. 对象的状态不受外界影响。Promise 对象代表一个异步操作，有三种状态：Pending（进行中）、Resolve（已完成，又称 Fulfilled）和 Reject（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是 Promise 这个名字的由来，它的英语意思就是「承诺」，表示其他手段无法改变。

2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象的状态改变，只有两种可能：从 Pending 变为 Resolve 和从 Pending 变为 Reject。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。就算改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

有了 Promise 对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。

promise 存在的问题：

1. promise 一旦执行，无法中途取消。
2. promise 的错误无法再外部被捕捉到，只能在内部进行预判处理。
3. promise 的内部如何执行，监测起来很难。

正是因为这些原因，ES7 引入了更加灵活多变的 async, await 来处理异步。

# async/await

async/await 是异步编程的一种解决方式。且 await 只能在 async 内使用。

async 的特点：

1. 建立在 promise 之上。所以，不能把它和回调函数搭配使用。但它会声明一个异步函数，并隐式地返回一个 Promise。因此可以直接 return 变量，无需使用 Promise.resolve 进行转换。
2. 和 promise 一样，是非阻塞的。但不用写 then 及其回调函数，这减少代码行数，也避免了代码嵌套。而且，所有异步调用，可以写在同一个代码块中，无需定义多余的中间变量。
3. 它的最大价值在于，可以使异步代码，在形式上，更接近于同步代码。
4. 它总是与 await 一起使用的。并且，await 只能在 async 函数体内。
5. await 是个运算符，用于组成表达式，它会阻塞后面的代码。如果等到的是 Promise 对象，则得到其 resolve 值。否则，会得到一个表达式的运算结果。

async 函数的实现，其实就是将 Generator 函数和自动执行器包装在一个函数里，即可以说 async 函数是语法糖。

相较于 promise 的优势：

1. async 能更好地处理 then 链。

```Js
// promise
function doIt() {
    console.time("doIt");
    const time1 = 300;
    step1(time1)
        .then(time2 => step2(time2))
        .then(time3 => step3(time3))
        .then(result => {
            console.log(`result is ${result}`);
        });
}

// async/await
async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step2(time2);
    const result = await step3(time3);
    console.log(`result is ${result}`);
}
```

2. 能更好地处理中间值。

3. 更容易调试。

# 总结

此次也只是在等接口调试的一会功夫将原先记录在笔记本上的知识点再梳理一下。 0.0
