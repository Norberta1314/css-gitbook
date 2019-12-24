# 第1节：对其

前面的文章我们使用大篇幅描述了 「中间对其」—— 居中 的问题，但在经常使用的布局中，不但有居中，左对齐与右对其使用频率也很高，我们在 boxSizing 遗留的问题：「 Button 的位置」，其实回过头看，我们 Button 的位置就是右对其。

左/右对齐没有居中那么多幺蛾子，

对于 inline 元素来说，

- 左对齐： text-align: left
- 右对齐：text-align: right

对于 block 元素来说，

- 左对齐： margin-right: auto
- 右对齐： margin-left:auto