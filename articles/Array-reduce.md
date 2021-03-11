# Array 之 reduce
在实现多个数组交集的时候，看到有人使用了`reduce`方法，它吸引了我的注意力，下面来详细了解一下。
```javascript
// 以下代码片段源自GitHub的hugewilliam
/**
 * 实现多个数组交集 
 **/
const getIntersection = (...arrs) => {
    return Array.from(new Set(arrs.reduce((total, arr) => {
        return arr.filter(item => total.includes(item));
    })));
}
```
## Array.prototype.reduce()
> **MDN** 
> reduce() 方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。
> `arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])`


## 用法场景
### polyfill
```javascript
// Production steps of ECMA-262, Edition 5, 15.4.4.21
// Reference: http://es5.github.io/#x15.4.4.21
// https://tc39.github.io/ecma262/#sec-array.prototype.reduce
if (!Array.prototype.reduce) {
  Object.defineProperty(Array.prototype, 'reduce', {
    value: function(callback /*, initialValue*/) {
      if (this === null) {
        throw new TypeError( 'Array.prototype.reduce ' +
          'called on null or undefined' );
      }
      if (typeof callback !== 'function') {
        throw new TypeError( callback +
          ' is not a function');
      }

      // 1. Let O be ? ToObject(this value).
      var o = Object(this);

      // 2. Let len be ? ToLength(? Get(O, "length")).
      var len = o.length >>> 0;

      // Steps 3, 4, 5, 6, 7
      var k = 0;
      var value;

      if (arguments.length >= 2) {
        value = arguments[1];
      } else {
        while (k < len && !(k in o)) {
          k++;
        }

        // 3. If len is 0 and initialValue is not present,
        //    throw a TypeError exception.
        if (k >= len) {
          throw new TypeError( 'Reduce of empty array ' +
            'with no initial value' );
        }
        value = o[k++];
      }

      // 8. Repeat, while k < len
      while (k < len) {
        // a. Let Pk be ! ToString(k).
        // b. Let kPresent be ? HasProperty(O, Pk).
        // c. If kPresent is true, then
        //    i.  Let kValue be ? Get(O, Pk).
        //    ii. Let accumulator be ? Call(
        //          callbackfn, undefined,
        //          « accumulator, kValue, k, O »).
        if (k in o) {
          value = callback(value, o[k], k, o);
        }

        // d. Increase k by 1.
        k++;
      }

      // 9. Return accumulator.
      return value;
    }
  });
}
```


## 其他知识
**`Array.from()`** 从一个类似数组或可迭代对象创建一个新的浅拷贝对象。可以将伪数组or可迭代对象转换成数组。返回值为新的数组。
**`Array.prototype.includes()`** 用于判断某一个数组中是否包含一个指定的值。返回值为Boolean值，包含返回true。语法`arr.includes(valueToFind[, fromIndex])`。
**`Array.prototype.filter()`** 创建一个新数组，包含通所提供函数实现的测试的所有元素。语法： `var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])`。




## 参考文档
- [MDN: reduce](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
- [MDN: filter](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)