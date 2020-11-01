---
title: 你知道全部的对象分类吗？
date: 2020/10/23
---

![What are the differences between Objects, Arrays, Maps and Sets in  JavaScript? | by Mosh Hamedani | JavaScript In Plain English | Medium](https://miro.medium.com/max/2560/1*fkGZ1RKN6jvzwN2XHmAnfg.jpeg)

[JavaScript对象：你知道全部的对象分类吗？](https://time.geekbang.org/column/article/80011)

## JavaScript 中的对象分类

我们可以吧对象分成几类

- 宿主对象(host Objects): 有JavaScript宿主环境提供的对象,他们的行为完全由宿主环境决定.
- 内置对象(Built-in Objects):由JavaScript语言提供的对象.
  - 固有对象(Intrinsic Objects): 由标准规定, 随着JavaScript运行时创建而自动创建的对象实例.
  - 原生对象(Native Objects): 可以由用户通过Array、RegExp等内置构造器或者特殊语法创建的对象).
  - 普通对象(Ordinary Objects): 由{}语法、Object构造器或者class关键字定义类创建的对象,它能够被原型继承.



### 宿主对象

宿主对象可以理解为非`ECMAScript`标准对象,宿主对象又可以分为`固有宿主对象`及`用户可创建的宿主对象`两种.

如在浏览器环境下`固有宿主对象`有`window`、`document`、`navigator`等等.

`用户可创建的宿主对象`有`new Image()`和`document.createElement()`创建出来的对象等等.



### 内置对象

#### 固有对象

`Array`、`Array.prototype`等等

```js
const set = new Set();
const objects = [
    eval,
    isFinite,
    isNaN,
    parseFloat,
    parseInt,
    decodeURI,
    decodeURIComponent,
    encodeURI,
    encodeURIComponent,
    Array,
    Date,
    RegExp,
    Promise,
    Proxy,
    Map,
    WeakMap,
    Set,
    WeakSet,
    Function,
    Boolean,
    String,
    Number,
    Symbol,
    Object,
    Error,
    EvalError,
    RangeError,
    ReferenceError,
    SyntaxError,
    TypeError,
    URIError,
    ArrayBuffer,
    SharedArrayBuffer,
    DataView,
    Float32Array,
    Float64Array,
    Int8Array,
    Int16Array,
    Int32Array,
    Uint8Array,
    Uint16Array,
    Uint32Array,
    Uint8ClampedArray,
    Atomics,
    JSON,
    Math,
    Reflect];
objects.forEach(o => set.add(o));

for(let i = 0; i < objects.length; i++) {
    const o = objects[i]
    for(let p of Object.getOwnPropertyNames(o)) {
        let d = Object.getOwnPropertyDescriptor(o, p)
        if((d.value !== null && typeof d.value === "object") || (typeof d.value === "function")){
          if (!set.has(d.value)){
            set.add(d.value)
            objects.push(d.value)
					}
        }
        if (d.get){
          if (!set.has(d.get)) {
            set.add(d.get)
            objects.push(d.get)
          }
        }
        if (d.set) {
          if (!set.has(d.set)) {
            set.add(d.set)
            objects.push(d.set)
          }
        }
    }
}
```

用该方法可以获取所有固有对象



#### 原生对象

我们把 JavaScript 中，能够通过语言本身的构造器创建的对象称作原生对象。在 JavaScript 标准中，提供了 30 多个构造器。按照我的理解，按照不同应用场景，我把原生对象分成了以下几个种类。

![img](https://static001.geekbang.org/resource/image/6c/d0/6cb1df319bbc7c7f948acfdb9ffd99d0.png)



#### 普通对象

- JS中的函数使用对象模拟的(有[[call]]私有字段的对象)

- JS中的构造器其实就是函数(有[[construct]]私有字段的对象)

  



