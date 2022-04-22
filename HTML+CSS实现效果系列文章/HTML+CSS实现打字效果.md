**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第8天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

今天我们用HTML和CSS制作一个打字的效果。

实现原理很简单：

- 使用 `animation` 动画效果控制宽度由 1 变到 20  `ch` 。



## 实现

HTML 代码如下：

```html
<div class="wrapper">
    <div class="code-text">
      This is a code text.
    </div>
</div>
```



CSS 代码如下：

```css
.wrapper{
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.code-text{
  width: 20ch;
  /* alternate 属性定义是否应该轮流反向播放动画；奇数次数（1、3、5 等等）正常播放，而在偶数次数（2、4、6 等等）向后播放  */
  animation: typing 2s steps(20), blink .5s step-end infinite alternate;
  white-space: nowrap;
  overflow: hidden;
  border-right: 3px solid;
  font-family: monospace;
  font-size: 2em;
}

@keyframes typing {
  0% {
      width: 0;
    }
    100%{
        width: 20ch;
    }
}
    
@keyframes blink {
  50% {
    border-color: transparent;
  }
}
```

>ch 是一个相对单位，所谓相对，意思是 ch 会根据当前容器的 `font-size` 变化而变化。
>
>1ch = 1个英文 = 1个数字，但是 1ch ≠ 1个中文。



实现效果如下：

![GIF 2022-4-8 22-38-59](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\GIF 2022-4-8 22-38-59.gif)



可以看到打字结果的效果已经实现了。

加入一点 `CSS` 和 `JS` 代码 实现一下跳动的效果，这里只是让打字效果更美观一些，不喜欢的小伙伴可以不加哦~~

新增 `CSS` 代码如下：

```css
.code-text span{
    display: inline-block;
    animation: jump .4s ease-in-out;
    animation-delay: var(--delay);
}

@keyframes jump{
    0%,100%{
        transform: translateY(0px);
    }
    50%{
        transform: translateY(-10px);
    }
}
```



新增 ` JS` 代码如下：

```javascript
setTimeout(() => {
    const text = document.querySelector('.code-text');
    //反斜号S表示搜寻每一个非空白的字元 ，span里的$&是搜寻到的每一个字母
    //textContent获取元素的文本内容，为每一个字符套上一个 span 标签
    text.innerHTML = text.textContent.replace(/\S/g,'<span>$&</span>');
    const spans = text.querySelectorAll('span');
    spans.forEach((span,index) =>{
        // 为每一个span标签的style中加入 --delay 属性
        span.style.setProperty('--delay',`${index * 0.1}s`);
    })
}, 3000)
```



最终效果如下：

![GIF 2022-4-8 22-37-29](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\GIF 2022-4-8 22-37-29.gif)



:point_right: 演示的源代码在这里 [CodePen](https://codepen.io/jackbrens/pen/OJzZxxP) 链接。



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃

