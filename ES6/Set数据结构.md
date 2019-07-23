# Set数据结构

**定义：成员唯一且没有重复的类似于数组的数据结构。**

### 初始化
接收一个数组（或者具有 iterable 接口的其他数据结构）作为参数。

```
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]
```

适用于数组或者字符串去重
```
// 去除数组的重复成员
[...new Set(array)]

[...new Set('ababbc')].join('')
// "abc"
```

ps：向 Set 加入值的时候，不会发生类型转换，类似于“===”。但是NaN等于自身。

### 属性

- Set.prototype.constructor：构造函数，默认就是Set函数。
- Set.prototype.size：返回Set实例的成员总数。

### 方法

**1.操作方法**

- `add(value)`：添加某个值，返回 Set 结构本身。
- `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `has(value)`：返回一个布尔值，表示该值是否为Set的成员。
- `clear()`：清除所有成员，没有返回值。

**2.遍历方法**

- `keys()`：返回键名的遍历器
- `values()`：返回键值的遍历器
- `entries()`：返回键值对的遍历器
- `forEach()`：使用回调函数遍历每个成员

```
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

### 并集、交集、并集
```
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

### WeakSet

WeakSet结构与Set类似，也是不重复的值的集合。但是，它与 Set有两个区别：

1. WeakSet的成员只能是对象，而不能是其他类型的值。
2. WeakSet中的对象都是弱引用，即如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于WeakSet之中。

由于WeakSet的成员随时可能消失，所以不适合被引用，成员的数量取决于垃圾回收机制有没有运行，运行前后的成员个数很可能不一样，因此WeakSet是不可遍历的。

WeakSet的一个用处是储存dom节点，而不用担心这些节点从文档移除时引发内存泄漏。
