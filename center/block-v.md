# 第1节：块状元素水平居中

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

既然都已经用 0 作为最终值了，当然不能单独使用 margin:auto 实现垂直方向的居中了，但是天无绝人之路，我们可以通过其他方式帮助 margin:auto 实现垂直居中