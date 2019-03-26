# javascript-puzzlers

### Javascript难题 or 你真的了解Javascript吗？

**1、What is the result of this expression? (or multiple ones)**

```       
["1", "2", "3"].map(parseInt)
  
A. ["1", "2", "3"]
B. [1, 2, 3]
C. [0, 1, 2]
D. other
```

答案： D

**解析：**
	
首先, map接受两个参数, 一个回调函数 callback, 一个回调函数的this值

其中回调函数接受三个参数 ``currentValue, index, arrary``;

而题目中, map只传入了回调函数--``parseInt``.

其次, ``parseInt`` 只接受两个两个参数 ``string, radix(基数)``.

> 在没有指定基数，或者基数为 0 的情况下，JavaScript 作如下处理：

> * 如果字符串 ``string`` 以"0x"或者"0X"开头, 则基数是16 (16进制).

> * 如果字符串 ``string`` 以"0"开头, 基数是8（八进制）或者10（十进制），那么具体是哪个基数由实现环境决- 定。ECMAScript 5 规定使用10，但是并不是所有的浏览器都遵循这个规定。因此，永远都要明确给出radix参数的值。

> * 如果字符串 ``string`` 以其它任何值开头，则基数是10 (十进制)。

所以本题即问

```
parseInt('1', 0);
parseInt('2', 1);
parseInt('3', 2);
```
首先后两者参数不合法.

所以答案是 ``[1, NaN, NaN]``

---

**2、What is the result of this expression? (or multiple ones)**

```         
[typeof null, null instanceof Object]
        
A. ["object", false]
B. [null, false]
C. ["object", true]
D. other
```

答案： A

**解析:**

``typeof`` 返回一个表示类型的字符串. ``instanceof`` 运算符用来检测 ``constructor.prototype`` 是否存在于参数 ``object`` 的原型链上.

``typeof`` 的结果请看下表:

```
type         result
Undefined   "undefined"
Null        "object"
Boolean     "boolean"
Number      "number"
String      "string"
Symbol      "symbol"
Host object Implementation-dependent
Function    "function"
Object      "object"
```
所以``typeof null``返回object，但是null并不存在于参数 object 的原型链上，所以返回false。

---

**3、What is the result of this expression? (or multiple ones)**

```
[ [3,2,1].reduce(Math.pow), [].reduce(Math.pow) ]
        
A. an error
B. [9, 0]
C. [9, NaN]
D. [9, undefined]
```

答案： A

**解析：**

> 先说说``Array.reduce``这个方法吧：

```
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```
``reduce``接收两个参数，一个回调函数，一个可选的将用作累积的初始值。

如果提供了 ``initialValue``，则 ``reduce`` 方法会对数组中的每个元素调用一次 ``callbackfn`` 函数（按升序索引顺序）。如果未提供 ``initialValue``，则 ``reduce`` 方法会对从第二个元素开始的每个元素调用 ``callbackfn`` 函数。

回调函数接收四个参数，依次是:

* 通过上一次调用回调函数获得的值。如果向 reduce 方法提供 initialValue，则在首次调用函数时，total 为 initialValue。

* 当前数组元素的值。

* 当前数组元素的数字索引。

* 包含该元素的数组对象。

> 关于``Math.pow(x,y)``

``pow()`` 方法返回 x 的 y 次幂。

``[3, 2, 1].reduce(Math.pow)`` 拆分开来就是：

```
Math.pow(3, 2)    // 9
Math.pow(9, 1)    // 9
```
但是``reduce``在两个情况下会抛出异常,当满足下列任一条件时，将引发 TypeError 异常：

* callbackfn 参数不是函数对象。

* 数组不包含元素，且未提供 initialValue。

所以 ``[].reduce(Math.pow)`` 会抛出异常

答案为：an error

---
