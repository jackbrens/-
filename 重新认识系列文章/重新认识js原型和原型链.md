**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第13天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



前端初学者也能看懂的原型和原型链

## 前言

**javascript高级程序设计** 这样描述原型：

>每个函数都会创建一个`prototype`属性，这个属性是一个对象，包含应该由特定引用类型的实例共享的属性和方法。实际上，这个对象就是通过调用构造函数创建的对象的原型。使用原型对象的好处是，在它上面定义的属性和方法都可以被对象实例共享。原来在构造函数中直接赋给对象实例的值，可以直接赋值给它们的原型。

JavaScript 的原型和原型链一直是前端初学者的 "绊脚石"，看文档理解起来生涩难懂，所以我采用图文并茂的方式来谈谈我的理解，本文只代表我个人见解，如果有错误的，欢迎大家指出:smile: ~~。



## 原型

> 每个函数都有一个 `prototype` 属性。
>
> 每个对象都存在 `__proto__` 属性，用来描述对象的原型。



先来看一段代码：

```javascript
function Jackson () {}

Jackson.prototype.name = '杰克逊';
Jackson.prototype.age = '18';

let jack = new Jackson();

console.log('jack：', jack);
console.log('name：', jack.name);
```

上面代码打印的结果：

![image-20220413184656382](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220413184656382.png)

为什么会打印这些呢？ 首先我们 `new` 了一个原生构造函数 `Jackson()`，把他赋值给一个名字叫做 `jack` 的实例， `jack` 里没有 **挂载** 任何的东西(函数、属性等)，但是它有个 `Prototype` 对象，里面有 `age` 和 `name` 属性，还有一个 `constructor` 指向的是 `Jackson()`，为什么会这样呢？ 我明明没有添加这个对象啊，它是怎么出现的？ 因为 函数 `Jackson()` 和 `jack` 的 `__proto__` 都指向 `Jackson.prototype` 这个对象，这是 `js` 自带的属性。 

下面画一张图能够直观的看清它们之间的关系：

![image-20220413190827630](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220413190827630.png)

图中的关系可以解释为，每个构造函数都有一个 `__proto__` 指向它的 `原型对象` ，原型对象中有一个 `constructor` 属性指向构造函数，而 `new` 出来的 `实例` 内部有个 `__proto__` 的属性也指向 构造函数的 `原型对象`。



## 原型链

> 当我们访问一个对象的属性，如果在这个对象的内部找不到该属性，那么他就会去他的原型对象上找，这个原型对象又会有自己的原型，一直往上找，直到尽头 `Object.portotype`，这就是原型链的核心。
>
> 所有的原型对象 `prototype` 最终都会指向 `Object.prototype` ，而 `Object.prototype` 的 `__proto__` 指向 `null`。
>
>  `__proto__` 又叫做 `隐式原型` ，**也可以理解为一根看不见的绳子，绑住两个物体**。

画一张图供大家参考：

![image-20220413192104726](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220413192104726.png)



## 例子

> 说了那么多还不如写几道题目巩固一下知识，手底下见章~~

##### 例题一：

先来看看《我的大怨种面试官》出的题：

```javascript
// 看代码说结果

function foo() {}

const bar = new foo()

console.log(bar.__proto__)
console.log(bar.__proto__.__proto__)
console.log(bar.__proto__.__proto__.__proto__)
console.log(bar.__proto__.__proto__.__proto__.__proto__)
console.log(foo.prototype)
console.log(foo.prototype.prototype)
console.log(foo.prototype.prototype.prototype)
```

这就有点意思了，哼，就这？画一张图就清清楚楚了。

![image-20220413193150372](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220413193150372.png)



> `console.log(bar.__proto__)` 
>
> 打印结果为：`{constructor: ƒ}`



> `console.log(bar.__proto__.__proto__)` 
>
> 打印结果为：`{constructor: ƒ, __defineGetter__: ƒ...}`



> `console.log(bar.__proto__.__proto__.__proto__)` 
>
> 打印结果为：`null`



> `console.log(bar.__proto__.__proto__.__proto__.__proto__)`  
>
> 纳尼？ `null.__proto__` 哪还有东西？肯定会打印报错， <span style="color: red">`Cannot read properties of null (reading '__proto__')`</span> 结果不出所料。



> `console.log(foo.prototype)` 
>
> 打印结果为：`foo.prototype` 跟第一个 `console.log` 结果一样。



> `console.log(foo.prototype.prototype)` 
>
> 我擦？foo的原型对象的原型对象？ 有这种东西？肯定报错了。结果查看一下打印结果是 `undefined` ，什么情况？首先我们知道 `foo.prototype` 是一个 **对象**，说明它具有对象的特性(重点，要考的)，于是 我们要思考一个问题 [JavaScript 中什么情况下会返回 undefined 值？](https://blog.csdn.net/yezi153/article/details/121598529) ，哦，原来如此，在 `JavaScript ` 中 **对象属性名不存在时，显示undefined**，这就可以解释为什么会打印 `undefined` 了。

大家可以想象成下面的代码，有一个叫 `fooPrototype` 的对象，**当我们试图获取这个对象里的某一个不存在的属性时，它会返回 `undefined`**。

```javascript
let fooPrototype = {
    
}
console.log(fooPrototype.prototype) // undefined
```



**还有最后一个：**

> `console.log(foo.prototype.prototype.prototype)`

同理，把上述代码改造一下：

```javascript
let fooPrototype = {
    
}
console.log(fooPrototype.prototype.prototype) 
// Cannot read properties of undefined (reading 'prototype')
```

这里会出现报错了。提示 <span style="color: red">`Cannot read properties of undefined (reading 'prototype')`</span>

因为 `fooPrototype.prototype` 返回是一个 `undefined` ，它是一个基本数据类型，你想获取一个基本数据类型里的属性，就会出现报错。





## js 六种继承方式

### 原型链继承

缺点：

- 创建的实例不能传参
- 属性共享





### 经典继承

缺点：

- 复用性不强
- 子类无法访问父类的方法





### 组合式继承

缺点：

- 调用两次 父类 构造函数，有一定的性能浪费





## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃