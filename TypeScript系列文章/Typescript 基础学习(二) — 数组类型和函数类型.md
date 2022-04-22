**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第16天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

这是学习 `TypeScript` 的第二章节，我将会把我学习到的知识总结起来供大家参考。



## TypeScript 的特殊类型

### Array 类型

在 `ts` 中对数组的定义有两种方式：

```typescript
let array:string[] = ['jack', 'alan'];
let array2:Array<string> = ['jack', 'alan']; // 泛型语法，后期会细讲
```



定义联合类型的数组：

> 定义一个变量array，赋值的类型为数组，参数的类型必须为number或者string 。

```typescript
let array:(number | string)[];
array = ['jack', 1, 'alan', 2];
```



定义指定对象成员的数组：

```typescript
interface user {
    name: string,
    password: string | number // 密码可以是string或者number
}
let array:user[] = [{ name: 'jack1', password: 'jack123456' }];
let array2:user[] = [{ name: 'jack2', password: 123456 }];
```



### 函数类型

#### 函数声明

> 函数 `add` 的 参数 `a` 和参数 `b` 都是 `number` 类型，那函数的返回类型自然也是 `number` 类型了。

```typescript
function add (a: number, b: number): number {
    return a + b;
}
```



#### 函数表达式

>TS类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。
>
>这和ES6中的箭头函数是不一样的。

```typescript
let add:(a: number, b: number) => number = function(a: number, b: number): number {
  return a + b;
}
```



#### 可选参数

> 注意：可选参数必须位于必选参数后面。

```typescript
function sendUser(name: string, passwrod: string | number, email?: string) {
    if (!email) {
        return `${name}, ${passwrod}`;
    }
    return `${name}, ${passwrod}, ${email}`;
}
```



#### 参数默认值

```typescript
function sendUser(name: string, nickName: string = 'joke') {
    return `${name}, ${nickName}`;
}
```



#### 剩余参数

> 用 ES6 中的拓展运算符来接收剩余参数 1,2,3 ；并把他们放到一个数组中，类型定义为 any。

```typescript
function push(array: any[], ...items: any[]) {
    items.forEach(item => {
        array.push(item);
    });
}
let arr = [];
push(arr, 1, 2, 3);
```



#### 函数重载

重载的定义是 **会根据不同的参数而返回不同的类型的调用结果**，可能听得有些抽象，我们用一个例子来说明一下。

先定义一个 **联合类型** `string | number` ，取个别名：

```typescript
type union = string | number;
```

运用到 `add` 函数中：

```javascript
function add(a:number,b:number):number;
function add(a: string, b: string): string;
function add(a: string, b: number): string;
function add(a: number, b: string): string;
function add(a:union, b:union) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}
const result = add('jack', 'alan');
result.split('');
```

> 重载的意思可以理解为当一个函数接收不同数量或类型的参数时，对应的函数内部会有不同的逻辑处理方式。



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃

