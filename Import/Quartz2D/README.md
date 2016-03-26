# 什么是Quartz2D
1. Quartz 2D是一个二维(二维即平面)绘图引擎，同时支持iOS和Mac系统
    1. 什么是二维?平面
    - 什么是引擎?经包装的函数库，方便开发者使用。也就是说苹果帮我们封装了一套绘图的函数库
    - 同时支持iOS和Mac系统什么意思?用Quartz2D写的同一份代码，既可以运行在iphone上又可以运行在mac上，可以跨平台开发

- 作用：
    1. 自定义UI控件- 绘制文字绘制图形（主）
    - 截图\裁剪图片(矩形图片裁剪成圆形图片）（主）
    - 线条\三角形\矩形\圆\弧等
    - 绘制\生成图片(图像)
    - 读取\生成PDF
    - 手势解锁
    - 报表：折线图、饼状图、柱状图（苹果写的都不好，用第三方框架）

- Quartz2D的API是纯C语言的（API指应用程序界面），来自于Core Graphics框架，其数据类型和函数基本都以CG作为前缀
    1. CGContextRef
    - CGPathRef
    - CGContextStrokePath(ctx);


## Quartz2D在iOS开发中的价值
1. 利用UIKit框架提供的控件，拼拼凑凑，能搭建和现实一些简单、常见的UI界面
- 但有些UI界面极其复杂、而且比较个性化，用普通的UI控件无法实现，这时可以利用Quartz2D技术将控件内部的结构画出来，自定义控件的样子
- iOS中大部分控件的内容都是通过Quartz2D画出来的
- Quartz2D在iOS开发中很重要的一个价值是：自定义view（自定义UI控件）


### Quartz2D自定义view的步骤
1. 新建一个类，继承自UIView

2. 在-(void)drawRect:(CGRect)rect方法实现下述几步

3. 取得跟当前view相关联的图形上下文

4. 拼接路径，绘制相应的图形内容

- 把路径添加到上下文

- 设置绘图状态

- 利用图形上下文将绘制的所有内容渲染显示到view上面


##Graphics Context
详见“Graphics Context”


##-(void)drawRect:(CGRect)rect方法
详见“drawRect”


##常用函数（CG是C语言，没有方法，只有函数）
```objc
##常用拼接路径函数
新建一个起点
void CGContextMoveToPoint(CGContextRef c, CGFloat x, CGFloat y)

添加新的线段到某个点
void CGContextAddLineToPoint(CGContextRef c, CGFloat x, CGFloat y)

添加一个矩形
void CGContextAddRect(CGContextRef c, CGRect rect)

添加一个椭圆
void CGContextAddEllipseInRect(CGContextRef context, CGRect rect)

添加一个圆弧
void CGContextAddArc(CGContextRef c, CGFloat x, CGFloat y,
  CGFloat radius, CGFloat startAngle, CGFloat endAngle, int clockwise)


##常用绘制路径函数
//Mode参数决定绘制的模式
void CGContextDrawPath(CGContextRef c, CGPathDrawingMode mode)

//绘制空心路径
void CGContextStrokePath(CGContextRef c)

//绘制实心路径
void CGContextFillPath(CGContextRef c)

//提示：一般以CGContextDraw、CGContextStroke、CGContextFill开头的函数，都是用来绘制路径的


##图形上下文栈的操作
//将当前的上下文copy一份,保存到栈顶(那个栈叫做”图形上下文栈”)
void CGContextSaveGState(CGContextRef c)

//将栈顶的上下文出栈,替换掉当前的上下文
void CGContextRestoreGState(CGContextRef c)


##矩阵操作
//利用矩阵操作，能让绘制到上下文中的所有路径一起发生变化
缩放
void CGContextScaleCTM(CGContextRef c, CGFloat sx, CGFloat sy)

//旋转
void CGContextRotateCTM(CGContextRef c, CGFloat angle)

//平移
void CGContextTranslateCTM(CGContextRef c, CGFloat tx, CGFloat ty)
```

### Quartz2D的内存管理
1. 使用含有“Create”或“Copy”的函数创建的对象，使用完后必须释放，否则将导致内存泄露

- 使用不含有“Create”或“Copy”的函数获取的对象，则不需要释放

- 如果retain了一个对象，不再使用时，需要将其release掉

- 可以使用Quartz 2D的函数来指定retain和release一个对象。
    - 例如，如果创建了一个CGColorSpace对象，则使用函数CGColorSpaceRetain和CGColorSpaceRelease来retain和release对象。

- 也可以使用Core Foundation的CFRetain和CFRelease。
    - 注意不能传递NULL值给这些函数

### 水印
详见“drawWaterMark(水印）”
```objc
核心代码
开启一个基于位图的图形上下文
void     UIGraphicsBeginImageContextWithOptions(CGSize size, BOOL opaque, CGFloat scale)

从上下文中取得图片（UIImage）
UIImage* UIGraphicsGetImageFromCurrentImageContext();

结束基于位图的图形上下文
void     UIGraphicsEndImageContext();
```

###图片裁剪
- app的头像很多都是圆形的,所以需要把一张普通的图片刻意裁剪成圆形
详见“drawCircleBorderImage(圆环图片）”
```objc
核心代码
void CGContextClip(CGContextRef c)
将当前上下所绘制的路径裁剪出来（超出这个裁剪区域的都不能显示）
```

##屏幕截图
- 有时候需要截取屏幕上的某一块内容，比如捕鱼达人游戏
详见“printscreen”
```objc
//核心代码
//调用某个view的layer的renderInContext
- (void)renderInContext:(CGContextRef)ctx;
```


##paths
[](file:///Users/imac/Desktop/IOS/小码IOS/UI进阶/UI进阶预习视频/资料/day06Quartz2D/PPT/Paths.webarchive)



###备忘笔记





