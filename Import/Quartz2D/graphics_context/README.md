# Graphics Context
1. 图形上下文（图形环境）（Graphics Context）：是一个CGContextRef类型的数据

- 图形上下文不能绘制，绘制要通过贝塞尔路径绘制，或者将需要处理的图片加载到图形上下文中

- 图形上下文渲染（渲染即将图形加载到view.layer上，让view.layer显示），在图形上下文中，可以进行`绘画状态设置，添加水印，裁剪`等操作）

- 图形上下文不能显示图形，显示图形是view.layer的功能，所以要将图形上下文渲染到view.layer中

- 图形上下文相当于一个内存缓存区，在内存里面操作是最快的，比直接在界面操作快多了。

- 作用
    - 保存绘图信息、绘图状态
    - 决定绘制的输出目标（绘制到什么地方去？）
    （输出目标可以是PDF文件、Bitmap或者显示器的窗口上）

- 绘制好的图形 --保存--图形上下文--显示--输出目标

- 相同的一套绘图序列，指定不同的Graphics Context，就可将相同的图像绘制到不同的目标上

- Quartz2D提供了以下几种类型的Graphics Context：
    - Bitmap Graphics Context（图形上下文）
    - PDF Graphics Context（PDF上下文）
    - Window Graphics Context（视窗上下文）
    - Layer Graphics Context（图形层上下文
    - Printer Graphics Context（打印上下文）
    - 除了layer上下文,其他上下文(位图上下文)都需要自己手动创建
