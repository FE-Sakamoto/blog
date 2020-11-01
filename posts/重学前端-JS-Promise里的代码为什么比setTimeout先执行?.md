---
title: Promise里的代码为什么比setTimeout先执行？
date: 2020/10/19
---

![理解单线程的Javascript - 的笃的博客](https://didu.site/images/qiniu/18-3-30-13797484.jpg)



![Pin on Techno Crunch](https://i.pinimg.com/originals/e8/a0/30/e8a03025b969eb9adf69ac24e6cf4993.png)



[JavaScript执行（一）：Promise里的代码为什么比setTimeout先执行？](https://time.geekbang.org/column/article/82764)



## JS是单线程?如何执行异步任务?

首先明确一点:

> 多线程不是异步的必要条件，异步是效果,多线程是其一种实现方案。

JS不是单线程也不是多线程，`Ecmascript`规范中并无与多线程相关的内容。JS中所以的



## 宏观和微观任务

JavaScript引擎等待宿主环境分配宏观任务，在操作系统中，通常等待的行为都是一个事件循环,所以在Node术语中,也会把这个部分称为事件循环，伪代码如下。

```js
while(true) {
  r = wait();
  execute(r);
}
```





