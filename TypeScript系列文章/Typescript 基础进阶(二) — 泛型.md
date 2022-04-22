**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第20天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## :tada:前言

这是学习 `TypeScript` 的基础进阶的第二章节，我将会把我学习到的知识总结起来供大家参考。



## :beers: ​TypeScript泛型

### :pizza: ​泛型介绍

泛型，顾名思义就是 **广泛的** 类型，就是我们在定义一个函数、类或者接口时，不固定他的类型，而是在 **实例化** 的时候才指定类型。

设计泛型的关键目的是在成员之间提供有意义的约束，这些成员可以是：类的实例成员、类的方法、函数参数和函数返回值。

为了更好理解上述的内容，我们举一个例子，让大家理解。

```js
function identity (value) {
  return value;
}

console.log(identity(1)) // 1
```

然后，我们将 `identity` 函数做适当的调整，以支持 `TypeScript` 的 `number` 类型的参数：

```typescript
function identity (value: number) : number {
  return value;
}

console.log(identity(1)) // 1
```

这里我们将 `identity` 定义了 `number` 类型的参数和返回类型，但是一旦固定的具体类型，该函数就不可扩展或通用了，很明显这并不是我们所希望的。但是如果把类型定义为 `any`，那就变成跟 `js` 没区别了。

我们的目标是让 `identity` 函数可以适用于任何特定的类型，为了实现这个目标，我们可以使用泛型来解决这个问题，具体实现方式如下：

```typescript
function identity <T>(value: T) : T {
  return value;
}

console.log(identity<number>(1)) // 1
```

对于刚接触 `TypeScript` 泛型的读者来说，上述代码中里 `<T>` 是什么意思呢，这个 `T` 可以是任意类型，当然，具体的类型是我们自己指定的。就比如上面代码中我们给 `identity` 指定了 `number` 类型，所以他的参数类型和返回类型都变成了 `number` 类型。是不是很酷呢~~

这里有个问题，如果我们不给泛型函数指定类型怎么办，它会报错吗？

```typescript
function identity <T>(value: T) : T {
  return value;
}
console.log(identity(18)); // 18
console.log(identity('jack')); // jack
```

居然没有报错，因为如果我们不给泛型函数指定类型的话，它内部会自动帮我们推导出正确的类型，如果传入的是数字它就会推导出 `number` 类型；如果传入的是字符串它就会推导出 `string` 类型。



### :hamburger: ​泛型约束

有时候，我们希望类型变量对应的类型上存在某些属性。这时，除非我们显式地将特定属性定义为类型变量，否则编译器不会知道它们的存在。

```typescript
function identity<T>(arg: T): T {
  console.log(arg.length); // Error
  return arg;
}
```

报错的原因是编译器不知道 `T` 是否具有 `length` 属性，尤其是在可以将任何类型赋给类型变量 `T` 的情况下。

那我们应该怎么确定 `T` 一定有 `length` 属性呢，这时我们可以约束一下。

```typescript
interface haveLength {
  length: number;
}
function identity<T extends haveLength>(x: T): T{
  let id:T = x;
  console.log(id.length); //不报错了
  return id;
}
```

`T extends Length` 用于告诉编译器，我们支持已经实现 `Length` 接口的任何类型。之后，当我们使用不含有 `length` 属性的对象作为参数调用 `identity` 函数时，TypeScript 会提示相关的错误信息：

```typescript
identity(18); // 报错；类型“number”的参数不能赋给类型“haveLength”的参数。ts(2345)
```



### :fries: ​泛型接口

在之前的章节中我们了解到了接口是用来描述对象的形状，我们来对比一下基本接口和使用泛型接口的区别。

```typescript
// 基本接口
interface Animal {
  name: string;
  call: string;
}
let dog: Animal = {
  name: '阿黄',
  call: '汪汪'
}

// 泛型接口
interface Animal2<T> {
  name: T;
  call: T;
}
let dog2: Animal2<string> = {
  name: '阿黄',
  call: '汪汪'
}
let dog3: Animal2<number> = {
  name: 12,
  call: 13
}
```

如上例我们可以看到，我们制作这个接口 `Animal2` 的时候给了泛型，那么在使用它的时候就可以随意规定一个类型啦，是不是比基本接口更加灵活呢~~



### :meat_on_bone: 泛型类

既然接口可以写成泛型，当然类也是可以写成泛型的形式哦。

```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>(); // 实例化时指定类型为number
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```



### :fried_shrimp: ​泛型参数默认类型

在 TypeScript 2.3 以后，我们可以为泛型中的类型参数指定默认类型。当使用泛型时没有在代码中直接指定类型参数，从实际值参数中也无法推断出类型时，这个默认类型就会起作用。

泛型参数默认类型与普通函数默认值类似，对应的语法很简单，即 `<T=Default Type>`，对应的使用示例如下：

```typescript
interface Person<T = string> {
  name: T;
}

let strA: Person = { 
  name: "jack" 
}
let numB: Person<number> = { 
  name: 18 
}
```



## :sparkles:总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃

