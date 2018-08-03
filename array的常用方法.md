## javascript 中关于array的常用方法

### 第一部分

数组去重，总结了一些数组去重的方法，代码如下：
```
/**
 * 去重操作,有序状态
 * @param target
 * @returns {Array}
 */
function unique(target) {
  let result = [];
  loop: for (let i = 0,n = target.length;i < n; i++) {
    for (let x = i + 1;x < n;x++) {
      if (target[x] === target[i]) {
        continue loop;
      }
    }
    result.push(target[i]);
  }
  return result;
}
 
/**
 * 去重操作，无序状态，效率最高
 * @param target
 * @returns {Array}
 */
function unique1(target) {
  let obj = {};
  for (let i = 0,n = target.length; i < n;i++) {
    obj[target[i]] = true;
  }
  return Object.keys(obj);
}
 
/**
 * ES6写法,有序状态
 * @param target
 * @returns {Array}
 */
function unique2(target) {
  return Array.from(new Set(target));
}
 
function unique3(target) {
  return [...new Set(target)];
}
```

### 第二部分

数组中获取值，包括最大值，最小值，随机值。
```
/**
 * 返回数组中的最小值，用于数字数组
 * @param target
 * @returns {*}
 */
function min(target) {
  return Math.min.apply(0,target);
}
 
/**
 * 返回数组中的最大值，用于数字数组
 * @param target
 * @returns {*}
 */
function max(target) {
  return Math.max.apply(0,target);
}
 
/**
 * 从数组中随机抽选一个元素出来
 * @param target
 * @returns {*}
 */
function random(target) {
  return target[Math.floor(Math.random() * target.length)];
}
```

### 第三部分

对数组本身的操作，包括移除值，重新洗牌，扁平化和过滤不存在的值
```
/**
 * 移除数组中指定位置的元素，返回布尔表示成功与否
 * @param target
 * @param index
 * @returns {boolean}
 */
function removeAt(target,index) {
  return !!target.splice(index,1).length;
}
 
/**
 * 移除数组中第一个匹配传参的那个元素，返回布尔表示成功与否
 * @param target
 * @param item
 * @returns {boolean}
 */
function remove(target,item) {
  const index = target.indexOf(item);
  if (~index) {
    return removeAt(target,index);
  }
  return false;
}
 
/**
 * 对数组进行洗牌
 * @param array
 * @returns {array}
 */
function shuffle(array) {
  let m = array.length, t, i;
  // While there remain elements to shuffle…
  while (m) {
    // Pick a remaining element…
    i = Math.floor(Math.random() * m--);
 
    // And swap it with the current element.
    t = array[m];
    array[m] = array[i];
    array[i] = t;
  }
  return array;
}
 
 
 
/**
 * 对数组进行平坦化处理，返回一个一维的新数组
 * @param target
 * @returns {Array}
 */
function flatten (target) {
  let result = [];
  target.forEach(function(item) {
    if(Array.isArray(item)) {
      result = result.concat(flatten(item));
    } else {
      result.push(item);
    }
  });
  return result;
}
 
 
/**
 * 过滤属性中的null和undefined,但不影响原数组
 * @param target
 * @returns {Array.<T>|*}
 */
function compat(target) {
  return target.filter(function(el) {
    return el != null;
  })
}
```

### 第四部分

根据指定条件对数组进行操作。
```
/**
 * 根据指定条件（如回调或对象的某个属性）进行分组，构成对象返回。
 * @param target
 * @param val
 * @returns {{}}
 */
function groupBy(target,val) {
  var result = {};
  var iterator = isFunction(val) ? val : function(obj) {
    return obj[val];
  };
  target.forEach(function(value,index) {
    var key = iterator(value,index);
    (result[key] || (result[key] = [])).push(value);
  });
  return result;
}
function isFunction(obj){
  return Object.prototype.toString.call(obj) === '[object Function]';
}
 
// 例子
function iterator(value) {
  if (value > 10) {
    return 'a';
  } else if (value > 5) {
    return 'b';
  }
  return 'c';
}
var target = [6,2,3,4,5,65,7,6,8,7,65,4,34,7,8];
console.log(groupBy(target,iterator));
 
 
 
/**
 * 获取对象数组的每个元素的指定属性，组成数组返回
 * @param target
 * @param name
 * @returns {Array}
 */
function pluck(target,name) {
  let result = [],prop;
  target.forEach(function(item) {
    prop = item[name];
    if (prop != null) {
      result.push(prop);
    }
  });
  return result;
}
 
/**
 * 根据指定条件进行排序，通常用于对象数组
 * @param target
 * @param fn
 * @param scope
 * @returns {Array}
 */
function sortBy(target,fn,scope) {
  let array = target.map(function(item,index) {
    return {
      el: item,
      re: fn.call(scope,item,index)
    };
  }).sort(function(left,right) {
    let a = left.re, b = right.re;
    return a < b ? -1 : a > b ? 1 : 0;
  });
  return pluck(array,'el');
}
```

### 第五部分

数组的并集，交集和差集。
```
/**
 * 对两个数组取并集
 * @param target
 * @param array
 * @returns {Array}
 */
function union(target,array) {
  return unique(target.concat(array));
}
 
/**
 * ES6的并集
 * @param target
 * @param array
 * @returns {Array}
 */
function union1(target,array) {
  return Array.from(new Set([...target,...array]));
}
 
/**
 * 对两个数组取交集
 * @param target
 * @param array
 * @returns {Array.<T>|*}
 */
function intersect(target,array) {
  return target.filter(function(n) {
    return ~array.indexOf(n);
  })
}
 
/**
 * ES6 交集
 * @param target
 * @param array
 * @returns {Array}
 */
function intersect1(target,array) {
  array = new Set(array);
  return Array.from(new Set([...target].filter(value => array.has(value))));
}
 
/**
 * 差集
 * @param target
 * @param array
 * @returns {ArrayBuffer|Blob|Array.<T>|string}
 */
function diff(target,array) {
  var result = target.slice();
  for (var i = 0;i < result.length;i++) {
    for (var j = 0; j < array.length;j++) {
      if (result[i] === array[j]) {
        result.splice(i,1);
        i--;
        break;
      }
    }
  }
  return result;
}
 
/**
 * ES6 差集
 * @param target
 * @param array
 * @returns {Array}
 */
function diff1(target,array) {
  array = new Set(array);
  return Array.from(new Set([...target].filter(value => !array.has(value))));
}
```

### 第六部分

数组包含指定目标。
```
/**
 * 判定数组是否包含指定目标
 * @param target
 * @param item
 * @returns {boolean}
 */
function contains(target,item) {
  return target.indexOf(item) > -1;
}
```
最后模拟一下数组中的pop,oush,shift和unshift的实现原理
```
const _slice = Array.prototype.slice;
Array.prototype.pop = function() {
  return this.splice(this.length - 1,1)[0];
};
Array.prototype.push = function() {
  this.splice.apply(this,[this.length,0].concat(_slice.call(arguments)));
  return this.length;
};
Array.prototype.shift = function() {
  return this.splice(0,1)[0];
};
Array.prototype.unshift = function() {
  this.splice.apply(this,
    [0,0].concat(_slice.call(arguments)));
  return this.length;
};
```
