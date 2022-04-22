# IntersectionObserver版 — 你不知道的懒加载实现方法

**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第1天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



[IntersectionObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)  MDN官方文档

## 一、 构造器

用法非常简单。

```javascript
let ob = new IntersectionObserver(callback, option);
```

上述代码里，`IntersectionObserver` 是浏览器原生自带的构造函数。

接收两个参数：

- `callback` 第一个参数回调函数，是由被观察元素可见性变化时触发的（必填）。

- `option` 第二个参数是一个配置对象（选填）。



## 二、callback参数

发生 `可见性变化时` 接收一个 `entries`  参数，该参数是一个数组，提供被观察元素的信息。

```javascript
let ob = new IntersectionObserver(entries => {
    entries.forEach(item => {
        // item有哪些属性具体见下方表格
        console.log(item);
    })
});
```

| 属性                                                         | 介绍                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [boundingClientRect](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect) | 返回元素的大小及其相对于视口的位置                           |
| intersectionRatio                                            | 目标元素的可见比例，即 `intersectionRect` 和 `boundingClientRect` 的比例，完全可见时为`1`，完全不可见时小于等于`0` |
| boundingClientRect                                           | 目标元素的矩形区域的信息                                     |
| intersectionRect                                             | 目标元素与视口（或根元素）的交叉区域的信息                   |
| isIntersecting                                               | 元素是否可见，可见为true，不可见为false                      |
| target                                                       | 被观察的目标元素，是一个 DOM 节点对象                        |
| rootBounds                                                   | 根元素的矩形区域的信息，`getBoundingClientRect()`方法的返回值，如果没有根元素（即直接相对于视口滚动），则返回`null` |

`callback`一般会触发两次。一次是目标元素刚刚进入视口（开始可见），另一次是完全离开视口（开始不可见）。



## 三、option参数

`option` 配置项是一个对象类型，具体的属性有：

| 属性       | 介绍                                                         |
| ---------- | ------------------------------------------------------------ |
| root       | 指定父级元素，默认为 `根元素`                                |
| rootMargin | 字符串类型，默认为 `'0px 0px 0px 0px'`  (分别为上右下左，四个方向) |



```javascript
let ob = new IntersectionObserver(callback, {
    root: document.querySelector(element),
    // 上面往里缩了20px，右边往外扩散了30px，下边往外扩散了20px，右边0px
    rootMargin: '-20px 30px 20px 0px'  
});
```

![image-20220401174430027](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220401174430027.png)





## 四、实现懒加载

有时，我们希望某些静态资源（比如图片），只有用户向下滚动，它们进入视口时才加载，这样可以节省带宽，提高网页性能。这就叫做"懒加载"。

之前我们都是通过 `scroll` 事件配合 `图片距离顶部的距离` 实现的，现在我们利用 `IntersectionObserver` 这一个`api` 就可以轻松实现。

**下面我们来模拟触底时，实现懒加载，在页面中插入图片。**

`html和css` 代码如下：

```html
<style>
.layload{
    margin: auto;
    margin-top: 50vh;
    width: 52vw;
    background-color: #eee;
 }
.layload img {
    width: 250px;
    height: 300px;
    margin-left: 20px;
}
.layload footer{
    background-color: #ccc;
}
</style>

<div class="layload">
    <main></main>
    // 只要footer是可见的，即isIntersecting为true，就在main中插入
    <footer>加载中</footer>
</div>
```



`js` 代码如下：

> 为了优化页面渲染，我们使用document.createDocumentFragment()来插入图片
>若对 createDocumentFragment() 不熟悉的，可以查看 [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createDocumentFragment)

```javascript
// 获取DOM元素
let main = document.querySelector('.layload main');
let footer = document.querySelector('.layload footer');

// 新建 IntersectionObserver 的实例
let observer = new IntersectionObserver(entries => {
    entries.forEach(item => {
        // 一旦元素可见，调用插入图片的函数 imgLoad()
        if (item.isIntersecting) {
            imgLoad();
        }
    })
})
observer.observe(footer); // 添加被观察的元素



let df = document.createDocumentFragment();
function imgLoad () {
    // 利用setTimeout 模拟异步请求
    setTimeout(() => {
        for (let i = 0; i < 9; i++) {
            let img = document.createElement('img');
            img.src = 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F201608%2F20%2F20160820150344_wC4fk.thumb.1000_0.jpeg&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1651400145&t=a7b3a0cbce5110c667c44a3bd2fdb280';
            df.appendChild(img);
    	}
        main.appendChild(df);
    }, 500)
}
```



## 五、总结

懒加载的效果还有缺陷，若是屏幕大小发生改变时，该如何控制呢，这些就等着各位客官来解答哦~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~:smiley:

