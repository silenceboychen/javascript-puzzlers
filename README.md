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

**11、What is the result of this expression? (or multiple ones)**

```
function isOdd(num) {
    return num % 2 == 1;
}
function isEven(num) {
    return num % 2 == 0;
}
function isSane(num) {
    return isEven(num) || isOdd(num);
}
var values = [7, 4, '13', -9, Infinity];
values.map(isSane);
        
A. [true, true, true, true, true]
B. [true, true, true, true, false]
C. [true, true, true, false, false]
D. [true, true, false, false, false]
```

答案： C

**解析：**

此题等价于

```
7 % 2 => 1
4 % 2 => 0
'13' % 2 => 1
-9 % % 2 => -1
Infinity % 2 => NaN
```
答案 [true, true, true, false, false]

---

**12、What is the result of this expression? (or multiple ones)**

```
parseInt(3, 8)
parseInt(3, 2)
parseInt(3, 0)
        
A. 3, 3, 3
B. 3, 3, NaN
C. 3, NaN, NaN
D. other
```

答案： D

**解析：**

和第一题类似，参考第一题。

---

**13、What is the result of this expression? (or multiple ones)**

```
Array.isArray( Array.prototype )
        
A. true
B. false
C. error
D. other
```

答案： A

**解析：**

鲜为人知的事实：``Array.prototype`` 本身也是一个 ``Array``。

参考： [Array.prototype](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype#Description)

---

**14、What is the result of this expression? (or multiple ones)**

```
var a = [0];
if ([0]) {
  console.log(a == true);
} else {
  console.log("wut");
}
        
A. true
B. false
C. "wut"
D. other
```

答案： B

**解析：**

> 参考： 
> 
> 	- [一张图彻底搞懂JavaScript的==运算](https://zhuanlan.zhihu.com/p/21650547)
>  - [JavaScript-Equality-Table](https://dorey.github.io/JavaScript-Equality-Table/)

* Boolean([0]) === true
* [0] == true
	* true 转换为数字 => 1
	* [0] 转化为数字失败, 转化为字符串 '0', 转化成数字 => 0
	* 0 !== 1

所以答案为false。

---

**15、What is the result of this expression? (or multiple ones)**

```
[]==[]
        
A. true
B. false
C. error
D. other
```

答案： B

**解析：**

``[]`` 是``Object``, 两个 ``Object`` 不相等

答案是 false

---

**16、What is the result of this expression? (or multiple ones)**

```
'5' + 3
'5' - 3
        
A. "53", 2
B. 8, 2
C. error
D. other
```

答案： A

**解析：**

> 知识点：
> 
> * [Arithmetic_Operators#Addition](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Addition)
> * [Arithmetic_Operators#Subtraction](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Subtraction)

也就是说 - 会尽可能的将两个操作数变成数字, 而 + 如果两边不都是数字, 那么就是字符串拼接.

答案是 '53', 2

---

**17、What is the result of this expression? (or multiple ones)**

```
1 + - + + + - + 1
        
A. 2
B. 1
C. error
D. other
```

答案： A

**解析：**

这里应该从后往前计算：

```
g = + 1   => 1
f = - (g) => -1
e = + (f) => -1
d = + (e) => -1
c = + (d) => -1
b = + (c) => -1
a = - (b) => 1
1 + (a)  => 2
```
所以答案 2

---

**18、What is the result of this expression? (or multiple ones)**

```
var ary = Array(3);
ary[0]=2
ary.map(function(elem) { return '1'; });
        
A. [2, 1, 1]
B. ["1", "1", "1"]
C. [2, "1", "1"]
D. other
```

答案： D

**解析：**

稀疏数组. 同第7题.

题目中的数组是一个长度为3, 但是只有一个内容的数组, array 上的操作会跳过这些未初始化的'坑'.

所以答案是 ["1", empty × 2]

---

**19、What is the result of this expression? (or multiple ones)**

```
function sidEffecting(ary) {
  ary[0] = ary[2];
}
function bar(a,b,c) {
  c = 10
  sidEffecting(arguments);
  return a + b + c;
}
bar(1,1,1)
        
A. 3
B. 12
C. error
D. other
```

答案： D

**解析：**

> 知识点:
>
> - [Functions/arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments)

``arguments`` 是一个 ``object``, c 就是 ``arguments[2]``, 所以对于 c 的修改就是对 ``arguments[2]`` 的修改.

所以答案是 21.

当函数参数涉及到 `any rest parameters, any default parameters or any destructured parameters` 的时候, 这个 `arguments` 就不在是一个 `mapped arguments object(映射的参数对象)` 了。参考： [Rest, default, and destructured parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments#Rest_default_and_destructured_parameters)

请看:

```
function sidEffecting(ary) {
  ary[0] = ary[2];
}
function bar(a,b,c=3) {
  c = 10
  sidEffecting(arguments);
  return a + b + c;
}
bar(1,1,1)
```
答案是 12 !!!!

---

**20、What is the result of this expression? (or multiple ones)**

```
var a = 111111111111111110000,
    b = 1111;
a + b;
        
A. 111111111111111111111
B. 111111111111111110000
C. NaN
D. Infinity
```

答案： B

**解析：**

又是一道考查JavaScript数字的题，与第六题考察点相似。由于JavaScript实际上只有一种数字形式IEEE 754标准的64位双精度浮点数，其所能表示的整数范围为 -2^53 ~ 2^53 (包括边界值)。这里的 111111111111111110000 已经超过了2^53 次方，所以会发生精度丢失的情况。

---

**21、What is the result of this expression? (or multiple ones)**

```
var x = [].reverse;
x();
        
A. []
B. undefined
C. error
D. window
```

答案： D

**解析：**

这题考查的是函数调用时的`this`和`Array.prototype.reverse`方法。

首先看`Array.prototype.reverse`方法，首先举几个栗子：

```
console.log(Array.prototype.reverse.call("skyinlayer"));
//skyinlayer
console.log(Array.prototype.reverse.call({}));
//Object {}
console.log(Array.prototype.reverse.call(123));
//123
```
这几个栗子可以得出一个结论，`Array.prototype.reverse`方法的返回值，就是`this`.

`Javascript`中`this`有如下几种情况：

* 全局下this，指向window对象

```
console.log(this);
//输出结果：
//Window {top: Window, window: Window, location: Location, external: Object, chrome: Object…}
```

* 函数调用，this指向全局window对象：

```
function somefun(){
    console.log(this);
}
somefun();
//输出结果：
//Window {top: Window, window: Window, location: Location, external: Object, chrome: Object…}
```

* 方法调用，this指向拥有该方法的对象：

```
var someobj = {};
someobj.fun = function(){
    console.log(this);
};
console.log(someobj.fun());
//输出结果：
//Object {fun: function}
```

* 调用构造函数，构造函数内部的this指向新创建对象：

```
function Con() {
    console.log(this);
}
Con.prototype.somefun = function(){};
console.log(new Con());
//输出结果：
//Con {somefun: function}
```

这里可以看到，使用的是函数调用方式，this指向的是全局对象window，所以选D.

---

**22、What is the result of this expression? (or multiple ones)**

```
Number.MIN_VALUE > 0
        
A. false
B. true
C. error
D. other
```

答案： B

**解析：**

考查的`Number.MIN_VALUE`的概念，[MDN传送门](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MIN_VALUE)，关键的几句话:

- The `Number.MIN_VALUE` property represents the smallest positive numeric value representable in JavaScript. (`Number.MIN_VALUE`表示的是JavaScript中最小的正数)

- The `MIN_VALUE` property is the number closest to 0, not the most negative number, that JavaScript can represent. (`MIN_VALUE`是接近0的数，而不是最小的数)

- `MIN_VALUE` has a value of approximately 5e-324. Values smaller than MIN_VALUE ("underflow values") are converted to 0. (`MIN_VALUE`值约等于5e-324，比起更小的值（大于0），将被转换为0)

所以，这里是true，选B

顺带把`Number`的几个常量拉出来：

* `Number.MAX_VALUE`：最大的正数
* `Number.MIN_VALUE`：最小的正数
* `Number.NaN`：特殊值，用来表示这不是一个数
* `Number.NEGATIVE_INFINITY`：负无穷大
* `Number.POSITIVE_INFINITY`：正无穷大

如果要表示最小的负数和最大的负数，可以使用`-Number.MAX_VALUE`和`-Number.MIN_VALUE`

---

**23、What is the result of this expression? (or multiple ones)**

```
[1 < 2 < 3, 3 < 2 < 1]
        
A. [true, true]
B. [true, false]
C. error
D. other
```

答案： A

**解析：**

运算符的运算顺序和隐式类型转换的题，'<'运算符顺序是从左到右，所以变成了`[true < 3, false < 1]`

接着进行隐式类型转换，'<'操作符的转换规则:

* 如果两个操作值都是数值，则进行数值比较
* 如果两个操作值都是字符串，则比较字符串对应的字符编码值
* 如果只有一个操作值是数值，则将另一个操作值转换为数值，进行数值比较
* 如果一个操作数是对象，则调用valueOf()方法（如果对象没有valueOf()方法则调用toString()方法），得到的结果按照前面的规则执行比较
* 如果一个操作值是布尔值，则将其转换为数值，再进行比较

所以，这里首先通过`Number()`转换为数字然后进行比较，true会转换成1，而false转换成0，就变成了`[1 < 3, 0 < 1]`

所以结果为A.

---

**24、What is the result of this expression? (or multiple ones)**

```
// the most classic wtf
2 == [[[2]]]
        
A. true
B. false
C. undefined
D. other
```

答案： A

**解析：**

这里首先需要对`==`右边的数组进行类型转换，根据以下规则（来自[justjavac的文章《「译」JavaScript 的怪癖 1：隐式类型转换》](https://justjavac.iteye.com/blog/1848749)）：

1. 调用 `valueOf()`。如果结果是原始值（不是一个对象），则将其转换为一个数字。
2. 否则，调用 `toString()` 方法。如果结果是原始值，则将其转换为一个数字。
3. 否则，抛出一个类型错误。

所以右侧被使用`toString()`方法转换为"2"，然后又通过`Number("2")`转换为数字2进行比较，结果就是`true`了，选A.

---

**25、What is the result of this expression? (or multiple ones)**

```
3.toString()
3..toString()
3...toString()
        
A. "3", error, error
B. "3", "3.0", error
C. error, "3", error
D. other
```

答案： C

**解析：**

很多人都踩过`3.toString()`的坑, 虽然`JavaScript`会在调用方法时对原始值进行包装，但是这个点是小数点呢、还是方法调用的点呢，于是乎第一个就是`error`了，因为`JavaScript`解释器会将其认为是小数点。

而第二个则很好说通了，第一个点解释为小数点，变成了`(3.0).toString()`，结果就是"3"了

第三个也是，第一个点为小数点，第二个是方法调用的点，但是后面接的不是一个合法的方法名，于是乎就error了

综上，选C

---

**26、What is the result of this expression? (or multiple ones)**

```
(function(){
  var x = y = 1;
})();
console.log(y);
console.log(x);
        
A. 1, 1
B. error, error
C. 1, error
D. other
```

答案： C

**解析：**

变量提升和隐式定义全局变量的题，也是一个JavaScript经典的坑...

还是那句话，在作用域内，变量定义和函数定义会先行提升，所以里面就变成了:

```
(function(){
    var x;
    y = 1;
    x = 1;
})();
```
这点会问了，为什么不是`var x, y;`，这就是坑的地方...这里只会定义第一个变量`x`，而`y`则会通过不使用`var`的方式直接使用，于是乎就隐式定义了一个全局变量`y`

所以，`y`是全局作用域下，而`x`则是在函数内部，结果就为`1, error`，选C.

---

**27、What is the result of this expression? (or multiple ones)**

```
var a = /123/,
    b = /123/;
a == b
a === b
        
A. true, true
B. true, false
C. false, false
D. other
```

答案： C

**解析：**

首先需要明确JavaScript的正则表达式是什么。JavaScript中的正则表达式依旧是对象，使用`typeof`运算符就能得出结果：

```
console.log(typeof /123/);
//输出结果：
//"object"
```
`==`运算符左右两边都是对象时，会比较他们是否指向同一个对象，可以理解为C语言中两个指针的值是否一样（指向同一片内存），所以两个结果自然都是`false`.

---

**28、What is the result of this expression? (or multiple ones)**

```
var a = [1, 2, 3],
    b = [1, 2, 3],
    c = [1, 2, 4]
a ==  b
a === b
a >   c
a <   c
        
A. false, false, false, true
B. false, false, false, false
C. true, true, false, true
D. other
```

答案： A

**解析：**

和上题类似，JavaScript中`Array`的本质也是对象，所以前两个的结果都是`false`，

而JavaScript中`Array`的`>`运算符和`<`运算符的比较方式类似于字符串比较字典序，会从第一个元素开始进行比较，如果一样比较第二个，还一样就比较第三个，如此类推，所以第三个结果为`false`，第四个为`true`。

综上所述，结果为`false, false, false, true`，选A

---

**29、What is the result of this expression? (or multiple ones)**

```
var a = {}, b = Object.prototype;
[a.prototype === b, Object.getPrototypeOf(a) === b]
        
A. [false, true]
B. [true, true]
C. [false, false]
D. other
```

答案： A

**解析：**

考查的`__proto__`和`prototype`的区别。首先要明确对象和构造函数的关系，对象在创建的时候，其`__proto__`会指向其构造函数的`prototype`属性

`Object`实际上是一个构造函数（`typeof Object`的结果为`function`）,使用字面量创建对象和`new Object`创建对象是一样的，所以`a.__proto__`也就是`Object.prototype`，而`Object.getPrototypeOf(a)`与`a.__proto__`是一样的，所以第二个结果为`true`.

而实例对象是没有`prototype`属性的，只有函数才有，所以`a.prototype`其实是`undefined`，第一个结果为`false`.

综上，选A

---

**30、What is the result of this expression? (or multiple ones)**

```
function f() {}
var a = f.prototype, b = Object.getPrototypeOf(f);
a === b
        
A. true
B. false
C. null
D. other
```

答案： B

**解析：**

还是`__proto__`和`prototype`的区别，两者不是一个东西，所以选B.

`f.prototype` 是使用使用 `new` 创建的 `f` 实例的原型. 而 `Object.getPrototypeOf` 是 `f` 函数的原型.

请看:

```
a === Object.getPrototypeOf(new f()) // true
b === Function.prototype // true
```

---

**31、What is the result of this expression? (or multiple ones)**

```
function foo() { }
var oldName = foo.name;
foo.name = "bar";
[oldName, foo.name]
        
A. error
B. ["", ""]
C. ["foo", "foo"]
D. ["foo", "bar"]
```

答案： C

**解析：**

> [Function.prototype.name
](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/name)

考察了函数的`name`属性，使用函数定义方式时，会给`function`对象本身添加一个`name`属性，保存了函数的名称，很好理解`oldName`为`foo`。`name`属性时只读的，不允许修改，所以`foo.name = "bar";`之后，`foo.name`还是`foo`，所以结果为`["foo", "foo"]`，选C。

---

**32、What is the result of this expression? (or multiple ones)**

```
"1 2 3".replace(/\d/g, parseInt)
        
A. "1 2 3"
B. "0 1 2"
C. "NaN 2 3"
D. "1 NaN 3"
```

答案： D

**解析：**

首先需要确定`replace`会传给`parseInt`哪些参数。举个栗子：

```
"1 2 3".replace(/\d/g, function(){
    console.log(arguments);
});
//输出结果：
//["1", 0, "1 2 3"]
//["2", 2, "1 2 3"]
//["3", 4, "1 2 3"] 
```
一共三个：

1. match：正则表达式被匹配到的子字符串
2. offset：被匹配到的子字符串在原字符串中的位置
3. string：原字符串

这样就很好理解了，又回到之前`parseInt`的问题了，结果就是

```
parseInt("1", 10), 
parseInt("2", 2), 
parseInt("3", 4)
```
所以结果为`"1, NaN, 3"`，选D

---

**33、What is the result of this expression? (or multiple ones)**

```
function f() {}
var parent = Object.getPrototypeOf(f);
f.name // ?
parent.name // ?
typeof eval(f.name) // ?
typeof eval(parent.name) //  ?
        
A. "f", "Empty", "function", "function"
B. "f", undefined, "function", error
C. "f", "Empty", "function", error
D. other
```

答案： C

**解析：**

又是`Function.name`属性的题，和31题一样，`f.name`值为`"f"`，而`eval("f")`则会输出`f`函数，所以结果为`"function"`

接着看`parent`，`parent`实际上就是`f.__proto__`，需要明确的是JavaScript中的函数也是对象，其也有自己的构造函数Function，所以`f.__proto__ === Function.prototype`结果是`true`，而`Function.prototype`就是一个名为`Empty`的`function`.

```
console.log(Function.prototype);
console.log(Function.prototype.name);
//输出结果：
//function Empty() {}
//Empty
```

所以`parent.name`的值为`Empty`

如果想直接在全局作用域下调用`Empty`，显示未定义,因为`Empty`并不在全局作用域下

综上所述，结果为C

---

**34、What is the result of this expression? (or multiple ones)**

```
var lowerCaseOnly =  /^[a-z]+$/;
[lowerCaseOnly.test(null), lowerCaseOnly.test()]
        
A. [true, false]
B. error
C. [true, true]
D. [false, true]
```

答案： C

**解析：**

> [RegExp.prototype.test](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test)

正则表达式的`test`方法会自动将参数转换为字符串，原式就变成了`[lowerCaseOnly.test("null"), lowerCaseOnly.test("undefined")]`，结果都是真，所以选C.

---

**35、What is the result of this expression? (or multiple ones)**

```
[,,,].join(", ")
        
A. ", , , "
B. "undefined, undefined, undefined, undefined"
C. ", , "
D. ""
```

答案： C

**解析：**

因为javascript 在定义数组的时候允许最后一个元素后跟一个`,`, 所以这是个长度为三的稀疏数组(这是长度为三, 并没有 0, 1, 2三个属性哦).

而三个元素，使用`join`方法，只需要添加两次，所以结果为", , "，选C.

---

**36、What is the result of this expression? (or multiple ones)**

```
var a = {class: "Animal", name: 'Fido'};
a.class
        
A. "Animal"
B. Object
C. an error
D. other
```

答案： D

**解析：**

经典坑中的一个，class是关键字。根据浏览器的不同，结果不同：

* chrome的结果： "Animal"
* Firefox的结果："Animal"
* Opera的结果："Animal"
* IE 8以上也是： "Animal"
* IE 8 及以下： 报错

---

**37、What is the result of this expression? (or multiple ones)**

```
var a = new Date("epoch")
        
A. Thu Jan 01 1970 01:00:00 GMT+0100 (CET)
B. current time
C. error
D. other
```

答案： D

**解析：**

> * [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
> * [Date/pares](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/parse)

简单来说, 如果调用 Date 的构造函数传入一个字符串的话需要符合规范, 即满足 `Date.parse` 的条件.

另外需要注意的是 如果格式错误 构造函数返回的仍是一个`Date` 的实例 `Invalid Date`.

答案 `Invalid Date`

---

**38、What is the result of this expression? (or multiple ones)**

```
var a = Function.length,
    b = new Function().length
a === b
        
A. true
B. false
C. error
D. other
```

答案： B

**解析：**

> [Function.length](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/length#Description)

我们知道一个`function()`的 `length` 属性就是函数签名的参数个数, 所以 `b.length == 0`.

`Function` 构造器本身也是个`Function`。他的 `length` 属性值为 1 。该属性 `Writable: false, Enumerable: false, Configurable: true`.

---

** 39、What is the result of this expression? (or multiple ones)**

```
var a = Date(0);
var b = new Date(0);
var c = new Date();
[a === b, b === c, a === c]
        
A. [true, true, true]
B. [false, false, false]
C. [false, true, false]
D. [true, false, false]
```

答案： B

**解析：**

还是关于Date 的题, 需要注意的是：

* 如果不传参数等价于当前时间.
* 如果是函数调用 返回一个字符串.

---

**40、What is the result of this expression? (or multiple ones)**

```
var min = Math.min(), max = Math.max()
min < max
        
A. true
B. false
C. error
D. other
```

答案： B

**解析：**

> * [Math.min](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/min)
> * [Math.max](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

`Math.min` 不传参数返回 `Infinity`, `Math.max` 不传参数返回 `-Infinity`.

---

**41、What is the result of this expression? (or multiple ones)**

```
function captureOne(re, str) {
  var match = re.exec(str);
  return match && match[1];
}
var numRe  = /num=(\d+)/ig,
    wordRe = /word=(\w+)/i,
    a1 = captureOne(numRe,  "num=1"),
    a2 = captureOne(wordRe, "word=1"),
    a3 = captureOne(numRe,  "NUM=2"),
    a4 = captureOne(wordRe,  "WORD=2");
[a1 === a2, a3 === a4]
        
A. [true, true]
B. [false, false]
C. [true, false]
D. [false, true]
```

答案： C

**解析：**

因为第一个正则有一个 `g` 选项 它会 **记忆** 他所匹配的内容, 等匹配后他会从上次匹配的索引继续, 而第二个正则不会.

举个例子:

```
var myRe = /ab*/g;
var str = 'abbcdefabh';
var myArray;
while ((myArray = myRe.exec(str)) !== null) {
  var msg = 'Found ' + myArray[0] + '. ';
  msg += 'Next match starts at ' + myRe.lastIndex;
  console.log(msg);
}
// Found abb. Next match starts at 3
// Found ab. Next match starts at 9
```
所以 a1 = '1'; a2 = '1'; a3 = null; a4 = '2'.

---

**42、What is the result of this expression? (or multiple ones)**

```
var a = new Date("2014-03-19"),
    b = new Date(2014, 03, 19);
[a.getDay() === b.getDay(), a.getMonth() === b.getMonth()]
        
A. [true, true]
B. [true, false]
C. [false, true]
D. [false, false]
```

答案： D

**解析：**

> * [Date](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date)

当Date作为构造函数调用并传入多个参数时，如果数值大于合理范围时（如月份为13或者分钟数为70），相邻的数值会被调整。比如 `new Date(2013, 13, 1)`等于`new Date(2014, 1, 1)`，它们都表示日期`2014-02-01`（注意月份是从0开始的）。其他数值也是类似，`new Date(2013, 2, 1, 0, 70)`等于`new Date(2013, 2, 1, 1, 10)`，都表示时间`2013-03-01T01:10:00`。

此外，`getDay` 返回指定日期对象的星期中的第几天（0～6），所以，你懂的。

---

**43、What is the result of this expression? (or multiple ones)**

```
if ('http://giftwrapped.com/picture.jpg'.match('.gif')) {
  'a gif file'
} else {
  'not a gif file'
}
        
A. 'a gif file'
B. 'not a gif file'
C. error
D. other
```

答案： A

**解析：**

> 如果传入一个非正则表达式对象，则会隐式地使用 new RegExp(obj)
将其转换为正则表达式对象。

所以我们的字符串 `".gif"` 会被转换成正则对象 `/.gif/`，会匹配到 `"/gif"`。

---

**44、What is the result of this expression? (or multiple ones)**

```
function foo(a) {
    var a;
    return a;
}
function bar(a) {
    var a = 'bye';
    return a;
}
[foo('hello'), bar('hello')]
        
A. ["hello", "hello"]
B. ["hello", "bye"]
C. ["bye", "bye"]
D. other
```

答案： B

**解析：**

一个变量在同一作用域中已经声明过，会自动移除 var 声明，但是赋值操作依旧保留，结合前面提到的变量提升机制，你就明白了。

---

**45、输出以下代码执行的结果并解释为什么**

```
var obj = {
    '2': 3,
    '3': 4,
    'length': 2,
    'splice': Array.prototype.splice,
    'push': Array.prototype.push
}
obj.push(1)
obj.push(2)
console.log(obj)
```
答案： 

```
{ '2': 1,
  '3': 2,
  length: 4,
  splice: [Function: splice],
  push: [Function: push] }
```

**解析：**

> push 方法有意具有通用性。该方法和 call() 或 apply() 一起使用时，可应用在类似数组的对象上。push 方法根据 length 属性来决定从哪里开始插入给定的值。如果 length 不能被转成一个数值，则插入的元素索引为 0，包括 length 不存在时。当 length 不存在时，将会创建它。

例子中的 `length` 属性值为 2，意味着 将从属性为 2 的地方开始增加键值对，正巧 `obj` 有一个 '2' 属性了，所以 `obj.push(1)` 会将其属性值覆盖成 1，并且 `length` 属性的属性值变成 3，第二个 `push` 同样，将 '3' 的属性值覆盖成 2，同时 `length` 变成 4

 `length` 变成了 4，所以索引为 0 和 1 会变成 `empty`。

 ---

 ** 46、如何实现一个new?**

```
function _new(fn, ...arg) {
    const obj = Object.create(fn.prototype);
    const ret = fn.apply(obj, arg);
    return ret instanceof Object ? ret : obj;
}
```
