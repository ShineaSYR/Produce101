# 字符串补全
业务上偶尔会出现需要补零的情况，例如月份“3”变成“03”等，后面偶然看到关于字符串补全的API——`padStart()`和`padEnd()`，故而在此处学习记录。
## padStart
从字符串左侧开始用另一个字符串填充（如果需要的话则重复填充）。
```
str.padStart(targetLength [, padString])
```
`targetLength`为补全后字符串的长度，若数值小于原有长度，则返回原字符串。
`padString`为补全的字符串内容，若字符串长度大于空缺长度，则多出部分被截断。

输出示例：
```
'abc'.padStart(10);         // "       abc"
'abc'.padStart(10, "foo");  // "foofoofabc"
'abc'.padStart(6,"123465"); // "123abc"
'abc'.padStart(8, "0");     // "00000abc"
'abc'.padStart(1);          // "abc"
```
**polyfill**

可兼容到IE9+
```javascript
if (!String.prototype.padStart) {
    String.prototype.padStart = function padStart(targetLength,padString) {
        targetLength = targetLength>>0; //floor if number or convert non-number to 0;
        padString = String((typeof padString !== 'undefined' ? padString : ' '));
        if (this.length > targetLength) {
            return String(this);
        }
        else {
            targetLength = targetLength-this.length;
            if (targetLength > padString.length) {
                padString += padString.repeat(targetLength/padString.length); //append to original to ensure we are longer than needed
            }
            return padString.slice(0,targetLength) + String(this);
        }
    };
}
```

## padEnd
从字符串右侧开始用另一个字符串填充（如果需要的话则重复填充）。
```
str.padStart(targetLength [, padString])
```
`targetLength`为补全后字符串的长度，若数值小于原有长度，则返回原字符串。
`padString`为补全的字符串内容，若字符串长度大于空缺长度，则多出部分被截断。

输出示例：
```
'abc'.padEnd(10);          // "abc       "
'abc'.padEnd(10, "foo");   // "abcfoofoof"
'abc'.padEnd(6, "123456"); // "abc123"
'abc'.padEnd(1);           // "abc"
```
**polyfill**

可兼容到IE9+
```javascript
if (!String.prototype.padEnd) {
    String.prototype.padEnd = function padEnd(targetLength,padString) {
        targetLength = targetLength>>0; //floor if number or convert non-number to 0;
        padString = String((typeof padString !== 'undefined' ? padString: ''));
        if (this.length > targetLength) {
            return String(this);
        }
        else {
            targetLength = targetLength-this.length;
            if (targetLength > padString.length) {
                padString += padString.repeat(targetLength/padString.length); //append to original to ensure we are longer than needed
            }
            return String(this) + padString.slice(0,targetLength);
        }
    };
}
```

## 在线访问调试
<a href="../padStart-padEnd.html" target="_blank">点击体验字符补全</a>


