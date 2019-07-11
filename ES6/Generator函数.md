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
