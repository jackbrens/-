**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第15天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

就目前的前端发展趋势，`TypeScript` 是越来越主流了，所以也导致更多的人投入到对 `TypeScript` 的学习中，学习之前，哎呀，怎么那么麻烦，学了 `JavaScript` 还不够，又来一个语言；学习之后，`Typescript` 真香，你还不会啊，还不快学习起来，之前在网上看到有人调侃道：如果下一家公司的项目不是用 `TS` 写的，不好意思，不想去。只是开个玩笑哈，大家别当真:smile: ~~



## TypeScript 安装

`TypeScript ` 不是编译器与生育来的哟，是需要 `coder` 手动安装的，所有在安装之前先查看一下 `node` 是否安装，因为 `TypeScript ` 是需要使用 `npm` 安装的。

![image-20220415185902479](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220415185902479.png)



**全局安装TypeScript**

```
npm install -g typescript
```

**安装完成后我们可以使用 tsc 命令来执行 TypeScript 的相关代码，以下是查看版本号：**

![image-20220415190110210](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220415190110210.png)



**编译TypeScript**

在项目开发中，ts 文件最终都会编译成 js 文件。

项目中新建 `hello.ts`  ，在命令行中执行 `tsc .\hello.ts` 就会生成对应的 `hello.js`。



![image-20220415190851419](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220415190851419.png)



每次修改完代码都手动执行一遍命名也太麻烦了，我们可以生成一个配置文件来帮我们自动编译 `ts` 代码，在命令行中执行 `tsc --init` 会生成一个 `tsconfig.json` 配置文件。

![image-20220415191421605](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220415191421605.png)

之后在终端中选择运行任务，选择监视 `tsconfig.json` 配置文件。

![image-20220415191706547](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220415191706547.png)

![image-20220415191745925](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220415191745925.png)

这样，以后修改代码就会自动帮忙保存啦。



## TypeScirpt类型定义

我们已经知道在 `JavaScript` 中的基本数据类型有： `boolean`(布尔类型)、`string`(字符串类型)、`number`(数字类型)、`null` 、`undefined`。

我们来对比一下在 `TypeScirpt` 中是如何规范的定义这些类型的。



### boolean(布尔类型)

在项目中我们经常遇到这种情况：判断条件是否成立，或者对错。结果无非两种情况 `true` 或 `false`。

```js
let disable:boolean = false;
disable = 'string'; // 不能将类型“string”分配给类型“boolean”.
```

当我们把一个字符串赋值给 `disable` 就会报错。



### string(字符串类型)

由 `单引号 ''` 或者 `双引号 ""` 括起来的一串字符就是字符串。

```js
let name:string = 'jackbrens';
```



### number(数字类型)

```js
let a:number = 99;
```



### null 和 undefined

当定义了一个变量，但是没有赋值时，这个变量会有一个默认值 `undefined`。

```js
let a:undefined;
let b:null;
console.log(a); //undefined
console.log(b); //undefined
```

此时 `a` 和 `b` 打印的都是 `undefined`，但在打印 `b` 时编译器会报错： `在赋值前使用了变量“b”。` 



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃