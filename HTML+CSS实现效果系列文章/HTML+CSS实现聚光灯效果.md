**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第4天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

今天我们用HTML和CSS制作一个聚光灯的效果。

实现原理很简单：

- 将两个文字完全重叠，内层是深灰色，外层是有渐变颜色的。
- 在将外层的文字套用圆形遮罩。
- 最后加上 `CSS Animation`。



## 技术支持

引用到的CSS属性有：

- [linear-gradient()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/gradient/linear-gradient)
- [background-image](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-image)

- [background-clip](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)

- [clip-path](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path)



## 实现

为了将HTML结构保持简洁，之后会使用 伪类元素 去制作。

HTML代码如下：

```html
<h1 data-spotlight="我想藏在罐头里!!!">我想藏在罐头里</h1>
```



>**注意:** `attr()` 理论上能用于所有的CSS属性但目前支持的仅有伪元素的 [`content`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/content) 属性，其他的属性和高级特性目前是实验性的
>
>译者注：如果发现浏览器兼容表里attr()的高级用法依旧没有良好的支持的话，本文大部分内容都是纸上谈兵
>
>​																																							引用 [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/attr)

CSS代码如下：

``` css
*{
    margin: 0;
    padding: 0;
}
img{
    border:0;
}
ol, ul ,li{list-style: none;}
html{
    font-size: 16px;
}
body{
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: #222;
}
h1{
    font-size: 4rem;
    color: #333;
    width: 37.5rem;
    letter-spacing: 0.3rem;
    position: relative;
}
h1::after{
    /* attr(attribute_name) */
    content:attr(data-spotlight);
    position: absolute;
    top: 0;
    left: 0;
    color: pink;
    clip-path: ellipse(100px 100px at 0% 50%);
    animation: move 5s infinite;
}
@keyframes move{
    0%{
        clip-path: ellipse(100px 100px at 0% 50%);
    }
    50%{
        clip-path: ellipse(100px 100px at 100% 50%);
    }
    100%{
        clip-path: ellipse(100px 100px at 0% 50%);
    }
}
```



实现效果如下：

![asdasd](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\asdasd.gif)



现在动态的聚光灯效果就完成了。

但是还有问题，不知道细心的小伙伴发现了没有，制作成品的文字是 `彩色` 的，原理就是加上背景图片，然后将文字作为遮罩，最后把`color` 改成透明，所以我们要修改一下代码。

在 `h1:after` 中新增代码 `background-image` 和 `background-clip` ：

```css
h1::after{
    /* 别忘记修改color为透明 */
    color: transparent;
    background-image: linear-gradient(to left,#1a2a6c,#b21f1f,#fdbb2d);
    background-clip: text;
    /* 因为background-clip是预览阶段的css属性，要加上一个前缀版本 */
    -webkit-background-clip: text;
}
```



看一下最终的完成效果：

![abc](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\abc.gif)

## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃