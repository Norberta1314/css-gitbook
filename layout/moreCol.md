# 第5节：三栏或多栏

三栏布局是将正常的文档流分为左中右三栏，一般业务场景来讲，左边与右边的栏目宽度固定，可能是菜单栏、分类等栏目，中间的栏目是页面主体部分。

### 流体布局

```
<div class="three-column">
  <div >something...</div>//左栏
  <div >something...</div>//右栏
  <div >Lorem ipsum dolor sit amet,</div>//中间栏
</div>
```

```
.three-column {
  width: 600px;
  height: 300px;
}
.three-column div {
  border: solid 1px #666666;
  height: 300px;
}
.three-column div:nth-child(1){//左边
  width: 120px;
  float:left;//左边的模块向左浮动
}
.three-column div:nth-child(3) {//中间
  margin: 0 240px 0 120px;
}
.three-column div:nth-child(2) {//右边
  width: 240px;
  float:right;//右边的模块向右浮动
}
```

三栏布局内最简单的流体布局了，利用流体特性， 使左边的模块向左浮动，右边的模块向右浮动，中间的模块顺着文档流的方向顺推达到三栏布局的目的，简单粗暴。

但是用流体布局的缺点也很明显，中间的部分最后显示，当网络环境不友好的时候不利于用户体验，不利于SEO

### 双翼飞布局

为了解决中间的部分最后显示的问题，可以使用双飞翼布局

```
<div class="three-column clearfix">
  <div>Lorem ipsum dolor sit amet,</div>//中间栏
  <div >Lorem ipsum dolor sit amet, consectetur adipisicing elit. </div>//左栏
  <div >Lorem ipsum dolor sit amet, consectetur adipisicing elit. </div>//右栏
</div>
```

```
.three-column {
  margin-left: 120px;			//给左栏留位置
  margin-right: 220px;		//给右栏留位置
}
.three-column div:nth-child(1) {
  float: left;
  width: 100%;					//中间模块占 100%宽度
  height: 300px;
}
.three-column div:nth-child(2){
  position: relative;
  width: 100px;
  height: 300px;
  margin-left: -100%;		//向左移动父元素的宽度，其实是将元素从中间块移到与中间块重叠并且在左边
  left: -120px;					//移出 three-column ，出现在左列
}
.three-column div:nth-child(3) {
  position: relative;
  width: 200px;
  height: 300px;
  margin-left: -200px; 	//向左移自己的宽度，使元素从中间块下面移到与中间块重叠并且在右边
  right: -220px;				//移出中间块，出现在右侧
}
```

咦？似曾相识？dei！就是和双栏布局 SEO 的优化一样，只不过是额外增加了右栏。

双飞翼布局的优缺点都很明显，牺牲代码易懂度与简洁度来达到先渲染中间块的效果，如果不看css添加的注释，新入门的前端同学可能根本搞不清楚上面的代码是在做什么，但是如果强调 SEO的话，还是双翼飞布局更好一些

### 三栏平分，并且每两栏之前右相同空隙

三栏平分每两栏之间有空隙，自然而然就能想到使每栏都带有 margin-right:10px，再使用子元素选择器使最后一个元素的 margin-right:0px ,在这里，我们有一种更优雅的方法实现三栏平分

```
 <div class="three-column-warp">
    <div class="three-column">
      <div>Lorem ipsum dolor sit amet</div>//左
      <div>Lorem ipsum dolor sit amet</div>//中
      <div>Lorem ipsum dolor sit amet</div>//
    </div>
  </div>
```

```
  .three-column-warp {
    overflow: hidden;			//隐藏因为margin-right带来的滚动条问题
  }
  .three-column {
    overflow: hidden;			//解决子元素浮动带来的高度塌陷
    margin-right: -10px;	//处理最后的margin
  }
  .three-column div {
    float: left;							//文档流向左浮动
    width: calc(33.3% - 12px);//计算每一列的宽度 33.3 - content-border-			padding
    height: 200px;
    margin-right: 10px;				//每一列元素带的 margin
    border: 1px solid #666666;
  }
```

要注意两个overflow的作用不一样， three-column的 overflow 可以替换其他清除浮动的方法，但是 warp 的 overflow， 目前我还没有找到更优的方式解决滚动条带来的问题