![](https://www.atnyla.com/library/images-tutorials/class-and-object-in-java-3.PNG)

[JavaScript对象：面向对象还是基于对象？](https://time.geekbang.org/column/article/79319)



JS对象可以理解为`key-value`的集合,key可以是`String`或`Symbol`,value又可以分为`数据属性`和`访问器属性`

数据属性:

- value：就是属性的值。

- writable：决定属性能否被赋值。

- enumerable：决定 for in 能否枚举该属性。

- configurable：决定该属性能否被删除或者改变特征值。

  ```js
  var o = { a: 1 }; 
  Object.defineProperty(o, "b", {value: 2, writable: false, enumerable: false, configurable: true}); //a和b都是数据属性，但特征值变化了
  Object.getOwnPropertyDescriptor(o,"a"); // {value: 1, writable: true, enumerable: true, configurable: true} 
  Object.getOwnPropertyDescriptor(o,"b"); // {value: 2, writable: false, enumerable: false, configurable: true} o.b = 3; console.log(o.b); // 2
  ```

  

访问器属性:

- getter：函数或 undefined，在取属性值时被调用。

- setter：函数或 undefined，在设置属性值时被调用。

- enumerable：决定 for in 能否枚举该属性。

- configurable：决定该属性能否被删除或者改变特征值。

  ```js
  var o = { get a() { return 1 } }; 
  console.log(o.a); // 1
  Object.getOwnPropertyDescriptor(o, 'a') // {set: undefined, enumerable: true, configurable: true, get: ƒ}
  Object.defineProperty(o, 'b', {get: ()=>42, set: undefined, enumerable: true, configurable: true})
  o.b // 42
  ```





