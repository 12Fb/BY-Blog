---
title: MVC 架构
description: MVC架构的介绍,以及带来的一些问题

---

## MVC的全称
**M(model):模块  V(view):视图  C(controller): 控制器**
----在mvc中, 执行顺序是这样的: **Controller -> view -> Model->Controller->循环**,可以看到当只有一个Controller的时候,整个架构是非常清晰的,但是在大型项目总,往往有多个Controller,model和view,而**model和view是多对多的**,在多个controller,还有一大堆多对多的**model和view**存在的情况下,必然会变的很混乱

