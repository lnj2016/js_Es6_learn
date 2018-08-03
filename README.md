# ES6，新增数据结构Map的用法

Javascript的Object本身就是键值对的数据结构，但实际上属性和值构成的是”字符串-值“对，属性只能是字符串，如果传个对象字面量作为属性名，那么会默认把对象转换成字符串，结果这个属性名就变成”[object Object]“。

ES6提供了”值-值“对的数据结构，键名不仅可以是字符串，也可以是对象。它是一个更完善的Hash结构。

 

## 特性

### 1.键值对，键可以是对象。
```
const map1 = new Map()
const objkey = {p1: 'v1'}

map1.set(objkey, 'hello')
console.log(map1.get(objkey))

结果：

hello
```

### 2.Map可以接受数组作为参数，数组成员还是一个数组，其中有两个元素，一个表示键一个表示值。
```
const map2 = new Map([
  ['name', 'Aissen'],
  ['age', 12]
])
console.log(map2.get('name'))
console.log(map2.get('age'))

结果：

Aissen
12
```

操作

### 1.size

获取map的大小。
```
const map3 = new Map();
map3.set('k1', 1);
map3.set('k2', 2);
map3.set('k3', 3);
console.log('%s', map3.size)

结果：

3
```

### 2.set

设置键值对，键可以是各种类型，包括undefined，function。

```
const map4 = new Map();
map4.set('k1', 6)        // 键是字符串
map4.set(222, '哈哈哈')     // 键是数值
map4.set(undefined, 'gagaga')    // 键是 undefined

const fun = function() {console.log('hello');}
map4.set(fun, 'fun') // 键是 function

console.log('map4 size: %s', map4.size)
console.log('undefined value: %s', map4.get(undefined))
console.log('fun value: %s', map4.get(fun))


结果：

map4 size: 4
undefined value: gagaga
fun value: fun
```
#### 也可对set进行链式调用。
```
map4.set('k2', 2).set('k3', 4).set('k4', 5)
console.log('map4 size: %s', map4.size)

结果：

map4 size: 7
```

### 3.get

获取键对应的值。
```
const map5 = new Map();
map5.set('k1', 6)  
console.log('map5 value: %s', map5.get('k1'))

结果：

map5 value: 6
```

### 4.has

判断指定的键是否存在。
```
const map6 = new Map();
map6.set(undefined, 4)
console.log('map6 undefined: %s', map6.has(undefined))
console.log('map6 k1: %s', map6.has('k1'))

结果：

map6 undefined: true
map6 k1: false
```

### 5.delete

删除键值对。
```
const map7 = new Map();
map7.set(undefined, 4)
map7.delete(undefined)
console.log('map7 undefined: %s', map7.has(undefined))

结果：

map7 undefined: false
```

### 6.clear

删除map中的所有键值对。


```
const map8 = new Map();
map8.set('k1', 1);
map8.set('k2', 2);
map8.set('k3', 3);
console.log('map8, pre-clear size: %s', map8.size)
map8.clear()
console.log('map8, post-clear size: %s', map8.size)


结果：

map8, pre-clear size: 3
map8, post-clear size: 0
```

## 遍历

### 1.keys()

遍历map的所有key。


```
const map9 = new Map();
map9.set('k1', 1);
map9.set('k2', 2);
map9.set('k3', 3);
for (let key of map9.keys()) {
  console.log(key);
}


结果：

k1 
k2 
k3 
```

### 2.values()

 遍历map所有的值。
```
for (let value of map9.values()) {
  console.log(value);
}

结果：

1 
2 
3 
```

### 3.entries()

遍历map的所有键值对。

方法1：
```
for (let item of map9.entries()) {
  console.log(item[0], item[1]);
}

结果：

k1 1
k2 2
k3 3
```

方法2：
```
for (let [key, value] of map9.entries()) {
  console.log(key, value);
}

结果不变。
```
 

### 4.forEach()

遍历map的所有键值对。
```
map9.forEach(function(value, key, map) {
  console.log("Key: %s, Value: %s", key, value);
});

结果：

Key: k1, Value: 1
Key: k2, Value: 2
Key: k3, Value: 3
```
forEach有第二个参数，可以用来绑定this。

这样有个好处，map的存储的数据和业务处理对象可以分离，业务处理对象可以尽可能的按职责分割的明确符合SRP原则。


```
const output = {
  log: function(key, value) {
    console.log("Key: %s, Value: %s", key, value);
  }
};

map9.forEach(function(value, key, map) {
  this.log(key, value);
}, output);
```

 

## 和其它结构的互转

### 1.Map To Array

使用扩展运算符三个点（...）可将map内的元素都展开的数组。
```
const map10 = new Map();
map10.set('k1', 1);
map10.set('k2', 2);
map10.set('k3', 3);

结果：

[ [ 'k1', 1 ], [ 'k2', 2 ], [ 'k3', 3 ] ]
 console.log([...map10]);
```

### 2.Array To Map

使用数组构造Map。
```
const map11 = new Map([
  ['name', 'Aissen'],
  ['age', 12]
])
console.log(map11)

结果：

Map { 'name' => 'Aissen', 'age' => 12 }
```

### 3.Map To Object

写一个转换函数，遍历map的所有元素，将元素的键和值作为对象属性名和值写入Object中。


```
function mapToObj(map) {
  let obj = Object.create(null);
  for (let [k,v] of map) {
    obj[k] = v;
  }
  return obj;
}

const map12 = new Map()
  .set('k1', 1)
  .set({pa:1}, 2);
console.log(mapToObj(map12))


 结果：

{ k1: 1, '[object Object]': 2 }
```

### 4.Object To Map

同理，再写一个转换函数便利Object，将属性名和值作为键值对写入Map。


```
function objToMap(obj) {
  let map = new Map();
  for (let k of Object.keys(obj)) {
    map.set(k, obj[k]);
  }
  return map;
}

console.log(objToMap({yes: true, no: false}))


结果：

Map { 'yes' => true, 'no' => false }
```

### 5.Set To Map
```
const set = new Set([
  ['foo', 1],
  ['bar', 2]
]);
const map13 = new Map(set)
console.log(map13)

结果：

Map { 'foo' => 1, 'bar' => 2 }
```

### 6.Map To Set


```
function mapToSet(map) {
  let set = new Set()
  for (let [k,v] of map) {
    set.add([k, v])
  }
  return set;
}

const map14 = new Map()
  .set('k1', 1)
  .set({pa:1}, 2);
console.log(mapToSet(map14))


结果：

Set { [ 'k1', 1 ], [ { pa: 1 }, 2 ] }
```
