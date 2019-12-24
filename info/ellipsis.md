# 第3节：省略号

张鑫讯在 2009 年写过一片文章[《](https://www.zhangxinxu.com/wordpress/2009/09/%E5%85%B3%E4%BA%8E%E6%96%87%E5%AD%97%E5%86%85%E5%AE%B9%E6%BA%A2%E5%87%BA%E7%94%A8%E7%82%B9%E7%82%B9%E7%82%B9-%E7%9C%81%E7%95%A5%E5%8F%B7%E8%A1%A8%E7%A4%BA/)[关于文字内容溢出用点点点(…)省略号表示](https://www.zhangxinxu.com/wordpress/2009/09/%E5%85%B3%E4%BA%8E%E6%96%87%E5%AD%97%E5%86%85%E5%AE%B9%E6%BA%A2%E5%87%BA%E7%94%A8%E7%82%B9%E7%82%B9%E7%82%B9-%E7%9C%81%E7%95%A5%E5%8F%B7%E8%A1%A8%E7%A4%BA/)[》](https://www.zhangxinxu.com/wordpress/2009/09/%E5%85%B3%E4%BA%8E%E6%96%87%E5%AD%97%E5%86%85%E5%AE%B9%E6%BA%A2%E5%87%BA%E7%94%A8%E7%82%B9%E7%82%B9%E7%82%B9-%E7%9C%81%E7%95%A5%E5%8F%B7%E8%A1%A8%E7%A4%BA/)，其中为了浏览器兼容hack了很多代码，表达了对网上流行的内容溢出现有方法的不满， 最后提出了自己的  负边距 定位法。

但是现在都 9102 年了，所有的浏览器都已经支持 text-overflow:ellipsis; 方法了，也极大的方便了我们制作文字省略号的效果。

但是聪明的大家怎么可以止步于此呢？

让我们来继续进阶版文字省略号：

### inline 还是 block？

![图片](https://uploader.shimo.im/f/hCIag73U2qkEfYrN.png!thumbnail)

如图，text1需要溢出时表示省略号，text2需要围绕 img。

如果先不考虑溢出，那么问题很简单：

```
<div style="width: 500px">
  <img src="" alt="">
  <span class='title'>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</span>
  <span>Lorem ipsum dolor sit amet, consectetur adipisicing elit. A ab adipisci aut, consectetur dolores ducimus esse eum laborum modi mollitia natus nisi obcaecati optio porro quibusdam quidem quisquam, vero, voluptate.</span>
</div>
```

css:

```
img {
  float: left;
  width: 150px;
  height: 150px;
}
```

然后我们给 title 加上省略：

```
white-space: nowrap;
overflow: hidden;
text-overflow:ellipsis;
```

然后发现：
![图片](https://uploader.shimo.im/f/1BRAElqd6lMofuwf.png!thumbnail)

咦？事情并没有像我们想象的这么简单，title 没有依照约定好的规则在 img 后面，而是缩进到了 img 下面，这可怎么办！

通过审查元素我们发现：

![图片](https://uploader.shimo.im/f/tBov00OxHy4B9TDy.png!thumbnail)

.title 的 width 并没有像我们预期中的缩起来为 500 - 150 = 350， 而是根据 title 本身的长度，延伸到了 419，通过一项一项筛检元素我们发现，是 white-space: nowrap; 属性导致 title 的长度很“硬气”得折不断。那怎么样才能使 title 「折腰」呢？先不卖关子了，给 title 设置 display:block 就可以了：

![图片](https://uploader.shimo.im/f/i710XpniP30eU5l9.png!thumbnail)

确认过眼神，是我们想要的效果！

但是感觉怪怪的，是不是哪里少了点什么？

！这里我们可以直接用 h1-h6 标签啊！本身表达的就是标题的含义，而 h1-h6 也是block 属性，这样我们就可以直接实现效果了：

![图片](https://uploader.shimo.im/f/kmq891K7pX4c5jmC.png!thumbnail)

虽然有骚操作，但是截止现在我们解决的都是单行文本的省略号，如果我们的业务场景是要对多行文本实现省略号怎么解决！

### 多行文本省略号

通过 Google 搜索 「多行文本省略号」，兼容性比较强的是下面这段代码：

```
p{
position:relative;
line-height:1.4em;
/*设置容器高度为3倍行高就是显示3行*/
height:4.2em;
overflow:hidden;
}
p::after{
content:'...';
font-weight:bold;
position:absolute;
bottom:0;
right:0;
padding:0 20px 1px 45px;
background:#fff;
}
```

但是这段代码有一个很明显的缺陷：就是无法判断文字的高度，无论文本溢不溢出都会显示省略号，这样做显然很不优雅。
在网上找到有一位大神[「利用 float 特性实现多行文本截断」](https://github.com/happylindz/blog/issues/12)，代码在此：

```
.wrap {
  height: 40px;
  line-height: 20px;
  overflow: hidden;
}
.wrap .text {
  float: right;
  margin-left: -5px;
  width: 100%;
  word-break: break-all;
}
.wrap::before {
  float: left;
  width: 5px;
  content: '';
  height: 40px;
}
.wrap::after {
  float: right;
  content: "...";
  height: 20px;
  line-height: 20px;
  /* 为三个省略号的宽度 */
  width: 3em;
  /* 使盒子不占位置 */
  margin-left: -3em;
  /* 移动省略号位置 */
  position: relative;
  left: 100%;
  top: -20px;
  padding-right: 5px;
}
```

原理：
[「利用 float 特性实现多行文本截断」](https://github.com/happylindz/blog/issues/12) 但是这样做也有一个缺陷，就是只能实现定高的省略号溢出。

就是不管文字是一行还是两行，warp 都会占两行的位置，

![图片](https://uploader.shimo.im/f/Z2rGmMTppzUW0MGj.png!thumbnail)

看来到现在位置， 使多行文本居中还没有找到纯 CSS 实现靠谱的解决方案。如果要完美高度实现CSS