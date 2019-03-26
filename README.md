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

**4、What is the result of this expression? (or multiple ones)**

```      
var val = 'smtg';
console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');
        
A. Value is Something
B. Value is Nothing
C. NaN
D. other
```

答案： D

**解析：**

主要靠运算符优先级，简单来说就是+的优先级大于？

所以原题等价于：

```
console.log('Value is true' ? 'Something' : 'Nothing')
```
答案应该是 ``Something``

---

**5、What is the result of this expression? (or multiple ones)**

```     
var name = 'World!';
(function () {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
})();
        
A. Goodbye Jack
B. Hello Jack
C. Hello undefined
D. Hello World
```

答案： A

**解析：**

在javascript里，声明变量或函数会被提升，就是说，变量提升是JavaScript将声明移至作用域 scope (全局域或者当前函数作用域) 顶部的行为。

但是javascript只提升声明，而不是初始化，如果使用一个在已经使用后才声明和初始化的值，这个值将是``undefined``.

所以这题就相当于：

```
var name = 'World!';
(function () {
    var name;
    if (typeof name === 'undefined') {
        name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
})();
```

---

**6、What is the result of this expression? (or multiple ones)**

```
var END = Math.pow(2, 53);
var START = END - 100;
var count = 0;
for (var i = START; i <= END; i++) {
    count++;
}
console.log(count);
        
A. 0
B. 100
C. 101
D. other
```

答案： D

**解析：**

2的53次方是js能正确计算且不失精度的最大整数，可以参见js权威指南。
js中可以表示的最大整数不是2的53次方，而是1.7976931348623157e+308。

``Math.pow(2, 53) = 9007199254740992。``最大值加1还是``9007199254740992``，所以这个循环会一直下去

``9007199254740992 +1``还是 ``9007199254740992`` ，这就是因为精度问题，如果 ``9007199254740992 +11``或者 ``9007199254740992 +111``的话，值是会发生改变的，只是这时候计算的结果不是正确的值，就是因为精度丢失的问题。

---

**7、What is the result of this expression? (or multiple ones)**

```
var ary = [0,1,2];
ary[10] = 10;
ary.filter(function(x) { return x === undefined;});
        
A. [undefined × 7]
B. [0, 1, 2, 10]
C. []
D. [undefined]
```

答案： C

**解析：**

这里先科普一下``Array.filter()``:

```
var newArray = array.filter(function(currentValue,index,arr), thisValue)
```
``filter()`` 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

``filter``接收两个参数， 一个回调函数，一个是可选的回调函数的this值，默认为undefined

回调函数依次接收三个参数：

* 必须。当前元素的值

* 可选。当期元素的索引值

* 可选。当期元素属于的数组对象

再说稀疏矩阵，当你取数组中某个没有定义的数时：

```
arr[4] // undefined
```
但是当你遍历它时,你会发现,它并没有元素。JavaScript会跳过这些缝隙.

所以答案为： []

---

**8、What is the result of this expression? (or multiple ones)**

```
var two   = 0.2
var one   = 0.1
var eight = 0.8
var six   = 0.6
[two - one == one, eight - six == two]
        
A. [true, true]
B. [false, false]
C. [true, false]
D. other
```

答案： C（笔者认为答案为B）

**解析：**

IEEE 754标准中的浮点数并不能精确地表达小数

根据计算机内二进制的计算标准，二进制并不能表示上述四个小数。所以笔者认为应该都为false，但程序内部是怎么处理的，笔者也不是很清楚，希望有大神解答。

附：部分二进制对应的十进制表。

| 二进制数 | 对应的十进制数 |
| :--- | :----: |
| 0.0000 | 0 |
| 0.0001 | 0.0625|
| 0.0010 | 0.125 |
| 0.0011 | 0.1875|
| 0.0100 | 0.25  |
| 0.0101 | 0.3125|
| 0.0110 | 0.375|
| 0.0111 | 0.4375|
| 0.1000 | 0.5|
| 0.1001 | 0.5625|
| 0.1010 | 0.625|
| 0.1011 | 0.6875|
| 0.1100 | 0.75|
| 0.1101 | 0.8125|
| 0.1110 | 0.875|
| 0.1111 | 0.9375|

---

**9、What is the result of this expression? (or multiple ones)**

```
function showCase(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase(new String('A'));
        
A. Case A
B. Case B
C. Do not know!
D. undefined
```

答案： C

**解析：**

``switch`` 是严格比较, 在``switch``里，比较用的是 ``===``，``String`` 实例和 字符串不一样.

```
var s_prim = 'foo';
var s_obj = new String(s_prim);

console.log(typeof s_prim); // "string"
console.log(typeof s_obj);  // "object"
console.log(s_prim === s_obj); // false
```

所以答案是 ``Do not know!``

---

**10、What is the result of this expression? (or multiple ones)**

```
function showCase2(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase2(String('A'));
        
A. Case A
B. Case B
C. Do not know!
D. undefined
```

答案： A

**解析：**

``String(x)``不创建对象，但返回一个字符串，即``typeof String(1)==="string"``

所以答案为 ``Case A``

---