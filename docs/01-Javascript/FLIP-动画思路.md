---
title: FLIP动画
description: 对天才动画思路FLIP的动画实践
---
## FLIP
1. FLIP的的过程就跟他的名字一样: FIRST -> LAST -> **INVERT -> PLAY**
### First 和 Last
    - First和Last分别拿到一个元素的**初始状态和最终状态**,在Fitst和Last的中间,**是改变元素位置或者css状态的JS**
### Invert: 反转
    - Invert就是**将元素改变回元素的初始位置**,由于这是在同一帧中完成的,所以并不会给我们有视觉上的感受,**看起来元素就像没有变过一样**
### Play: 开始执行动画
    - 再**下一帧**将元素的反转效果移除,同时加上transition之类的过渡,这样元素就有了动画效果并驾向目的地      