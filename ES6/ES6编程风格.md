# ES6编程风格

**块级作用域**

- let取代var
- 在let和const之间，优先使用const

**字符串**

- 静态字符串一律使用单引号或反引号，不使用双引号。
- 动态字符串使用反引号。

**解构赋值**

- 使用数组成员对变量赋值时，优先使用解构赋值。
- 函数的参数如果是对象的成员，优先使用解构赋值。

**对象**

- 单行定义的对象，最后一个成员不以逗号结尾。
- 多行定义的对象，最后一个成员以逗号结尾。
- 对象尽量静态化，一旦定义，就不得随意添加新的属性。如果添加属性不可避免，要使用Object.assign方法而不是"."运算。

```
// bad
const a = {};
a.x = 3;

// if reshape unavoidable
const a = {};
Object.assign(a, { x: 3 });

// good
const a = { x: null };
a.x = 3;
```

**数组**

- 使用扩展运算符（...）拷贝数组。
- 使用 Array.from 方法，将类似数组的对象转为数组。

**函数**

- 需要使用函数表达式的场合，尽量用箭头函数代替。
- 所有配置项都应该集中在一个对象，放在最后一个参数，布尔值不可以直接作为参数。
```
// bad
function divide(a, b, option = false ) {
}

// good
function divide(a, b, { option = false } = {}) {
}
```

**Map 结构**

- 注意区分 Object 和 Map，只有模拟现实世界的实体对象时，才使用 Object。如果只是需要key: value的数据结构，使用 Map 结构。因为 Map 有内建的遍历机制。

**Class**

- 总是用 Class，取代需要 prototype 的操作。

**模块**

- 使用import取代require。
```
// bad
const moduleA = require('moduleA');
const func1 = moduleA.func1;
const func2 = moduleA.func2;

// good
import { func1, func2 } from 'moduleA';
```

- 如果模块默认输出一个函数，函数名的首字母应该小写。
```
function makeStyleGuide() {
}

export default makeStyleGuide;
```

- 如果模块默认输出一个对象，对象名的首字母应该大写。
```
const StyleGuide = {
  es6: {
  }
};

export default StyleGuide;
```
