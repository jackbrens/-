**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第9天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

今天我们用HTML和CSS制作一个艺术体的效果。

实现原理如下：

- 利用 `h1` 元素、 `h1::before` 和 `h1::after` 两个伪元素实现三层字体的嵌套。
- 使用 `text-stroke`  实现文本描边效果。
- 最后使用 `z-index`  属性设置上述三个元素的堆叠顺序。



## 实现

HTML 代码如下：

```html
<div class="content">
  <h1 data-text="Identity V">
    Identity V
  </h1>
  <h1 data-text="第五人格">
    第五人格
  </h1>
</div>
```



CSS 代码如下：

```css
html,body{
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #1C1C2A;
  font-size: 16px;
  z-index: -9;
}
.content{
  width: 606px;
  height: 360px;
  filter: grayscale(80%);
  background-size: cover;
  z-index: -3;
  position: relative;
  user-select: none;
  -webkit-user-select: none;
}

h1{
  margin: 0;
  font-size: 4rem;
  color: transparent;
  background: linear-gradient(to right, #fff, #BF391B, #0D0D0D);
  -webkit-text-fill-color: transparent;
  background-clip: text;
  -webkit-background-clip: text;
  position: relative;
  letter-spacing: 0.4rem;
  font-style: italic;
}
h1::before,
h1::after{
  content: attr(data-text);
  position: absolute;
  top: 0;
  left: 0;
  color: transparent;
  -webkit-text-stroke: 12px #000;
  z-index: -1;
}
h1::after{
  -webkit-text-stroke: 17px #fff;
  z-index: -2;
}
h1:nth-child(2){
  margin-top: -12px;
  font-size: 1.5rem;
}
.content:hover{
  filter: none;
}
```

>`text-stroke` 属性是向内填充，设置的像素如果很大，就会把字给挡住，具体请参考 [文本描边](https://blog.csdn.net/taotaomin99/article/details/72553280)。
>
>`letter-spacing` 属性可增加或减少字符间的空白（字符间距）具体请参考 [百度](https://baike.baidu.com/item/letter-spacing/7524945?fr=aladdin)。



实现效果如下：

![image-20220409181758815](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409181758815.png)



效果看起来不错，但是字体的效果不够 "艺术"，我们可以在 [Google Fonts](https://fonts.google.com) 中找一下你想要的字体，进不去的小伙伴，懂得都懂哈:smile: ~~

:point_right: [Google Fonts 使用教程](https://www.zhihu.com/question/19578734)

Google Fonts 使用教程：

![image-20220409183105041](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409183105041.png)

找到想要的字体类型，点进去后，点击右边的 `+ select this style`。

之后点击右上角的 `View selected families`  图标， 复制里面的 `link` 和 `font-family` 即可。





新增HTML代码如下：

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Nosifer&display=swap" rel="stylesheet">
```



新增 CSS 代码如下：

```css
h1{
    /* ...之前代码 */ 
    font-family: 'Nosifer', cursive;
}
```



最终实现效果如下：

![image-20220409182659697](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409182659697.png)



:point_right: 演示的源代码在这里 [CodePen](https://codepen.io/jackbrens/pen/YzYLMrr) 链接。



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃