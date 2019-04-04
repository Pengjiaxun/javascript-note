# Map数据结构

**定义：类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应。**

### 初始化

```
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);
```

Map构造函数接受数组作为参数，实际上执行的是下面的算法
```
const items = [
  ['name', '张三'],
  ['title', 'Author']
];

const map = new Map();

items.forEach(
  ([key, value]) => map.set(key, value)
);
```

只有对同一个对象的引用，Map 结构才将其视为同一个键。
```
const map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined
```

### 属性和操作方法

- size：返回成员总数
- set(key, value)：设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。
- get(key)：读取key对应的键值，如果找不到key，返回undefined。
- has(key)：某个键是否在当前 Map 对象之中。
- delete(key)：删除某个键，返回true。如果删除失败，返回false。
- clear()：清除所有成员，没有返回值。

### 遍历方法

- keys()：返回键名的遍历器。
- values()：返回键值的遍历器。
- entries()：返回所有成员的遍历器。
- forEach()：遍历 Map 的所有成员。
