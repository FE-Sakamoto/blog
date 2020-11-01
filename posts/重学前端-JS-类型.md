---
title: 重学前端-JS-类型
date: 2020/10/19
---

![JavaScript 类型的那些事](https://tech.youzan.com/content/images/2016/01/aa.png)

[JavaScript类型：关于类型，有哪些你不知道的细节？](https://time.geekbang.org/column/article/78884)

## 0x00

JS语言中规定了7语言类型：

1. Undefined
2. Null
3. Boolean
4. String
5. Number
6. Symbol(ES6)
7. Object



## [Undefined](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)

`Undefined`类型表示未定义且只有一个只`undefined`,任意变量在声明后未赋值的情况下其值均为`undefined`,用以表示该变量未被赋值过.所以开发中一般不会讲变量赋值为`undefined`.



许多编程规范使用`void 0`代替`undefined`, 如用[babel](https://www.babeljs.cn/repl#?browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=GYVwdgxgLglg9mABMOcAUBDRBeRAmASgG8AoRRCBAZzgBsBTAOlrgHNMCBuEgXyA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react%2Ces2015-loose&prettier=false&targets=&version=7.12.1&externalPlugins=)将代码转化为`es6-loose`.

![image-20201016154554104](https://tva1.sinaimg.cn/large/007S8ZIlly1gjr7zx0wc6j30ya069jrw.jpg)



不同于`null` 是一个`关键字`, `undefined`实际上是挂在在`window`的属性,因此`undefined`存在被重新赋值的可能,但是`void 0`是一种运算其结果始终为`undefined`.

![image-20201016154127458](https://tva1.sinaimg.cn/large/007S8ZIlly1gjr7va7sh7j306u054q30.jpg)



但是在现代浏览器中我们已经无法直接对`undefined`进行赋值了,因为现代浏览器中已经将`undefined`设置为`non-configurable`,`non-writable`了.参考[mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)

![image-20201016155944431](https://tva1.sinaimg.cn/large/007S8ZIlly1gjr8ebc2r6j30h504tjry.jpg)



## [Null](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)

`Null`类型也只有一个值,即为`null`,且`null`是JS中的关键字,可以放心使用.且可以用`null`来赋值变量表示变量为空值.



## [Boolean](https://developer.mozilla.org/zh-CN/docs/Glossary/Boolean)

`Boolean`类型只有2个值, `true`与`false`,且均为JS关键字.



## [Number](https://developer.mozilla.org/zh-CN/docs/Glossary/Number)

 {% post_link IEEE 754详解-JS世界中的number揭秘 参考此文章 %}





## [String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)

`String`用于表示文本数据,其理论最大长度为`2^53-1`,注意此处为编码(UTF-16)长度而非文本长度,原因是受`String`的下标所限制,下标最大值即`2^53-1`(即JS中的最大安全整数参见[安全整数](#安全整数)).

![image-20201016161448447](https://tva1.sinaimg.cn/large/007S8ZIlly1gjr8tzkimjj3080047aa6.jpg)

但实际上`2^53-1`约为9PB,无论从业务场景还是计算机硬件内存还是浏览器允许的heap上限角度看都应该不存在不够的情况.



>PS:
>
>为什么JS中字符串是用UTF-16进行编码而非在存储和传输过程中更为流行的`UTF-8`?
>
>个人猜测是由于`UTF-8`的字符编码长度是变长的特性导致无法实现O(1)时间复杂度的下标查询操作.



## [Symbol](https://developer.mozilla.org/zh-CN/docs/Glossary/Symbol)

`Symbol` 是 ES6 中引入的新类型，它是一切非字符串的对象 key 的集合，在 ES6 规范中，整个对象系统被用 `Symbol` 重塑。

我们创建 Symbol 的方式是使用全局的 Symbol 函数(无法用new调用)。例如：

```js
var mySymbol = Symbol("my symbol");
```

一些标准中提到的 Symbol，可以在全局的 Symbol 函数的属性中找到。例如，我们可以使用 Symbol.iterator 来自定义 for…of 在对象上的行为：

```js
var o = new Object

o[Symbol.iterator] = function() {
    var v = 0
    return {
        next: function() {
            return { value: v++, done: v > 10 }
        }
    }        
};

for(var v of o) 
    console.log(v); // 0 1 2 3 ... 9
```



## [Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)

JS中的对象其本质就是**属性的集合**(key-value形式), key为字符串或Symbol, value为值或者访问器.



JS中的几个基本类型都有对应的对象类型:

- Number
- String
- Boolean
- Symbol

我们在一些基本类型上直接调用方法就是基本类型进行装箱操作

```js
'abc'.length // 3
42.toFixed(20) // Uncaught SyntaxError: Invalid or unexpected token .被认作小数点而不是属性访问
(42).toFixed(20) // 42.00000000000000000000
42..toFixed(20) // 42.00000000000000000000
```



## 类型转换

js类型转换示意图

![img](https://static001.geekbang.org/resource/image/71/20/71bafbd2404dc3ffa5ccf5d0ba077720.jpg)



### 装箱转换

所谓装箱转换，正是把基本类型转换为对应的对象，它是类型转换中一种相当重要的种类。

我们定义一个函数，函数里面只有 return this，然后我们调用函数的 call 方法到一个 Symbol 类型的值上，这样就会产生一个 symbolObject。

```js
var symbolObject = (function(){ return this; }).call(Symbol("a"));

console.log(typeof symbolObject); //object
console.log(symbolObject instanceof Symbol); //true
console.log(symbolObject.constructor == Symbol); //true
```



使用内置的 Object 函数，我们可以在 JavaScript 代码中显式调用装箱能力。

```js
Object('a') // String {"a"}
Object(true) // Boolean {true}
Object(1) // Number {1}
Object(Symbol('s')) // Symbol {Symbol(s)}
```

### 

### 拆箱转换

对象到 String 和 Number 的转换都遵循“先拆箱再转换”的规则。通过拆箱转换，把对象变成基本类型，再从基本类型转换为对应的 String 或者 Number。拆箱转换会尝试调用 valueOf 和 toString 来获得拆箱后的基本类型。如果 valueOf 和 toString 都不存在，或者没有返回基本类型，则会产生类型错误 TypeError。

```js
var o = {
    valueOf : () => {console.log("valueOf"); return {}},
    toString : () => {console.log("toString"); return {}}
}

o * 2
// valueOf
// toString
// TypeError
```

在 ES6 之后，还允许对象通过显式指定 @@toPrimitive Symbol 来覆盖原有的行为。

```js
var o = {
    valueOf : () => {console.log("valueOf"); return {}},
    toString : () => {console.log("toString"); return {}}
}

o[Symbol.toPrimitive] = () => {console.log("toPrimitive"); return "hello"}


console.log(o + "")
// toPrimitive
// hello
```







