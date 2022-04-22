**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第19天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## :tada:前言

这是学习 `TypeScript` 的基础进阶的第一章节，我将会把我学习到的知识总结起来供大家参考。



## :sunny: JavaScript中的类

在 `ES6` 之前我们用 `JavaScript` 生成实例对象的传统方法还是通过构造函数的方式。

```js
function Person (name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.user = function () {
  return `我是${this.name}, 我今年${this.age}岁了`;
}

let jack = new Person('jack', 18);
console.log(jack.user()) // 我是jack, 我今年18岁了
```

我们通过创建一个函数，然后使用 `new` 方法实例化，得到一个实例对象。



## :umbrella: ES6中实现类

上面这种写法跟传统的面向对象语言（比如 C++ 和 Java）差异很大，很容易让新学习这门语言的程序员感到困惑。

在 `ES6` 提供了更接近传统语言的写法，引入了 `Class`（类）这个概念，作为对象的模板。通过 `class` 关键字，可以定义类。

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  user () {
    return `我是${this.name}, 我今年${this.age}岁了`;
  }
}

let jack = new Person('jack', 18);
console.log(jack.user()) // 我是jack, 我今年18岁了
```

`constructor` 方法是类的构造函数的默认方法，通过`new`命令生成对象实例时，自动调用该方法。

在上面的例子中我们可以看到之前通过 `function` 定义方法然后要通过原型链来新增方法。事实上，`ES6` 的`class`可以看作只是一个语法糖，它的绝大部分功能，`ES5` 都可以做到，新的 `class` 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。



## :cloud: TypeScript中的类

### :green_book: 类的属性与方法

在 TypeScript 中，我们可以通过 `Class` 关键字来定义一个类：

```typescript
class Person {
  readonly name: string;
  age: number;
  static sex:string = '男'

  constructor (name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  user (): string {
    return `我是${this.name}, 我今年${this.age}岁了，性别${Person.sex}`;
  }
}
let jack: Person = new Person('jack', 18);
console.log(jack.user()); // 我是jack, 我今年18岁了, 性别男
```

上述代码中，需要注意的是 `readonly` 的含义是声明该属性是 **只读** ，顾名思义就是只能读取不能修改。

```typescript
console.log(jack.name = 'alan'); // 报错；无法分配到 "name" ，因为它是只读属性。ts(2540)
```

`static` 的含义是声明该属性为 **静态** ，也就是在外部无法被调用，只能在 `calss` 中调用。

例如 调用 `name` 属性：`this.name` ；而 `sex` 调用方式则是：`Person.sex`。



### :blue_book: 访问修饰符

在`TypeScript`我们可以使用三种访问修饰符，分别是`public`  `private`  `protected` 

- **public** - 意思是 `公众的` 用于修饰该属性或者方法是公有的，可以在任何地方被访问，当我们不设置修饰符的时候默认所有的属性和方法都是`public`的。
- **private** - 意思是 `私有的` 用于修饰该属性或者方法是私有的，不可以在它的类外部调用和访问，包括它的子类。
- **protected** - 意思是 `受保护的` 用于修饰该属性或者方法是受保护的，不可以在它的类外部调用，**但是可以在子类调用和访问**。

```typescript
class Person {
  public name: string;
  private age: number;

  constructor (name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  public user (): string {
    return `我是${this.name}, 我今年${this.age}岁了`;
  }
  private user2 (): string {
    return `我是私有的方法，外部无法调用`;
  }
}
let jack: Person = new Person('jack', 18);
console.log(jack.user()); // 我是jack, 我今年18岁了
console.log(jack.age); // 报错；属性“age”为私有属性，只能在类“Person”中访问。ts(2341)
console.log(jack.user2()); // 报错；属性“user2”为私有属性，只能在类“Person”中访问。ts(2341)
```

上述代码中当我们给一个属性或者函数声明了 private 属性时，**该属性或者函数只能在类内部调用，而不能在实例对象上调用**。

```typescript
class Person {
  public name: string;
  protected age: number;

  constructor (name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  public user (): string {
    return `我是${this.name}, 我今年${this.age}岁了`;
  }
  private user2 (): string {
    return `我是私有的方法，外部无法调用`;
  }
}

class Ming extends Person {
  constructor (name: string, age: number) {
    super(name, age)
  }
}
let ming: Ming = new Ming('jack', 18);
console.log(ming.user()); // 我是jack, 我今年18岁了
console.log(ming.age); // 报错；属性“age”受保护，只能在类“Person”及其子类中访问。ts(2445)
```

- 当我们给属性设定了 `protected` 受保护的时候他虽然不可以在类外部访问，但是却可以通过继承 `extends` 在它的子类访问，如上我们的 `age` 就成功的在子类中使用。



### :orange_book: 类方法重载

在前面的学习中，我们已经介绍了函数重载。对于类的方法来说，它也支持重载。

```typescript
class User {
  getUser(): void;
  getUser(id: number): void;
  getUser(id?: number) {
    if (typeof id === 'number') {
      console.log(`获取id为 ${id} 的User信息`);
    } else {
      console.log(`获取所有的User信息`);
    }
  }
}

let userData = new User();
userData.getUser(1); // 获取id为 1 的User信息
userData.getUser(); // 获取所有的User信息
```

我们通过重载 `User` 里的 `getUser` 方法，就好像把它比作一个 `api` 接口拿来调用，如果不传 `id`，那我们就默认获取所有的用户，如果传入 `id` ，那我们就获取对应的用户信息。这样就不用写两个函数啦。



## :sparkles:总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃

