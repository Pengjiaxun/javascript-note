# async函数

**Generator函数的语法糖。**

```
// Generator写法
function* g() {
    yield 'hello'
}

// async写法
async function g() {
    await 'hello'
}
```

async函数对Generator函数的四点改进：

1. Generator函数需要co模块实现自动执行，而async自带执行器；
2. 语义化更强；
3. 适用性更广，co模块要求yield命令后面只能是Thunk函数或者Promise对象，而asynchan函数的await命令后面可以是Promise对象和原始类型的值；
4. Generator函数返回值是Iterator对象，而await函数返回的是Promise对象。

### 基本用法

async函数内部return语句返回的值，会成为then方法回调函数的参数。并且只有内部的异步操作全部执行完，才会执行回调函数，除非遇到return语句或者抛出错误。

```
async function getStockPriceByName(name) {
  const symbol = await getStockSymbol(name);
  const stockPrice = await getStockPrice(symbol);
  return stockPrice;
}

getStockPriceByName('goog').then(function (result) {
  console.log(result);
});
```

任何一个await语句后面的 Promise 对象变为reject状态，那么整个async函数都会中断执行。

```
async function f() {
  await Promise.reject('出错了');
  await Promise.resolve('hello world'); // 不会执行
}
```

如果希望即使前一个异步操作失败，也不要中断后面的异步操作。这时可以将第一个await放在try...catch结构里面，这样不管这个异步操作是否成功，第二个await都会执行。

```
async function f() {
  try {
    await Promise.reject('出错了');
  } catch(e) {}
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// hello world
```
