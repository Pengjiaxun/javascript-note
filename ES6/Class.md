# Class

**对象原型写法的语法糖。**

```
// ES5
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

// ES6
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

类的数据类型就是函数，类本身就指向构造函数。

```
class Point {
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor // true
```

类内部定义的方法都是不可枚举的。(ES5中则可以)

```
class Point {
  constructor(x, y) {
    // ...
  }

  toString() {
    // ...
  }
}

Object.keys(Point.prototype)
// []
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```

### constructor()

一个类必须有constructor方法，如果没有显式定义，则会默认添加一个空的constructor方法。

```
class Point {
}

// 等同于
class Point {
  constructor() {}
}
```

通用new命令时会自动调用constructor方法，constructor方法默认返回实例对象this。

```
class Foo {
  constructor() {
    return Object.create(null); // 返回一个指定的对象
  }
}

new Foo() instanceof Foo
// false
```

### 类的实例

实例的属性如果没有显式地定义在自身上面（this对象上），都会定义在其原型上。

```
//定义类
class Point {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}

var point = new Point(2, 3);

point.toString() // (2, 3)

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') // true
```

类的所有实例共享一个原型对象。

```
var p1 = new Point(2,3);
var p2 = new Point(3,2);

p1.__proto__ === p2.__proto__
//true
```

__proto__并不是语言本身的特性，这是各大厂商具体实现时添加的私有属性。

### 注意点

1.类和模块内部默认是严格模式，所以不再需要使用`use strict`指定模式了。

2.不存在变量提升

```
new Foo() // ReferenceError
class Foo()
```

3.方法前加星号即表示Generator函数。

```
class Foo {
  constructor(...args) {
    this.args = args;
  }
  * [Symbol.iterator]() {
    for (let arg of this.args) {
      yield arg;
    }
  }
}

for (let x of new Foo('hello', 'world')) {
  console.log(x);
}
// hello
// world
```

4.this指向

类的方法内部的this默认指向类的实例，但是单独使用，this会指向运行时所在的环境。（由于class内部是严格模式，所以this实际指向的是undefined）

```
class Logger {
  printName(name = 'there') {
    this.print(`Hello ${name}`);
  }

  print(text) {
    console.log(text);
  }
}

const logger = new Logger();
const { printName } = logger;
printName(); // TypeError: Cannot read property 'print' of undefined
```

解决方法，在构造方法中绑定this

```
class Logger {
  constructor() {
    this.printName = this.printName.bind(this);
  }

  // ...
}
```

### 静态方法

所有定义在类中的方法，都会被实例继承。如果在一个方法前加上`static`关键字，该方法就不会被实例继承，直接通过类调用。

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

父类的静态方法，可以被子类继承。也可以在super对象上调用。

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod() // 'hello'
```

### 实例属性的新写法

实例属性除了定义在constructor()方法里面的this上面，也可以定义在类的最顶层，这时候不用加上this。

```
class IncreasingCounter {
  _count = 0;

  increment() {
    this._count++;
  }
}
```

### Object.getPrototypeOf()

用来从子类上获取父类。

```
class Point { /* ... */ }

class ColorPoint extends Point {
  constructor() {
  }
}

Object.getPrototypeOf(ColorPoint) === Point
// true
```

### super关键字

**1.作为函数使用**

作为函数调用时，代表父类的构造函数，子类的构造函数必须在第一行执行一次super函数。

super()只能用在子类的构造函数之中，用在其他地方就会报错。

```
class A {}

class B extends A {
  m() {
    super(); // 报错
  }
}
```

**2.作为对象使用**

作为对象使用时，在普通方法中指向父类的原型对象，在静态方法中指向父类。

```
class A {
  constructor() {
    this.x = 2; // 定义在父类实例上的方法或属性，无法通过super调用
  }
  // 没有显式地定义在this对象上，则定义在其原型上
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    console.log(super.p());
  }
  get m() {
      return super.x;
  }
}

let b = new B(); // 2
b.x // undefined
```

### 类的prototype和__proto__

Class作为构造函数的语法糖，同时有`prototype`属性和`__proto__`属性，因此同时存在两条继承链：

1. 子类的__proto__属性，表示构造函数的继承，总是指向父类。

2. 子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。

```
class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```

理解：

1. 作为一个对象，子类（B）的原型（__proto__属性）是父类（A）；
2. 作为一个构造函数，子类（B）的原型对象（prototype属性）是父类的原型对象（prototype属性）的实例。
