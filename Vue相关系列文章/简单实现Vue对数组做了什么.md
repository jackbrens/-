**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第14天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

一般提问到Vue源码时，总有人问道 Vue 是如何检测数组变化？有人可以马上回答：Vue对数组进行了重写，但是在问道在Vue中是如何重写的呢？少数人却回答不上来。相信看完本篇文章你会有所收获，那么我们接下来简单模拟一下对于数组的监听吧~~



## 实现

开始之前，先思考两个函数分别做什么事情。

```javascript
// 执行该方法，重写数组
const reactive = (data) => {
    
}

// 监听数组变化
const observe = (method, ...args) => {
    
}

// 需要重写的数组有哪些
const methodsToPatch = []
```



重写之前，先把数组的原型对象 `Array.prototype` 保存起来并拷贝一份。

```javascript
const arrPrototype = Array.prototype;
const copyArrPrototype = Object.create(arrPrototype);
```



之后在 执行函数 `reactive` 中判断传进来的 `data` 是数组时，将 `data` 的 `__proto__` 指向拷贝的原型对象。

```javascript
const reactive = (data) => {
    if (!Array.isArray(data)) {
        throw new Error('`${data} is not Array!');
    }
    data.__proto__ = copyArrPrototype;
}
```



简单打印一下数组监听时的变化。

```javascript
const observe = (method, ...args) => {
    console.log(`method = ${method}, args= ${args.join(' ')}`);
}
```



执行重写数组的逻辑操作，在执行原数组的操作后，对数据进行劫持。

```javascript
// 列出需要重写的数组方法名
const methodsToPatch = [
  'push',
  'pop',
  'shift',
  'unshift',
  'splice',
  'sort',
  'reverse'
]

methodsToPatch.forEach(method => {
    
    // 在拷贝的原型对象上重写7个数组的方法，代码的核心
    copyArrPrototype[method] = function () {
        
        // 调用原始数组的方法，并传入参数arguments
        arrPrototype[method].call(this, ...arguments);
        
        // 监听数组变化
        observe(method, ...arguments);
    }
})
```

重写数组之前：

![image-20220414224057606](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220414224057606.png)

重写数组之后：

![image-20220414224431400](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220414224431400.png)

可以明显的看到多了 7 个重写的数组方法。



最后测试一下代码：

```javascript
const array = [1, 2, 3, 4]
reactive(array);
array.push(5,6);
array.splice(0, 2);
```

打印结果如下图：

![image-20220414222920768](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220414222920768.png)



:point_right: 演示的源代码在这里 [CodePen](https://codepen.io/jackbrens/pen/JjMmJQm) 链接。



> 问：Vue 是如何检测数组变化？
>
> 答：对数组的7个方法进行重写。
>
> 
>
> 追问：怎么重写的呢？简单说一下
>
> 答：首先把数组原型对象保存并且拷贝一份，然后将执行一个函数，把数组的 `__proto__` (隐式原型)指向拷贝的对象，之后再执行重写数组的逻辑操作，在执行原数组的操作后，对数据进行劫持。



以上只是简单的回答了问题，但是在Vue源码中还通过执行 `发布函数` 将当前数组的变更通知给其订阅者，这样当使用重写后方法改变数组后，数组订阅者会将这边变化更新到页面中，我们这里只是简单的实现了一下，并不涉及到更深层次。



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃

