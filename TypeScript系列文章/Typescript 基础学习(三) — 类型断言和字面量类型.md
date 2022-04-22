**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第17天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

这是学习 `TypeScript` 的第三章节，我将会把我学习到的知识总结起来供大家参考。



## 类型断言

类型断言的含义就是当你清楚无比的知道一个实体具体有什么类型的返回值时，就可以使用断言。类型断言好比其他语言里的类型转换，但是不进行特殊的数据检查和解构。它没有运行时的影响，只是在编译阶段起作用。

### 语法

类型断言一般有两种形式：

- 尖括号语法

```typescript
let userName: any = 'jackbrens';
let strLength: number = (<string>userName).length;
```

- as 语法

```typescript
let userName!: string | number;
console.log((userName as string).length); // ok
console.log((userName as number).length); // error
console.log((userName as string).toFixed(2)); // error
console.log((userName as number).toFixed(2)); // ok
```

> 报错原因：（1）number类型 是没有 length 的；（2）string类型 也是没有 toFixed 方法的。



### 非空断言

在上下文中当类型检查器无法断定类型时，一个新的后缀表达式操作符 `!` 可以用于断言操作对象是非 `null` 和非 `undefined` 类型。**具体而言，x! 将从 x 值域中排除 null 和 undefined 。**

- 忽略 undefined 和 null 类型

```typescript
function getUser(parmas: string | undefined | null) {
    //不能将类型“string | null | undefined”分配给类型“string”。
	//不能将类型“undefined”分配给类型“string”。ts(2322)
    const onlyString: string = parmas; // 报错
    const ignoreUndefinedAndNull: string = parmas!; // Ok
}
```



```typescript
interface Entity {
  name: string;
}
function processEntity(e?: Entity) {
  let s = e.name;  // error 对象可能 "未定义"。
}
```

上例中：`e` 为可选参数，所以有可能不传，不传时 `e` 的值为 `undefined` , `undefined`接着调用 `name` 就会抛出异常，因为 `undefined` 中怎么可能有 `name` 属性呢~~

此时我们需要加上一个 `!` 就能避免这个异常。

 ```typescript
interface Entity {
  name: string;
}
function processEntity(e?: Entity) {
  let s = e!.name;  // ok；断言 e 是非空并且访问 name 属性
}
 ```



## 字面量类型

在 TypeScript 中，字面量不仅可以表示值，还可以表示类型，即所谓的字面量类型。

目前，TypeScript 支持 3 种字面量类型：字符串字面量类型、数字字面量类型、布尔字面量类型，对应的字符串字面量、数字字面量、布尔字面量分别拥有与其值一样的字面量类型，具体示例如下：

```typescript
{
  let specifiedStr: '我是字符串' = '我是字符串';
  let specifiedNum: 1 = 1;
  let specifiedBoolean: true = true;
}
```

比如 `'我是字符串'` （这里表示一个字符串字面量类型）类型是 `string` 类型（确切地说是 `string` 类型的子类型），而 `string` 类型不一定是 `'我是字符串'`（这里表示一个字符串字面量类型）类型，如下具体示例：

```typescript
{
  let specifiedStr: '我是字符串' = '我是字符串';
  let str: string = 'any string';
  specifiedStr = str; // ts(2322) 类型 '"string"' 不能赋值给类型 '我是字符串'
  str = specifiedStr; // ok 
}
```

这里为什么 不能把 `str` 赋值给 `specifiedStr` 呢？可以这么解释：比如说我们用“马”比喻 `string` 类型，即“黑马”代指 `'我是字符串'` 类型，“黑马”肯定是“马”，但“马”不一定是“黑马”，它可能还是“白马” “灰马”。这个比喻同样适合于形容数字、布尔等其他字面量和它们父类的关系。这时你要问了，为什么要设定这么细致的字面量类型呢？我直接用 `string` 类型不就好了吗。那当然是有用处的，看一下下面的介绍。



### 字符串字面量类型

```typescript
type rules = 'admin' | 'user';
function getRules (userType: rules) {
  // do something
}
getRules('admin'); // ok
getRules('jack'); // 类型“"jack"”的参数不能赋给类型“rules”的参数。
```

通过使用字面量类型组合的 **联合类型**，我们可以限制函数的参数为指定的字面量类型集合，然后编译器会检查参数是否是指定的字面量类型集合里的成员，这就有点类似于 **字典** 一样，我们只能传入预先定义好的参数，如果传入不属于字典里的参数，就会报错。

因此，相较于使用 string 类型，使用字面量类型（组合的联合类型）可以将函数的参数限定为更具体的类型。这不仅提升了程序的可读性，还保证了函数的参数类型，可谓一举两得。



### 数字字面量类型及布尔字面量类型

数字字面量类型和布尔字面量类型的使用与字符串字面量类型的使用类似，我们可以使用字面量组合的联合类型将函数的参数限定为更具体的类型，比如声明如下所示的一个类型 Config：

```typescript
interface Config {
    rules: 'admin' | 'user';
    avatarSize: 'small' | 'medium' | 'big'; // 头像大小
    isEnable:  true | false; // 头像是否显示
    avatarPadding: 10 | 20 | 30; // 头像的内边距
}
```

在上述代码中，我们指定一个 `interface` 配置，`isEnable` 为布尔字面量类型（布尔字面量只包含 true 和 false，true | false 的组合跟直接使用 boolean 没有区别），`avatarPadding` 为属性为数字字面量类型 `10 | 20 | 30`。



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃

