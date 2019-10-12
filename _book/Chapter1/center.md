# 第1节：居中

在css的布局世界中，元素居中可谓是再常见不过的一个场景了，要说居中，可能大家有数种方法可以实现，但是在数十种方法中找到最优的解决方案却不是每个人都能实现的，我们今天就来聊聊居中问题中的最佳实践方案吧。

因为css的元素分为块状元素`block`和行内元素`inline`,每种元素的居中方式又不相同，所以，我们先从块级元素说起吧。

## 块状元素的水平居中问题

聊到块状元素的水平居中问题，多数人能想到的第一个方法就是对元素设置`margin:auto`，那么我们就从这个css属性说起吧。

### margin:auto

多数人可能已经对`margin: auto `这个熟悉的不能再熟悉了，但是却少有人了解其实现居中的原理。

**CSS 2 规范 10.3.3 [Block-level, non-replaced elements in normal flow](https://www.w3.org/TR/CSS2/visudet.html#blockwidth)：**

> width计算方式：['margin-left'](https://www.w3.org/TR/CSS21/box.html#propdef-margin-left) + ['border-left-width'](https://www.w3.org/TR/CSS21/box.html#propdef-border-left-width) + ['padding-left'](https://www.w3.org/TR/CSS21/box.html#propdef-padding-left) + ['width'](https://www.w3.org/TR/CSS21/visudet.html#propdef-width) + ['padding-right'](https://www.w3.org/TR/CSS21/box.html#propdef-padding-right) + ['border-right-width'](https://www.w3.org/TR/CSS21/box.html#propdef-border-right-width) + ['margin-right'](https://www.w3.org/TR/CSS21/box.html#propdef-margin-right) = width of [containing block](https://www.w3.org/TR/CSS21/visudet.html#containing-block-details)
>
> **If both `margin-left` and`margin-right`are 'auto', their used values are equal.**
> 如果 margin-left 和 margin-right 都是 auto，他们使用的值平分了剩余的空间，也就实现了元素的水平居中效果

对于非替换元素：[10.3.4 Block-level, replaced elements in normal flow](https://www.w3.org/TR/CSS2/visudet.html#block-replaced-width)

> The used value of 'width' is determined as for inline replaced elements. Then the rules for non-replaced block-level elements are applied to determine the margins.
> **width 遵循 inline 替换元素， margin 遵循非替换元素的规则**



> 【替换元素】：浏览器根据元素的标签和属性，来决定元素的具体显示内容，如`<img>、<input>、<textarea>、<select>、<object>`，这些元素往往没有实际的内容，即是一个空元素；
> 【非替换元素】：内容直接表现给浏览器的元素，例如`<p>、<h1>`到`<h6>`等等·

规范明确规定了`margin:auto `时水平方向 width 的计算方式，而且是 CSS2 中的规范，浏览器都可以兼容，故为实现块级元素水平居中的上上策

既然`margin:auto `实现水平居中如此简单，所以我们能不能用`margin:auto `对元素进行垂直居中呢？答案是：

### margin:auto 不能直接对元素垂直居中

CSS 2 规范 10.6.3 [Block-level non-replaced elements in normal flow when 'overflow' computes to 'visible'](https://www.w3.org/TR/CSS2/visudet.html#normal-block) 中提及：

> If ['margin-top'](https://www.w3.org/TR/CSS21/box.html#propdef-margin-top), or ['margin-bottom'](https://www.w3.org/TR/CSS21/box.html#propdef-margin-bottom) are 'auto', their used value is 0. 
>
> **如果`margin-top`或者`margin-bottom`属性的值是 auto , 则他们实际用 0 作为**值

既然都已经用 0 作为最终值了，当然不能单独使用 margin:auto 实现垂直方向的居中了，但是天无绝人之路，

## 块状元素的垂直居中

我们可以使用其他的方式来帮助`margin:auto`实现元素垂直方向的居中

### Absolute + margin:auto

CSS 2 规范 10.6.4  [Absolutely positioned, non-replaced elements](https://www.w3.org/TR/CSS2/visudet.html#abs-non-replaced-height) 中有：

> 对于 绝对定位非替换元素高度的计算规则：
> 'top' + 'margin-top' + 'border-top-width' + 'padding-top' + 'height' + 'padding-bottom' + 'border-bottom-width' + 'margin-bottom' + 'bottom' = height of containing block
>
> > 并且有两个 if 与我们场景相关：
> > If all three of 'top', 'height', and 'bottom' are auto, set 'top' to the static position and apply rule number three below.
> > If none of the three are 'auto': If both 'margin-top' and 'margin-bottom' are 'auto', solve the equation under the extra constraint that the two margins get equal values.
> > 如果 `top`, `height`, `bottom` 三个属性都是 auto,top 取当元素是`static position`时的值，其他的计算看下面三条规则（此处省略）
> > 如果上面提及的三个属性都不是 auto，并且如果 margin-top 和 margin-bottom 都为 auto，平分剩余的空间使两个 margin 值相等

**那么问题就很明确了，我们可以对子元素设置 position:absolute ,并且四周的距离设置为 0 ，同时设置`margin:auto`:** 

```
<div class="parent">
  <div class="child">block元素</div>
</div>
```

```
.parent {
  position: relative;
  width: 400px;
  height: 400px;
}
.child {
  position: absolute;
  width: 150px;
  height: 150px;
  top:0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
```

这样.child元素就在parent元素内部实现水平垂直居中
如果我们仅仅想对元素进行垂直居中，那么根据上文提到的Absolute的规范，我们可以设置ausolute的属性'top:0;bottom:0',那么margin：auto平分上下空间使得margin值相等，从而实现元素单纯的垂直居中，如果子元素要在父元素的左侧垂直居中，那么根据absolute的规范，我们设置'left：0'即可，右侧同理；当然，此方法也可以实现元素的水平居中，设置ausolute的属性'left:0;right:0'，原理相同

当我们不想使子元素进行绝对的垂直居中时，想使元素相对垂直居中的位置偏下z px时，我们把问题分解：

元素相对垂直居中的位置再向下偏移 z px -> 元素居中的中线比绝对垂直居中的中线向下偏移 z px -> top向下偏移z px * 2 

上面阐述的步骤可能不是很直观，让我们举个🌰：父元素的高度是 100 px， 我们想让子元素的位置比绝对垂直居中的位置向下偏20px，那么我们：

元素垂直居中的中线 50px + 20px -> 元素居中的中线 70px -> 元素居中的中线离父元素底部 30px  ，那么子元素绝对居中的顶部的线离父元素底部距离为 30px  *2 = 60px ，那么top的偏移值应该是 20px * 2 = 40px

![图片](https://uploader.shimo.im/f/YSHYjlADf8oSnd96.png!thumbnail)

通过借助绝对定位与 margin:auto 使元素居中是规范中明确规定的，没有二意性而且大部分浏览器都有实现，没有兼容问题，而且我们现在也没有发现什么特殊的场景让使`absolute`+`margin:auto` 对块级元素的居中不能完美适配。

但是借助 top， bottom = 0 时垂直居中，我们还可以借助另一种方式使 margin 可以帮助元素实现垂直居中

### Absolute + 负margin

```
.parent {
    position: relative;
    width: 600px;
    height: 300px;
}
.child {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 50%;
    margin-top: -50px; //(content+padding+border)/2: ;
}
```

先使用 top: 50%（根据父元素height计算）,将子元素的 top 线推到父元素的中线处，

![图片](https://uploader.shimo.im/f/0Dcb0TVtPjwmVBzO.png!thumbnail)

再根据 margin-top:-50px（子元素高度的一半），将子元素推上去，使子元素与父元素的中线一致。

![图片](https://uploader.shimo.im/f/XDIKHzN0uxkSMVdS.png!thumbnail)

但是这种方式很遗憾的是必须元素的 height 已知，那如果元素的 height 未知，怎么办？

<u>负margin小技巧：</u>

> 负margin-top:元素上移
>
> 负margin-left:元素左移

**要注意的是 `margin-right` 与 `margin-bottom` 的移动规则与 `margin-top` 和 `margin-left` 不一样，也不能借此实现该元素的水平垂直居中。**

### absolute + translate

我们可以使用 `translate`， 使子元素移动：

```
.chlid {
    position: absolute;
    top: 50%;
    transform: translate(0, -50%);
}
```

利用 `transform` 将子元素推上去子元素的 50%,

这种方式令元素垂直居中的缺点是不是所有浏览器都支持，遇到了头疼的 IE8 ， `transform` 就直接歇菜了。



到此，我们只了解了块状元素居中的方式，在文档流中，除了块状元素还有另外一大块——行内元素。相比块状元素而言，行内元素的居中更为难以理解和复杂，所以，你准备好了么？

## 行内元素的水平居中

柿子要挑软的捏，学习也要挑简单的学起：

### Text-align:center

对于行内元素来说，有属性天然支持水平居中，我们可以对父元素设置 `text-align: center` 来对子元素进行水平居中：

```
<div class="parent">
  <span class="child">inline 元素</span>
</div>
```

```
.parent{
   text-align: center;
}
```

当我们需要对一行多个元素同时进行水平居中，其实这个需求很常见，比如豆瓣展示电影：![图片](https://uploader.shimo.im/f/rgAhH5iAT5wPWSF6.png!thumbnail)

淘宝展示商品，等等……

这里的切图，如果使用 `float:left` 那么前后的空格就很难处理，最为直观的解决思路就是让其中的内容两端对齐来实现居中。

### justify

`text-align:justify` 可以用在列表两端对齐的布局上面，也是一种居中。

> 列表两端对齐就是每行列表元素的第一个元素与父元素左边缘重合，最后一个元素与父元素的右边缘重合，

但是需要注意的是 列表元素首尾标签留空或换行，比如：

```
<ul>
  <li></li><li></li>//第一个li元素与第二个li元素之间没有留空或换行
</ul>
<ul>
  <li></li> <li></li>//第一个li元素与第二个li元素之间留空
</ul>
<ul>
  <li></li>//第一个li元素与第二个元素之间换行
  <li></li>
</ul>
```

还需要注意，列表两端对齐的前提是内容超过一行，而且对最后一行不生效，所以`text-align:justify` 不会产生任何效果，解决方法是:
`text-align-last:justify`

不幸的是，` text-align-last:justify`不是所有浏览器都支持，对于不支持` text-align-last` 的浏览器，解决方法可以是人工再生成一行文本，把人工生成的文本隐藏，那么自然就可以实现两端对齐了



聊完了行内元素的水平居中，还有「垂直居中」这块硬骨头等着我们要啃：

## 行内元素的垂直居中

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

`display:table-cell` 对宽度敏感： `table-cell` 内的元素宽度设置为： width:100% 的时候，因为这是一种规范未定义行为，不同的浏览器会产生不同的效果

除此之外，`table-cell` 在垂直居中一届中还是占很高的地位的，尤其是在父元素高度未知的场景下，并且比 `translate` 更好在兼容性更强一些

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

`justify-content`属性定义了项目在主轴（水平）上的对齐方式。

`justify-content`可取值：

- flex-start（默认值）：左对齐
- flex-end：右对齐
- center： 居中
- space-between：两端对齐，项目之间的间隔都相等。
- space-around：每个项目两侧的间隔相等（类似margin-left = margin-right）。

父级使用`display:flex` `justify-content:center`轻松实现元素的水平居中

`align-items`属性定义项目在交叉轴（垂直）上如何对齐

`align-items`可取值：

- flex-start：交叉轴的起点对齐。
- flex-end：交叉轴的终点对齐。
- center：交叉轴的中点对齐。
- baseline: 项目的第一行文字的基线对齐。
- stretch（默认值）：如果项目未设置高度或设为 auto，将占满整个容器的高度。

父级使用`display:flex` ` align-items:center`可以轻松实现元素的垂直居中

`flex`作为现代的布局方案，只需要几行代码就能优雅的实现垂直水平居中，然而这么逆天的神器，为什么没有作为最佳实践呢？因为神器目前还有缺陷。

当前来说，移动端基本支持`flex`布局，而pc端flex在支持ie6-9上还存在一些问题，所以在移动端布局方案中，`flex`推荐适用。

另外，适用`flex`布局需要注意的是：`flex`容器内子元素的`float`、`clear`和`vertical-align`属性将失效，所有子元素自动成为容器成员`flex item`。

# 分割场景，布局经验

读到这里，相信大家对元素的垂直水平居中问题有了比较明确的了解了，但是在实际操作中，场景往往是多变的，所以说，想要在实际开发中处理垂直水平居中问题，还需要有对场景处理的经验和原则

比如说，我们学会了对块级元素的垂直水平居中，可以轻轻松松的实现元素的垂直水平居中，那么，现在有三个块级元素，要全部实现居中呢？比如下面的场景:

![图片](https://uploader.shimo.im/f/W3qBJRVgNCA6uw68.png!thumbnail)
要实现多个块级元素的垂直水平居中，我们不妨给三个子元素设置为	inline-block	后外面套一个	div	，然后问题就变成了单个块级元素的水平居中问题

假如场景再复杂，变成多行块级元素，每行都有多个块级元素，那么处理问题的一般思路，都是给每一行的元素套一个父级后又给这些父级再嵌套一个父级，直到问题化简成一个块级元素的水平居中问题。

这里我们就能总结出一个规律了，对于情况复杂的场景，要实现垂直水平居中的布局，那么，我们要分解问题，将复杂多变的场景，分解成可控的单一场景，比如，将多个块级元素的垂直水平居中问题，变成一个块级元素的垂直水平居中问题等等，这样，轻轻松松就解决了css中令人头疼的居中问题了。



