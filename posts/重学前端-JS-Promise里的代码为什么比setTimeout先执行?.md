[JavaScript执行（一）：Promise里的代码为什么比setTimeout先执行？](https://time.geekbang.org/column/article/82764)



## JS是单线程?如何执行异步任务?

### 何为`单线程`?

就是一次只能执行一个任务如果有多个任务那么就必须排队依次执行. 好处是简单不需要涉及线程 通信 锁等问题. 但是缺点同样明显就是耗时长的任务阻塞整个程序的执行,常见浏览器卡死无响应等情况.

### JS是`单线程`?

JS不是单线程也不是多线程，`ECMAScript`规范中并无与多线程相关的内容。JS是否是单线程是根据浏览器厂商实现的,就目前来看基本浏览器内的JS都是单线程(PS: 不考虑Web Workes).

### JS是如何执行异步任务的?

以`setTimeout`函数为例, 此函数并非`ECMAScript`的API,而是宿主环境`window`,`global`上的属性本质是浏览器或node提供的.

```js
window.setTimeout === setTimeout // true
```



浏览器进程内部有两个常驻线程`JS执行线程`, `事件触发线程`并且维护一个`执行队列`.

`JS执行线程`负责依次执行`执行队列`里的任务,`事件触发线程`负责将任务插入到`执行队列`尾部.

以如下代码为例:

```js
// [task0, task1, task2] 当前队列

console.log('the answer is 42!')
setTimeout(doSomething
, 1000)
```

- `事件触发线程`读取到上述代码将其作为一个任务`task3`插入到`执行队列`尾部.
- 等待前面的task0, task1, task2执行完毕,开始执行task3
- 执行完`console.log`之后调用`setTimeout`浏览器记录下`doSomething`函数并开始计时(计时功能由浏览器实现),在1000毫秒之后`事件触发线程`将`doSomething`函数插入到`执行队列`尾部等待执行.



PS: 从上述流程可知,`setTimeout`中的时间参数并不是任务执行时间,而是任务插入执行队列时间,如果任务插入时队列中还有耗时任务的话`setTimeout`的执行时间会有较大偏差

PS: 同样的异步函数还有诸如`xhr`, `fetch`都是浏览器所实现的.



## 宏任务和微任务?

`事件触发线程`维护的任务队列其实有两个 `宏任务队列(Task Queue)`和`微任务队列(Microtask Queue)`.上面我们将述的仅仅只是`宏任务队列`.上文将的任务队列就是`宏任务队列`.

#### 为什么还有分`宏任务`和`微任务`? 请带着这个疑问往下看.

宏任务包含：

```
script(整体代码)
setTimeout
setInterval
I/O
UI交互事件
postMessage
MessageChannel
setImmediate(Node.js 环境)
```

宏任务包含：

```
Promise.then
Object.observe
MutaionObserver
process.nextTick(Node.js 环境)
```



引入了`微任务`之后的流程图如下



![img](https://pic4.zhimg.com/v2-d437562d6ea5874b3205701819bc1f27_b.jpg)



![img](https://pic2.zhimg.com/v2-e6dd78c74cb671dd9408c2273308a265_b.jpg)

可以看到`微任务`会全部进入到微任务队列,然后在执行完`一个宏任务`后会执行`全部的微任务`.



### 为什么还有分`宏任务`和`微任务`? 

现在可以解答这个问题了,我任务原因如下:

- 分了`宏任务`与`微任务`可以让异步任务有不同的优先级。
- 因为每次循环结束都会伴随着浏览器的渲染,有些数据更新需要在浏览器渲染之前完成。如果没有微任务会进行对此渲染。



## Promise里的代码为什么比setTimeout先执行？

```js
setTimeout(()=>{
  console.log('setTimeout')
}, 0)
Promise.resolve().then(()=>{console.log('promise1')}).then(()=>{console.log('promise2')})
// promise1 promise2 setTimeout
```

1. setTimeout中的回调函数会被插入到`宏任务队列`.

2. Promise.then中的函数会被插入到`微任务队列`.
3. 上述代码全部执行完后,会执行`微任务队列`里的全部任务.Promise中的代码被执行
4. `微任务`全部执行完后才会执行`宏任务`setTimeout中的代码被执行



