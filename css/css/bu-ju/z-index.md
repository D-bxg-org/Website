# z-index

我们把z-index作为一个图层的概念来去使用。

我们把它的数值理解为，每个位上的数值都表达特定含义。

例如：

* 0-0-0-0
* 千位上的是图层
* 百位上的是在图层上的前后
* 十位是在组件内部前后关系
* 个位是单独调整时使用。

这样我们就能避免z-index的数值容易出现混乱，或者过大，或者利用不均的情况。
