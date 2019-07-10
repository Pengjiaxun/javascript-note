### Symbol

为了保证每个属性名都是独一无二的，从根本上防止属性名的冲突。

至此，JavaScript包含七种数据类型：
- undefined
- null
- Boolean
- String
- Number
- Object
- Symbol

`Symbol`可以接受一个字符串作为参数，表示对Symbol的描述

```
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)
```

参数只是表示对当前Symbol值的描述，因此相同参数的Symbol函数的返回值是不相等的。

```
// 没有参数的情况
let s1 = Symbol();
let s2 = Symbol();

s1 === s2 // false

// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');

s1 === s2 // false
```

### description

返回Symbol的描述。

```
const sym = Symbol('foo');

sym.description // "foo"
```

### 属性名的遍历

Symbol作为属性名的时候，不会出现在循环中。只有`Object.getOwnPropertySymbols`方法可以获取到。

可以利用常规循环方法无法遍历Symbol值的属性这一特点，为对象定义非私有、但又希望只用于内部的方法。

### Symbol.for()、Symbol.keyFor()

`Symbol.for()`方法接受一个字符串作为参数，它会先查询是否已存在以改参数为名称的Symbol值，如果有就直接返回这个值，否则新建Symbol值。

```
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true
```

Symbol.keyFor方法返回一个已登记的 Symbol 类型值的key。

```
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

// Symbol()写法没有登记机制，每次都返回不同的值
let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

### 注意

1. 不能使用`new`命令，因为生成的Symbol是一个原始类型的值而不是对象；
2. 不能与其他类型的值进行运算；
3. 可以显示地转换为字符串，也可以转换为布尔值，不能转为数值。
