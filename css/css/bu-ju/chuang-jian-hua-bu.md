---
description: 不溢出-两侧自适应-盒子
---

# 创建画布

画布是指我们在浏览器里的可视区域，我们可以人为的划出这个区域。这有一些好处，如：

* 我们可以自由的在画布内创建元素，而不用害怕画布内的元素越界
* 也不必害怕用户会看到我们不想展示给他们看的糟糕的边角。

如何创建？

画布的创建非常简单，与我们真实世界里的画布不同，浏览器里的画布概念更像是一个相框，而人们只能看到相框内部的画面，相框外部的人们并不能看到，但我们仍旧可以在外侧“绘画”。

```css
:root{
    --vw100:calc(100vw - 17px)
}
/**
 * 绝对与自适应
 * 我们在制作任何一种css样式时，总是会被绝对或自适应的问题拦住。
 * 而解决这类问题的方式，通常要判断以下几个问题：
 *     1. 内部内容是否会出现超出盒子border的情况
 *     2. 我们是否希望展示超出的部分
 *     3. 我们通过何种手段来展示超出部分
 *         · 如果是文本，我们通常采用min-width的方式
 *         · 如果是盒子样式，我们通常采用overflow的方式
 */
.cvs{
    width: 1080px
    
}
```

