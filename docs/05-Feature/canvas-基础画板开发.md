--- 
title: 画板开发 from 作业批改项目中
descript: 主要功能涉及: 图片显示居中,画笔,橡皮,拖拽,缩放,撤销前进,保存上传,解决keeplive导致图片缓存无法更新的冲突
---

### 整体功能实现

- 工具栏: 用于切换功能
- 在 canvas 上进行事件委托,分别有 mouseEnter,mouseDown,mouseMove,moseUp
- 每一次交互前都需要`save`初始状态,完成交互之后`restore`到之前的状态
- ctx.moveTo 来定位点,ctx.lineTo 来连线,ctx.stroke 来绘制,ctx.fill 来填充
- html 结构

```html
<div class="Container">
  <canvas></canvas>
</div>
```

1. 图片显示: 使用 new Image 来 load 图片,onload 之后然后再将画板的 backgroundImage 设置为图片 url(url 还要加上当前时间,防止缓存) | 使用 canvas.drawImage.
2. 居中显示:通过计算 image 的宽度和容器的宽度,使用 css 的`scale`属性进行对于比例的缩放.
3. 画笔:mouseDown 开始绘制第一个点,随后跟随鼠标绘制
4. 橡皮: 设置 ctx.globalCompositeOperation 属性规定重叠部分被去除, 在 mouseDown 的时候开始第一次橡皮绘制,先用 ctx.fill()清除内容,在画上橡皮 => 进入 mouseMove, 先清除上一个位置绘制的橡皮,再进行下一个位置橡皮的绘制,并使用`requestAnimationframe`中来完成绘制函数, 在 mouseUp 的时候完成
5. 拖拽: 注意边界即可,不让用户将画板移出 container
6. 缩放: 非线性倍率缩放
7. 撤销前进: 维护两个数组,一个 revokeData,一个 saveData,**每次撤销将 saveData 顶部数据弹出,push 进 revokeData,反之亦然**,使用 ctx.putImageData 绘制操作后的图片
8. 保存上传: 使用 canvas.toBlob 和 formData 来上传图片,
