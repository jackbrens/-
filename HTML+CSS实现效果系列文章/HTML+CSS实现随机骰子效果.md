**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第7天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

今天我们用HTML和CSS制作一个随机骰子的效果。

实现原理很简单：

- 利用元素的 `background-image`设置一张​ :game_die: 图片。
- 在使用 `animation` 动画效果循环 `background-position` 的 x轴。



## 技术支持

引用到的CSS属性有：

- [animation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation)
- [+ (相邻兄弟选择器)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Adjacent_sibling_combinator)

- [backdrop-filter](https://developer.mozilla.org/zh-CN/docs/Web/CSS/backdrop-filter)



## 实现

HTML 代码如下：

```html
<input type="checkbox" id="dices">
<label for="dices">
    <div class="dice"></div>
</label>
```

> 需要注意的是 `input ` 元素必须要在 `label` 元素之前。
>
> 因为 `+` 相邻兄弟选择器的原理是选择紧接在另一个元素**之后**的元素，而且二者有相同的父元素。



CSS 代码如下：

```css
html,body{
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
.dice{
    width: 22.875rem;
    height: 22.875rem;
    background-image: url('https://assets.codepen.io/2002878/random-dice.png');
    background-position: 0 100%;
    /* steps(步数) 让动画以步数的方式过渡  */
    animation: random .3s steps(5) infinite;
}

@keyframes random{
    0%{
        background-position: 0 100%;
    }
    100%{
        background-position: 100% 100%;
    }
}

#dices{
    display: none;
}

#dices:checked + label .dice{
    animation-play-state: paused;
}
```



实现效果如下：

![GIF 2022-4-7 21-23-19](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\GIF 2022-4-7 21-23-19.gif)



可以看到随机结果的效果已经实现了。

感觉这样的效果看起来有些生硬，那我们在​ :game_die: 加一些上修饰吧~~

新增代码如下：

```css
.dice::before{
    content: '我是谁?';
    background-color: rgba(255, 255, 255, 0.7);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    width: inherit;
    height: inherit;
    font-size: 3rem;
    border-radius: 3.75rem;
    display: flex;
    justify-content: center;
    align-items: center;
}

#dices:checked + label .dice::before{
    display: none;
}
```

> 或许会有人疑问为什么不用 `filter: blur()` 实现毛玻璃效果？
>
> 是因为 `filter` 会把内容和背景都进行模糊，而 `backdrop-filter` 则是只会模糊背景。



看一下最终的完成效果：

![GIF 2022-4-7 21-34-28](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\GIF 2022-4-7 21-34-28.gif)



想要实现的效果就是在原来的基础上加入一个毛玻璃效果，并且在 :game_die: 结束后将毛玻璃效果去掉。

:point_right: 演示的源代码在这里 [CodePen](https://codepen.io/jackbrens/pen/LYedBKx) 链接。



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃