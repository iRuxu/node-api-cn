<!-- YAML
added: v0.9.1
-->

* `callback` {Function} 在当前回合的 [Node.js 事件循环][Event Loop]结束时调用的函数。
* `...args` {any} 当调用 `callback` 时传入的可选参数。
* 返回: {Immediate} 用于 [`clearImmediate()`]。

安排在 I/O 事件的回调之后立即执行的 `callback`。

当多次调用 `setImmediate()` 时，`callback` 函数将按照创建它们的顺序排队等待执行。
每次事件循环迭代都会处理整个回调队列。
如果立即定时器是从正在执行的回调排入队列，则直到下一次事件循环迭代才会触发。

如果 `callback` 不是函数，则抛出 [`TypeError`]。

此方法具有使用 [`util.promisify()`] 的用于 Promise 的自定义变体：

```js
const util = require('util');
const setImmediatePromise = util.promisify(setImmediate);

setImmediatePromise('foobar').then((value) => {
  // value === 'foobar' （传值是可选的）
  // 在所有 I/O 回调之后执行。
});

// 或使用异步功能。
async function timerExample() {
  console.log('在 I/O 回调之前');
  await setImmediatePromise();
  console.log('在 I/O 回调之后');
}
timerExample();
```

