# CGContentState-CGContentOperate(图形上下文状态栈与矩阵操作）

##矩阵操作（关注核心代码即可）
```objc
## 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

## 核心代码——给上下文做形变（在渲染前操作，对图形上下文进行操作，而不是对绘制的图形进行操作）

    // 平移
    CGContextTranslateCTM(ctx, 100, 100);

    // 旋转
    CGContextRotateCTM(ctx, M_PI_4);

    // 缩放
    CGContextScaleCTM(ctx, 0.5, 0.5);

##拼接路径
    UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(-50, -100, 100, 200)];

##设置绘图状态
    [[UIColor redColor] set];

##渲染
    [path fill];
```

##图形上下文状态栈（关注核心代码即可）
```objc
    // 1.获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 2.拼接路径——画第一根线
    UIBezierPath *path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(10, 125)];
    [path addLineToPoint:CGPointMake(240, 125)];

    // 3.把路径添加到上下文
    CGContextAddPath(ctx, path.CGPath);

#核心代码—— 保存以后会用到的状态（在修改状态前）
    CGContextSaveGState(ctx);

    // 设置上下文的状态
    // 颜色
    [[UIColor redColor] set];
    // 设置线宽
    CGContextSetLineWidth(ctx, 5);
    CGContextSetLineCap(ctx, kCGLineCapRound);

    // 4.渲染路径,在渲染之前会查看下当前上下文的状态,根据状态去渲染
    CGContextStrokePath(ctx);

    // 描述第二根线
    path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(125, 10)];
    [path addLineToPoint:CGPointMake(125, 240)];

    // 添加到上下文
    CGContextAddPath(ctx, path.CGPath);

#核心代码—— 还原状态,从之前的栈里面取出状态替换掉当前状态
    CGContextRestoreGState(ctx);
//    相当于将上面的设置上下文状态的改为下面三句代码：
//    [[UIColor blackColor] set];
//    CGContextSetLineWidth(ctx, 1);
//    CGContextSetLineCap(ctx, kCGLineCapSquare);

    // 渲染第二根线
    CGContextStrokePath(ctx);
```
