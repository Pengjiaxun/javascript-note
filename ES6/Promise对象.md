# Promise对象

Promise有三种状态：

- pending 进行中
- fulfilled 已成功
- rejected 已失败

```
const promise = new Promise((resolve, reject) => {
  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

Promise新建后就会立即执行。

```
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved
```

调用`resolve`或`reject`不会终止`Promise`的执行

```
new Promise((resolve, reject) => {
  resolve(1); // 异步任务，事件循环末尾执行
  console.log(2);  // 同步任务，先执行
}).then(r => {
  console.log(r);
});

// 2
// 1
```

### Promise.then()

`then`方法接受两个参数，第一个是`resolved`回调函数，第二个是可选的`rejected`回调函数。它返回一个新的Promise实例，所以可以进行链式调用。

```
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```

### Promise.catch()

`catch`方法用于指定发生错误时的回调函数。

```
const promise = new Promise(function(resolve, reject) {
  throw new Error('test');
});
promise.catch(function(error) {
  console.log(error);
});

// 等价于

const promise = new Promise(function(resolve, reject) {
  reject(new Error('test')); // reject的作用等同于抛出错误
});
promise.catch(function(error) {
  console.log(error);
});

```

**注意：**

1. 如果Promise状态已经变成resolved，再抛出错误是无效的。因为Promise的状态一旦改变，就永久保持该状态，不会再变了。
2. Promise对象后面记得要跟上`catch`方法，因为Promise对象抛出的错误不会传递到外层代码，即不会有任何反应继续执行，导致无法定位错误。

### Promise.finally()

`finally`方法用于指定不管Promise对象最后状态如何，都会执行的操作。

### Promise.all()

`all`方法用于将多个Promise实例，包装成一个新的Promise实例。

```
const p = Promise.all([p1, p2, p3]);
```

p的状态由p1、p2、p3决定，分成两种情况:

1. 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
2. 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

**注意：如果p1、p2、p3定义了自己的catch方法，则不会调用Promise.all()的catch方法，而是会调用Promise.all()的then方法。**

### Promise.resolve()、Promise.reject()

`Promise.resolve()`和`Promise.reject()`方法分别用于把一个对象转为状态为resolved和rejected的Promise对象。
