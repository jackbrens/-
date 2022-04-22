# 两分钟讲明白call、apply和bind的用法



**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第2天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



**今天用一个很生动的例子介绍  `call`、`apply` 和 `bind` 的用法。**

**使用object的方式建立两个人，jack和rose。**

```javascript
const jack = {
  name: 'jack',

}
const rose = {
  name: 'rose'
}
```



**此时jack有一个手机，加入一个函数，叫做phone，接收一个参数，意思是打给谁。**

```javascript
const jack = {
  name: 'jack',
  phone: function (callname) {
    console.log(`${this.name} 打了一通电话给 ${callname}...`);
  }
}
```



**然后jack打了一通电话给marry，询问她今晚吃饭的事情...**

```javascript
jack.phone('marry') // jack 打了一通电话给 marry...
```



**这时，rose无意中听到了jack的通话，但是rose不想用自己的手机打，于是借用了jack的手机打给marry。**
**这个时候更改一下写法。**
**jack的手机借给了谁？ 借给了rose；打给谁？ 打给marry**

```javascript
jack.phone.call(rose, 'marry') // rose 打了一通电话给 marry...
```



**为了演示 `call` 和 `apply` 的不同，在 `phone` 上新加一个参数**

```javascript
const jack = {
  name: 'jack',
  phone: function (callname, callname2) {
    console.log(`${this.name} 打了一通电话给 ${callname} 和 ${callname2}...`);
  }
}

jack.phone.call(rose, 'marry', 'marry的妈咪')// rose 打了一通电话给 marry 和 marry的妈咪...
```



**换成 `apply`，则把参数修改为数组即可。**

```javascript
jack.phone.apply(rose, ['marry', 'marry的妈咪'])// rose 打了一通电话给 marry 和 marry的妈咪...
```



**如果换成 `bind` 的话，它会返回一个函数，意思是rose借用了jack的电话，不过是不是立即打电话，而是等一下在打。**

```javascript
const roseCallPhone = jack.phone.bind(rose)
roseCallPhone('marry', 'marry的妈咪') // rose 打了一通电话给 marry 和 marry的妈咪...
```

> 问：rose为什么要等一下在打？
> 答：因为她当时在打jack，可能腾不出手来... 



 **完整代码如下：**

```javascript
const jack = {
  name: 'jack',
  phone: function (callname, callname2) {
    console.log(`${this.name} 打了一通电话给 ${callname} 和 ${callname2}...`);
  }
}
const rose = {
  name: 'rose'
}


jack.phone.call(rose, 'marry', 'marry的妈咪')// rose 打了一通电话给 marry 和 marry的妈咪...

jack.phone.apply(rose, ['marry', 'marry的妈咪'])// rose 打了一通电话给 marry 和 marry的妈咪...

const roseCallPhone = jack.phone.bind(rose)
roseCallPhone('marry', 'marry的妈咪')// rose 打了一通电话给 marry 和 marry的妈咪...
```



**总结：**

**call、apply和bind简单来说就是 `把自己的方法借给别人使用` 。**

> jack：不借行不行？
>
> rose：不借我就锤你​ :anger::anger::anger:

...

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~:smiley:

