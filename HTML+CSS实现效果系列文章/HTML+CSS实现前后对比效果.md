**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第6天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

今天我们用HTML和CSS制作一个前后的效果。

实现原理很简单：

- 利用伪元素设置两张背景图片。
- 在 `* :after` 伪元素中利用 `clip-path` 裁剪方式设置可显示区域。
- 最后用 `CSS` 变量 和 `input range` 滑块控件和   `oninput` 事件设置 `CSS` 变量即可。





## 技术支持

引用到的HTML标签有：

- [input range](https://developer.mozilla.org/zh-CN/docs/web/html/element/input/range)

  

引用到的CSS属性有：

-  [clip-path](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path)

- [var()](https://developer.mozilla.org/zh-CN/docs/web/css/using_css_custom_properties)
- [appearance](https://developer.mozilla.org/zh-CN/docs/Web/CSS/appearance)
- [calc()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/calc)



引用到的CSS伪类有：

- `input[type='range']::-webkit-slider-thumb` [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::-webkit-slider-thumb)



## 实现

HTML代码如下：

```html
<div class="contrast" style="--liner: 10">
    <input
        type="range"
        class="mask"
        min="1"
        max="100"
        value="10"
        oninput="this.parentNode.style.setProperty(`--liner`, `${this.value}`)"
    >
</div>
```



CSS代码如下：

```css
html,body{
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
:root{
    --height: 25vw;
}
.contrast{
    width: 40vw;
    height: var(--height);
    position: relative;
    overflow: hidden;
    border-radius: 4px;
    box-shadow: 0 3px 8px rgba(0, 0, 0, .2);
}
.contrast::after,
.contrast::before{
    content: '';
    display: block;
    width: inherit;
    height: inherit;
    background-image: url('https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fp3.itc.cn%2Fimages01%2F20200929%2F188fed59a07d471f811af1c99de64ee2.jpeg&refer=http%3A%2F%2Fp3.itc.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1651840595&t=ef06f1492ede2b907cc9f02ad076b0a5');
    background-size: cover;
    position: absolute;
    top: 0;
    left: 0;

}
.contrast::before{
    background-image: url('https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201806%2F12%2F20180612164256_qylvv.jpeg&refer=http%3A%2F%2Fb-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1651842155&t=2a2980ddb0274e3494e7f3d082b7cb7e');
}
.contrast::after{
    clip-path: inset(0 0 0 calc(var(--liner) * 1%));
}
.mask{
    position: absolute;
    top: 0;
    left: 0;
    width: inherit;
    height: inherit;
    margin: 0;
    -moz-appearance:none; /* Firefox */
    -webkit-appearance:none; /* Safari and Chrome */
    appearance: none;
    outline: none;
    background: transparent;
    z-index: 99;
}
.mask::-webkit-slider-thumb{
    -moz-appearance:none; /* Firefox */
    -webkit-appearance:none; /* Safari and Chrome */
    appearance: none;
    width: 0.729vw;
    height: var(--height);
    background: #000;
    box-shadow: 0 0 5px rgba(0, 0, 0, .5);
    border-radius: 2px;
    cursor: ew-resize;
}
.mask::-webkit-slider-thumb:hover{
    box-shadow: 0 0 8px rgba(0, 0, 0, .5);
}
```



实现效果如下：

![GIF 2022-4-6 22-00-26](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\GIF 2022-4-6 22-00-26.gif)



现在前后对比的效果就完成了。

但是有没有觉得，拖拽 `range` 时有一些明显的卡顿现象，这是因为 `range` 预设值间距不够大的原因，也就是 `min` 和 `max` 的差值不够大（粒度不够细）。

在HTML中稍微修改一下代码：

```html
<div class="contrast" style="--liner: 100">
    <input
        type="range"
        class="mask"
        min="1"
        max="1000"
        value="100"
        oninput="this.parentNode.style.setProperty(`--liner`, `${this.value}`)"
    >
</div>
```



同理 CSS中也要修改一下：

```css
.contrast::after{
    clip-path: inset(0 0 0 calc(var(--liner) / 10 * 1%));
}
```



看一下最终的完成效果：

![GIF 2022-4-6 22-33-06](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\GIF 2022-4-6 22-33-06.gif)

好像也没啥变化，可能是GIF录制软件的原因:scream: ~~

:point_right: 源代码 [CodePen](https://codepen.io/jackbrens/pen/zYpRgXp) 链接。



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃