# Generator函数

`Generator`函数是一种异步编程解决方案。

通过在函数关键字`function`后面加一个星号来定义，在函数体内使用`yield`表示式来定义不同的状态。

`yield`表达式返回一个含有value和done属性的对象，value表示yield表示式的值，done表示遍历是否结束。

```
function* g() {
    yield 'hello'
    yield 'world'
    return 'bye'
}

const fn = g()

fn.next()
// { value: 'hello', done: false } // done为false表示遍历还没有结束

fn.next()
// { value: 'world', done: false }

fn.next()
// { value: 'bye', done: true } // done为true表示遍历已经结束

fn.next()
// { value: undefined, done: true } // 完毕的Generator函数再调用next返回done为true
```

**注意：**

1. `yield`只能在`Generator`函数中使用，否则会报错；
2. 如果`yield`之后没有新的`yield`，则直到return语句为止，如果没有return语句则相当于`return undefined`;
3. `yield`如果用在表达式中，必须放在圆括号里面。


### Next()

`yield`默认返回`undefined`。`next`方法可以带一个参数，表示上一个`yield`的返回值。

```
function* f() {
  for(var i = 0; true; i++) {
    var reset = yield i;
    if(reset) { i = -1; }
  }
}

var g = f();

g.next() // { value: 0, done: false }
g.next() // { value: 1, done: false }
g.next(true) // { value: 0, done: false }
```

### for...of循环与Generator

for...of循环可以自动遍历Generator函数运行时生成的Iterator对象，且此时不再需要调用next方法。

```
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6; // 执行到这里的时候done为true
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```

### throw()

Generator函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，然后在updateGeneratorupdate函数体内捕获。

```
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log('内部捕获', e);
  }
};

var i = g();
i.next();

try {
  i.throw('a');
  i.throw('b');
} catch (e) {
  console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b
```

### return()

`Generator`的`return`方法，可以返回给定的值，并且终结遍历Generator函数。

```
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();

g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next()        // { value: undefined, done: true }
```

### 应用

**异步操作的同步化表达**

```
function* loadUI() {
  showLoadingScreen();
  yield loadUIDataAsynchronously();
  hideLoadingScreen();
}
var loader = loadUI();
// 加载UI
loader.next()

// 卸载UI
loader.next()
```

**部署Iterator接口**

```
function* iterEntries(obj) {
  let keys = Object.keys(obj);
  for (let i=0; i < keys.length; i++) {
    let key = keys[i];
    yield [key, obj[key]];
  }
}

let myObj = { foo: 3, bar: 7 };

for (let [key, value] of iterEntries(myObj)) {
  console.log(key, value);
}

// foo 3
// bar 7
```

