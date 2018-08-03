### 1、对数字数组进行由小到大的顺序进行排序。
```
var arr = [22,12,3,43,56,47,4];
arr.sort();
console.log(arr); // [12, 22, 3, 4, 43, 47, 56]
arr.sort(function (m, n) {
 if (m < n) return -1
 else if (m > n) return 1
 else return 0
});
console.log(arr); // [3, 4, 12, 22, 43, 47, 56]
```
### 2、对字符串数组执行不区分大小写的字母表排序。
```
var arr = ['abc', 'Def', 'BoC', 'FED'];
console.log(arr.sort()); // ["BoC", "Def", "FED", "abc"]
console.log(arr.sort(function(s, t){
 var a = s.toLowerCase();
 var b = t.toLowerCase();
 if (a < b) return -1;
 if (a > b) return 1;
 return 0;
})); // ["abc", "BoC", "Def", "FED"]
```
### 3、对包含对象的数组排序，要求根据对象中的年龄进行由大到小的顺序排列
```
var arr = [{'name': '张三', age: 26},{'name': '李四', age: 12},{'name': '王五', age: 37},{'name': '赵六', age: 4}];
var objectArraySort = function (keyName) {
 return function (objectN, objectM) {
  var valueN = objectN[keyName]
  var valueM = objectM[keyName]
  if (valueN < valueM) return 1
  else if (valueN > valueM) return -1
  else return 0
 }
}
arr.sort(objectArraySort('age'))
console.log(arr) // [{'name': '王五', age: 37},{'name': '张三', age: 26},{'name': '李四', age: 12},{'name': '赵六', age: 4}]

```
