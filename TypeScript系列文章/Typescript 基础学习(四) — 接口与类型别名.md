**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第18天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## :tada:前言

这是学习 `TypeScript` 的第四章节，我将会把我学习到的知识总结起来供大家参考。



## :pencil2:接口（interface）

> 在面向对象语言中，接口是一个很重要的概念，它类似于一个匹配机制，只有双方都匹配正确才能使用，否则就会报错，它是对行为的抽象，而具体如何行动需要由类去实现。



### :mountain_bicyclist:对象的形状

```typescript
interface Dog {
    name: string;
    color: string;
    age: number;
}

let dogOne: Dog = {
    name: '阿黄',
    color: '黄色',
    age: 4 // ok
}

let dogTwo: Dog = {
    name: '阿黄',
    color: '黄色',
    age: '4' // 报错
}

// 报错；类型 "{ name: string; color: string; }" 中缺少属性 "age"，但类型 "Dog" 中需要该属性。
let dogThree: Dog = { 
    name: '阿黄',
    color: '黄色'
}
```

上面的例子中，我们定义了一个接口 `Dog`，接着定义两个变量 `dogOne` 和 `dogTwo` ，它们的 **形状** 都必须和 `Dog` 相同，例如 `dogTwo` 中 `age` 属性一个是 `4` ，一个是 `'4'` 。类型不匹配就会报错。定义的变量比接口多或者少一些属性都是不允许的。

> 接口定义名字一般是字母大写开头。



### :bicyclist:可选和只读属性

```typescript
interface Dog {
    readonly id: number;
    name: string;
    color: string;
    age?: number;
}

let dogOne: Dog = {
    id: 1,
    name: '阿黄',
    color: '黄色'
}

dogOne.id = 10; // 报错；无法分配到 "id" ，因为它是只读属性。ts(2540)
```

**只读属性** 用于限制只能在对象刚刚创建的时候修改其值，当我们试图修改时，在编译阶段就会报错。

**可选属性** 用于定义的接口里的某个属性是否可选，在创建对象时可要可不要，它可以放在任意属性之间，这一点跟函数类型有所不同。	 [函数类型可以回顾一下往期](https://juejin.cn/post/7087175271559888909)



### :horse_racing:任意类型

有时候我们希望一个接口中除了包含必选和可选属性之外，还允许有其他的任意属性，这时我们可以使用 **索引签名** 的形式来满足上述要求。

```typescript
interface Dog {
    name: string;
    color: string;
    age?: number;
    [custom_name: string]: any
}

let dogOne: Dog = {
    name: '阿黄',
    color: '黄色',
    call: '汪汪汪' // 自定义了一个属性call，它的值是 '汪汪汪'
}
```

需要注意的是，**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**，上述代码中定义的接口  `Dog` 中 `string` 和 `number` 都是 `any` 的子集。



## :pencil2:类型别名（type）

### :snowboarder:基础使用

类型别名用来给一个类型起个新名字。它仅仅是给类型取了一个新的名字，并不是创建了一个新的类型。

```typescript
type Message = string | string[];

let greet = (message: Message) => {
  // ...
}
```



### :swimmer:交叉类型

```typescript
type Dog = { name: string; color: string; } & { age: number; };
const mixin: Dog = {
    name: '阿黄',
    color: '黄色',
    age: 4
}
```

在上述代码中，我们通过交叉类型，使得 `Dog` 同时拥有了 `name`、`color`、`age` 所有属性，这里我们可以试着将合并接口类型理解为求并集。



## :pencil2:interface 和 type 的区别

### :surfer:定义类型范围

>interface只能定义对象类型。
>
>而type声明可以声明任何类型，包括基础类型、联合类型或交叉类型。

- 基本类型

```typescript
type Dog = 'string' | 'number';
```

- 联合类型

```typescript
interface Dog {
    name: string;
}
interface Cat {
    name: string;
}
type animal = Dog | Cat;
```



### :ski:扩展性

接口可以 `extends` 、`implements` ，从而扩展多个接口或类。`type` 没有扩展功能。

- interface extends interface

```typescript
interface Animal {
    name: string;
    age: number;
}
interface Dog extends Animal {
    call: string
}
let dog: Dog = {
    name: '阿黄',
    age: 4,
    call: '汪汪汪'
}
```

- interface extends type

```typescript
type Animal = {
    name: string;
    age: number;
}
interface Dog extends Animal {
    call: string
}
let dog: Dog = {
    name: '阿黄',
    age: 4,
    call: '汪汪汪'
}
```

- type & type

> type 可以使用 **交叉类型**，来使成员合并。

```typescript
type Animal = {
    name: string;
    age: number;
}
type Dog = Animal & { call: string }
let dog: Dog = {
    name: '阿黄',
    age: 4,
    call: '汪汪汪'
}
```

- type & interface

```typescript
interface Animal  {
    name: string;
    age: number;
}
type Dog = Animal & { call: string }
let dog: Dog = {
    name: '阿黄',
    age: 4,
    call: '汪汪汪'
}
```



### :dart:合并声明

> 定义两个 `interface` 会自动合并声明。
>
> 定义两个同名的 `type` 则会出现异常。

```typescript
interface Animal {
    name: string;
    age: number;
}
interface Animal {
    call: string;
}

let dog: Animal = {
    name: '阿黄',
    age: 4,
    call: '汪汪汪'
}


type Animal { // 报错；标识符“Animal”重复。ts(2300)
    name: string;
    age: number;
}
type Animal { // 报错；标识符“Animal”重复。ts(2300)
    call: string;
}
```



## :sparkles:总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃

