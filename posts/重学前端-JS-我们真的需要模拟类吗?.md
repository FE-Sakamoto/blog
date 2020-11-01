---
title: 我们真的需要模拟类吗？
date: 2020/10/23
---

![Class Composition in JavaScript ← Alligator.io](https://alligator.io/images/js/class-composition.png)

[JavaScript对象：我们真的需要模拟类吗？](https://time.geekbang.org/column/article/79539)



## JavaScript 的原型

- 如果对象有私有字段[[prototype]], 就是其原型
- 读一个属性,如果对象本身没有,则会继续访问对象的原型,知道原型为空或者找到为止

ES6中提供了以下内置函数可以直接访问操作原型

- Object.create 根据指定的原型创造新对象,原型可以是null;
- Object.getPrototypeOf 获得一个对象的原型
- Object.setPrototypeOf 设置一个对象的原型

```js
var cat = {
    say(){
        console.log("meow~");
    },
    jump(){
        console.log("jump");
    }
}

var tiger = Object.create(cat,  {
    say:{
        writable:true,
        configurable:true,
        enumerable:true,
        value:function(){
            console.log("roar!");
        }
    }
})


var anotherCat = Object.create(cat);

anotherCat.say();

var anotherTiger = Object.create(tiger);

anotherTiger.say();
```



![原型链](https://tva1.sinaimg.cn/large/007S8ZIlly1gfxcrb60coj30eg0gamxv.jpg)

## New

new 运算接受一个构造器和一组调用参数，实际上做了几件事：以构造器的 prototype 属性（注意与私有字段[[prototype]]的区分）为原型，创建新对象；将 this 和调用参数传给构造器，执行；如果构造器返回的是对象，则返回，否则返回第一步创建的对象。

```js
function c1(){
    this.p1 = 1;
    this.p2 = function(){
        console.log(this.p1);
    }
} 
var o1 = new c1;
o1.p2(); // 1



function c2(){
}
c2.prototype.p1 = 1;
c2.prototype.p2 = function(){
    console.log(this.p1);
}

var o2 = new c2;
o2.p2(); // 1
```



## ES6中的类

```js
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  // Getter
  get area() {
    return this.calcArea();
  }
  // Method
  calcArea() {
    return this.height * this.width;
  }
}
```

在现有的类语法中，getter/setter 和 method 是兼容性最好的。

我们通过 get/set 关键字来创建 getter，通过括号和大括号来创建方法，数据型成员最好写在构造器里面。

类的写法实际上也是由原型运行时来承载的，逻辑上 JavaScript 认为每个类是有共同原型的一组对象，类中定义的方法和属性则会被写在原型对象之上。



此外，最重要的是，类提供了继承能力。我们来看一下下面的代码。

```js
class Animal { 
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // call the super class constructor and pass in the name parameter
  }

  speak() {
    console.log(this.name + ' barks.');
  }
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```

