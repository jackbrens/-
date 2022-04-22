**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第7天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

今天我们用HTML和CSS制作一个3D骰子的效果。

实现原理如下：

- 利用 `flex` 布局设置 :game_die: 的六个点数。
- 之后在利用 `transform` 将六个点数设置到不同的位置，最终形成一个正方体。



## 实现

HTML 代码如下：

```html
<div class="contanter">
    <div class="dice">
        <div class="dice-font">
            <li></li>
        </div>
        <div class="dice-top">
            <li></li>
            <li></li>
        </div>
        <div class="dice-left">
            <li></li>
            <li></li>
            <li></li>
        </div>
        <div class="dice-right">
            <li></li>
            <li></li>
            <li></li>
            <li></li>
        </div>
        <div class="dice-end">
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
        </div>
        <div class="dice-bottom">
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
        </div>
    </div>
</div>
```



CSS 代码如下：

```css
ol, ul ,li{list-style: none;}
html,body{
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
:root{
    --boxWidth: 200px;
    --width: 25%;
}
.contanter{
    perspective: 800px;
}
.dice{
    transform-style: preserve-3d;
    position: relative;
    animation: run 5s linear infinite;
    /*width: var(--boxWidth);
    height: var(--boxWidth);*/
}
.dice>div{
    box-sizing: border-box;
    width: var(--boxWidth);
    height: var(--boxWidth);
    background-color: #eee;
    position: absolute;
}
.dice li{
    width: var(--width);
    height: var(--width);
    background-color: #000;
    border-radius: 50%;
}
```



页面初始化如图所示：

![image-20220409223710789](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409223710789.png)

## 点数一

```css
.dice-font{
display: flex;
justify-content: center;
align-items: center;
}
.dice-font>li{
width: 40%;
height: 40%;
background-color: red;
}
```

![image-20220409224543453](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409224543453.png)



## 点数二

```css
.dice-top{
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
}
```

![image-20220409224727277](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409224727277.png)



## 点数三

```css
.dice-left{
  padding: 4px;
  display: flex;
  justify-content: space-around;
}
.dice-left>li:first-of-type{
  align-self: flex-end;
}
.dice-left>li:nth-of-type(2){
  align-self: center;
}
```

![image-20220409224826770](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409224826770.png)

## 点数四

```css
.dice-right{
  display: flex;
  align-items: center;
  flex-wrap: wrap;
}
.dice-right>li{
  margin: 25px;
  background-color: red;
}
```

![image-20220409224920517](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409224920517.png)



## 点数五

```css
.dice-end{
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}
.dice-end>li{
  margin: 0 25px;
}
.dice-end>li:nth-of-type(3){
  margin: 0 75px;
}
```

![image-20220409225005526](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409225005526.png)



## 点数六

```css
.dice-bottom{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
}
.dice-bottom>li{
  margin: 0 18px;
}
```

![image-20220409225044128](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220409225044128.png)



## 3D效果实现

利用 `transform`  给每个点数设置固定的位置。

新增代码如下：

```css
.dice-top{
    /* ...省略代码 */
    transform: translateZ(-100px) translateY(-100px) rotateX(90deg);
}
.dice-left{
    /* ...省略代码 */
    transform: translateZ(-100px) translateX(-100px) rotateY(90deg);
}
.dice-right{
    /* ...省略代码 */
    transform: translateZ(-100px) translateX(100px) rotateY(-90deg);
}
.dice-end{
    /* ...省略代码 */
    transform: translateZ(-200px);
}
.dice-bottom{
    /* ...省略代码 */
    transform: translateZ(-100px) translateY(100px) rotateX(-90deg);
}
```

现在还看不出效果来，接着往下~~

```css
.dice{
    /* ...省略代码 */
    animation: run 5s linear infinite;
    width: var(--boxWidth);
    height: var(--boxWidth);
}

/* 添加动画效果 */
@keyframes run {
  30%{
    transform: rotateX(360deg);
  }
  60%{
    transform: rotateX(360deg) rotateY(360deg);
  }
  100%{
    transform: rotateX(360deg) rotateY(360deg) rotateZ(360deg);
  }
}
```

此时可以看到旋转的效果

![GIF 2022-4-9 23-00-42](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\GIF 2022-4-9 23-00-42.gif)



最后加上一些 **猜点数** 的效果来助助兴~~

修改 HTML 代码如下：

> 添加一个 `checkbox` 元素搭配 `label` 来控制动画的播放和暂停。

```html
    <input type="checkbox" id="dices">

    <label for="dices">
        <div class="contanter">
        <!-- 省略代码 -->
        </div>
    </label>
```



修改 CSS 代码如下：

```css
.dice{
    /* ...省略代码 */
    animation: run 0.1s linear infinite;
    
}

#dices{
    display: none;
}

#dices:checked + label .contanter .dice{
    animation-play-state: paused;
}
```



最终效果如下：

![GIF 2022-4-9 23-08-06](D:\16高职计网1班\前端面试资料\个人总结的文章\图片\GIF 2022-4-9 23-08-06.gif)



:point_right: 演示的源代码在这里 [CodePen](https://codepen.io/jackbrens/pen/GRyGRVE) 链接。



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~😃