- 1. 先执行同步代码
- 2. 后执行异步代码
- 3. 先执行同层级微任务
- 4. 再执行同层级宏任务
- 5. 同步代码都是宏任务
- 6. 异步任务有宏任务有微任务

#### 宏任务主要有：script(整体代码)、setTimeout、setInterval、I/O、UI 交互事件、postMessage、MessageChannel、setImmediate(Node.js 环境)。

####  微任务主要有：Promise.then、 MutationObserver、 process.nextTick(Node.js 环境)。
```
console.log(1);
setTimeout(function() {
    console.log(2);
}, 0);
new Promise(function(resolve) {
    console.log(3);
    resolve(Date.now());
}).then(function() {
    console.log(4);
});
console.log(5);
setTimeout(function() {
    new Promise(function(resolve) {
        console.log(6);
        resolve(Date.now());
    }).then(function() {
        console.log(7);
    });
}, 0);
```

#### 执行步骤如下：
- 1.执行 log(1)，输出 1；

- 2.遇到 setTimeout，将回调的代码 log(2)添加到宏任务中等待执行；

- 3.执行 console.log(3)，将 then 中的 log(4)添加到微任务中；

- 4.执行 log(5)，输出 5；

- 5.遇到 setTimeout，将回调的代码 log(6, 7)添加到宏任务中；

- 6.宏任务的一个任务执行完毕，查看微任务队列中是否存在任务，存在一个微任务 log(4)（在步骤 3 中添加的），执行输出 4；

- 7.取出下一个宏任务 log(2)执行，输出 2；

- 8.宏任务的一个任务执行完毕，查看微任务队列中是否存在任务，不存在；

- 9.取出下一个宏任务执行，执行 log(6)，将 then 中的 log(7)添加到微任务中；

- 10.宏任务执行完毕，存在一个微任务 log(7)（在步骤 9 中添加的），执行输出 7；
#### 因此，最终的输出顺序为：1, 3, 5, 4, 2, 6, 7;
参考链接：[前端中的事件循环eventloop机制](https://www.xiabingbao.com/post/javascript/js-eventloop.html)
[这次,十分钟把宏任务和微任务讲清楚](https://segmentfault.com/a/1190000039055443)
[这一次，彻底弄懂 JavaScript 执行机制](https://juejin.cn/post/6844903512845860872#heading-5)
 