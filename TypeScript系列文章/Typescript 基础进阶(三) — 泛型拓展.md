**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第21天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## :tada:前言

这是学习 `TypeScript` 的基础进阶的第三章节，我将会把我学习到的知识总结起来供大家参考。

之前已经学习了泛型的基本用法，接下来我们拓展一下对于泛型的其他知识点。



## :japanese_ogre: ​泛型条件类型

在 TypeScript 2.8 中引入了条件类型，使得我们可以根据某些条件得到不同的类型，这里所说的条件是类型兼容性约束。尽管以上代码中使用了 `extends` 关键字，也不一定要强制满足继承关系，而是检查是否满足结构兼容性。

条件类型有点类似三元表达式，根据真假从而在两种类型中选择其一：

```typescript
T extends U ? X : Y
```

上述代码的意思是：若 `T` 能够赋值给 `U` ，那么类型是 `X`，否则为 `Y`。



## :japanese_goblin: ​泛型工具类型

### :see_no_evil: ​typeof

在 `TypeScript` 中，`typeof` 操作符可以用来获取一个变量声明或对象的类型。注意跟原生的 `api` 区分一下。

```typescript
interface Animal {
  name: string;
  age: number;
}
const dog: Animal = { name: '阿黄', age: 4 };
type Dog = typeof dog; // -> Dog
const dogtype = typeof dog; // object

function toArray(x: number): Array<number> {
  return [x];
}
type Func = typeof toArray; // -> (x: number) => number[]
```



### :hear_no_evil: ​keyof

`keyof` 操作符是在 TypeScript 2.1 版本引入的，该操作符可以用于获取某种类型的所有键，其返回类型是联合类型。

```typescript
interface Person {
  name: string;
  age: number;
}

type K1 = keyof Person; // "name" | "age"
type K2 = keyof Person[]; // "length" | "toString" | "pop" | "push" | "concat" | "join" 
type K3 = keyof { [x: string]: Person };  // string | number
```

`keyof` 作用就是只能获取类型的所有键吗，这未免也太鸡肋了吧，既然设计出来，那肯定是有用的。`JavaScript` 是一种高度动态的语言。有时在静态类型系统中捕获某些操作的语义可能会很棘手。以一个简单的 `prop` 函数为例：

```js
function getObj(obj, key) {
  return obj[key];
}
```

上述代码中的函数作用就是通过获取 `obj` 和 `key` 两个参数，然后返回对应的 `value` 值。对象上的不同属性，可以具有完全不同的类型，我们甚至不知道 obj 对象长什么样。

那么在 TypeScript 中如何定义上面的 `prop` 函数呢？我们来模拟一下：

```typescript
function getObj(obj: object, key: string) {
  return obj[key];
}
```

上述代码中，虽然我定义了，两个参数的类型，但是编译器还是报错了：

```js
元素隐式具有 "any" 类型，因为类型为 "string" 的表达式不能用于索引类型 "{}"。
在类型 "{}" 上找不到具有类型为 "string" 的参数的索引签名。ts(7053)
```

报错的原因看这里： [显示声明传入的值与这些值一致](https://blog.csdn.net/xlgod/article/details/123182392) 。

那么我们该如何操作呢？因此我们期望用户输入的属性是对象上已存在的属性，那么如何限制属性名的范围呢？这时我们可以利用本文的主角 `keyof` 操作符：

```typescript
function getObj<T extends object, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}
```

在以上代码中，我们使用了 TypeScript 的泛型和泛型约束。**首先定义了 T 类型并使用 `extends` 关键字约束该类型必须是 object 类型的子类型，然后使用 `keyof` 操作符获取 T 类型的所有键，其返回类型是联合类型，最后利用 `extends` 关键字约束 K 类型必须为 `keyof T` 联合类型的子类型。**

举个例子模拟一下：  

```typescript
type Animal = {
  name: string;
  age: number;
}

const dog: Animal = {
  name: '阿黄', 
  age: 4
}

function getObj<T extends object, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

const dogName = getObj(dog, "name"); // const name: string
const dogAge = getObj(dog, "age"); // const age: number
```

如果访问 `dog` 上不存在的属性时，会发什么呢？

```
const dogCall = getObj(dog, "call");
```

编译器会提示报错：

```js
类型“"call"”的参数不能赋给类型“keyof Animal”的参数。ts(2345)
```

这就阻止我们尝试读取不存在的属性。



### :speak_no_evil: ​in

`in` 用来遍历枚举类型：

```typescript
type Keys = "a" | "b" | "c"

type Obj =  {
  [p in Keys]: any
} // -> { a: any, b: any, c: any }
```



### :feet: infer

在条件类型语句中，可以用 `infer` 声明一个类型变量并且对它进行使用。

```typescript
type ReturnType<T> = T extends (
  ...args: any[]
) => infer R ? R : any;
```

以上代码中 `infer R` 就是声明一个变量来承载传入函数签名的返回值类型，简单说就是用它取到函数返回值的类型方便之后使用。



## :sparkles:总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃

