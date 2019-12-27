# 第4节：行内元素垂直居中

### line-height

对于单行元素的垂直居中，可以使 ` line-height ` 设置的数值与要居中的盒子高度（注意并不是 height 哦）相等，本质是文字的上边距与下边距相等，` line-height ` = 文字上边距 + 文字高度 + 文字下边距，当 ` line-height `与要居中的盒子高度相等时，文字可以呈现居中。

但是用这个方法的缺点是：有一些字体呈现出天然下沉（以微软雅黑为例），表现出来的居中并不是绝对的居中。但是由于大多数单行文字的使用场景使用字号都比较小，文字下沉不明显，而且可以和 UI 协商在单行文字垂直居中的场景下避免使用微软雅黑字帖。所以**使用 ` line-height ` 是实现单行元素绝对居中的最佳实践**

```
<div class="parent">
  <span class="child">inline 元素</span>
</div>
<style>
.parent {
  width: 600px;
  height: 100px;
}
.child {
  line-height: 100px;//不用设置 height 哦，会自动实现在100px的盒子里面的垂直居中
}
</style>
```

### vertical-align

对于多行文本，单独使用 `line-height` 不是一个好的主意，我们需要使用其他手段了。再看手段之前我们可以先了解一个属性：` vertical-align:middle`，

官方对 `vertical-align` 的解释是：使「元素中部」与「父元素基线加上x字母高度的一半」对齐，这也解释了为什么

```
<div class="parent">
  <span>
  	Lorem ipsum dolor sit amet, consectetur adipisicing elit. Non officia quasi saepe!
  	Ad aliquid, cum, deleniti eaque enim est eveniet illo laborum magni nihil odio quidem,
  	quis tenetur? Alias, voluptates!
  </span>
</div>
```

```
span {
    vertical-align: middle;
}
```

并不能使元素居中，因为我们并不能描述出父元素的基线在哪里，更不要说加上 x字母高度的一半了。但是没有基线我们可以创造基线，我们可以通过三种方式来实现对多行文本的垂直居中：

### table-cell

```
.parent {
    display: table;
    width: 600px;
    height: 200px;
}
.parent span {
    display: table-cell;
    vertical-align: middle;
}
```

在父元素为 `table`，子元素为 `table-cell` 的时候，`vertical-align: middle` 的含义是 「使单元格内边距盒模型在该行内居中对齐」，这样就可以轻松实现垂直居中。

你以为垂直居中到这里就万事大吉了么？不！ `display:table-cell` 有一些很局限的地方，比如：

对元素设置了 display:table-cell 的时候对其设置 margin 无反应，

`display:table-cell` 会被 `float`,`absolute` 破坏，所以不可以和 `float`，`absolute` 同时使用

`display:table-cell` 对宽度高度敏感： `table-cell` 内的元素宽度设置为： width:100% 的时候，因为这是一种规范未定义行为，不同的浏览器会产生不同的效果

`display:table-cell` 还有`创建匿名表格元素`的规则，什么是创建匿名表格元素呢？

> [css 创建匿名表格元素](w3.org/TR/CSS2/tables.html#anonymous-boxes)
>
> Document languages other than HTML may not contain all the elements in the CSS 2.1 table model. In these cases, the "missing" elements must be assumed in order for the table model to work.` Any table element will automatically generate necessary anonymous table objects around itself, consisting of at least three nested objects corresponding to a 'table'/'inline-table' element, a 'table-row' element, and a 'table-cell' element`. Missing elements generate [anonymous](https://www.w3.org/TR/CSS2/visuren.html#anonymous) objects (e.g., anonymous boxes in visual table layout) according to the following rules:
>
> 任何表格元素将自动在它周围自动生成匿名表格对象，由至少三个对应的嵌套对象组成 :`table/inline-table`元素，`table-row`元素、`table-cell`元素。
>
> 说人话就是：如果我只对 A 元素定义了 table-cell ，浏览器会自动为它生成  `table-row` 的爸爸和`table/inline-table` 的爷爷

除此之外，`table-cell` 在垂直居中一届中还是占很高的地位的，尤其是在父元素高度未知的场景下，并且比 `translate` 更好在兼容性更强一些。

（小预告：在考虑对 IE 兼容性的情况下，有一种垂直居中是其他方式很难实现的，只能用 table-cell ，我们在后期会揭晓

在我们需要使用 `margin` 的时候，我们要用什么方式居中呢？

### 隐式幽灵节点

在行框盒子前面，会有一个看不见但是确实存在的节点，我们姑且称之为幽灵节点。我们可以给幽灵节点设置基线，使 `span` 元素中线与幽灵元素中线对齐，达到我们的目的：

```
.parent {
  	line-height: 300px;
    width: 600px;
    height: 300px;
    background: gray;
}   
.parent span {
    display: inline-block;
    vertical-align: middle;
    line-height: normal; //划重点，要把 line-height 设置成 normal, 要不然会继承300，就很恐怖了
}
```

我们设置幽灵节点的高度以及幽灵节点的基线（通过 `line-height`），来设置幽灵节点的x-height, 是`span`的中线与幽灵节点的中线对齐，就可以了

### 显式幽灵节点

隐式的幽灵节点阅读不方便语意化不高，而且需要知道父元素高度，**如果不知道父元素高度**，我们应该如何使元素居中呢？

```
.parent::before {
    content: " ";
    display: inline-block;
    height: 100%;
    vertical-align: middle;
}
.multiple-line-ghost span {
    display: inline-block;
    vertical-align: middle;
}
```

使两个节点的中线对齐，就可以达到我们使文本垂直居中的目的

但是居中的方式这么多，感觉头都要大了，有没有什么方式可以简单粗暴的实现元素的居中呢？

有当然是有的，神器：

## flex 布局大法好

其实更简单粗暴的居中方式是使用 flex 居中，附上阮一峰老师的[ flex教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)，这里提及一些我们这里常用的

`justify-content` 属性定义了项目在主轴（水平）上的对齐方式。

`justify-content `可取值：

- flex-start（默认值）：左对齐
- flex-end：右对齐
- center： 居中
- space-between：两端对齐，项目之间的间隔都相等。
- space-around：每个项目两侧的间隔相等（类似margin-left = margin-right）。

父级使用 `display:flex` `justify-content:center `轻松实现元素的水平居中

`align-items` 属性定义项目在交叉轴（垂直）上如何对齐

`align-items` 可取值：

- flex-start：交叉轴的起点对齐。
- flex-end：交叉轴的终点对齐。
- center：交叉轴的中点对齐。
- baseline: 项目的第一行文字的基线对齐。
- stretch（默认值）：如果项目未设置高度或设为 auto，将占满整个容器的高度。

父级使用 `display:flex` ` align-items:center` 可以轻松实现元素的垂直居中

`flex `作为现代的布局方案，只需要几行代码就能优雅的实现垂直水平居中，然而这么逆天的神器，为什么没有作为最佳实践呢？因为神器目前还有缺陷。

当前来说，移动端基本支持 `flex` 布局，而pc端flex在支持ie6-9上还存在一些问题，所以在移动端布局方案中，`flex  `推荐适用。

另外，适用 `flex` 布局需要注意的是：`flex` 容器内子元素的 `float`、`clear` 和 `vertical-align` 属性将失效，所有子元素自动成为容器成员 `flex item`。

# 分割场景，布局经验

读到这里，相信大家对元素的垂直水平居中问题有了比较明确的了解了，但是在实际操作中，场景往往是多变的，垂直和水平居中往往是需要交杂在一起处理，所以，想要在实际开发中处理垂直水平居中问题，还需要有对场景处理的经验和原则。

考虑复杂的场景：

<img src='./img/img-4.png'>

绿色的盒子该如何布局呢？，我们可以把问题拆解开：

<img src='./img/img-5.png'>

将问题转化为使用绿色盒子相对紫色盒子 `text-align: justify` , 紫色盒子相对粉色盒子垂直水平居中。

如果发现实际业务场景中遇到比较复杂的居中问题，不妨拆解开，把每个元素身上背负的责任减少，一一化解，变成单一可控的问题。

这样子，css 居中问题就轻轻松松解决啦！