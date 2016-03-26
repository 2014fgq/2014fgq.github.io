# drawShape(图形）
- 在(void)drawRect:(CGRect)rect方法中实现

### 绘制图形步骤
1. 新建一个类，继承自UIView（略）

2. 在-(void)drawRect:(CGRect)rect方法实现下述几步（略）

3. 取得跟当前view相关联的图形上下文

4. 拼接路径，绘制相应的图形内容

- 把路径添加到上下文

- 设置绘图状态

- 利用图形上下文将绘制的所有内容渲染显示到view上面

### 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

### 拼接路径
    // 1/4扇形
    CGPoint center = CGPointMake(rect.size.width * 0.5, rect.size.height * 0.5);//设置圆心
    CGFloat radius = rect.size.width * 0.5 * 0.5;//设置半径
    UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:50 startAngle:0 endAngle:M_PI_2 clockwise:YES];

    // 添加一根线到圆心
    [path addLineToPoint:center];

    // 关闭路径:从路径的终点,连接到起点
    [path closePath];//最简单的实现方式
//    [path addLineToPoint:CGPointMake(center.x + 50, center.y)];//第二种实现方式

### 把路径添加到上下文
    CGContextAddPath(ctx, path.CGPath);

### 设置绘图状态
    //设置线宽
    CGContextSetLineWidth(ctx, 5);
    //设置填充颜色
    [[UIColor redColor] setFill];
    // 设置描边颜色
    [[UIColor greenColor] setStroke];

### 渲染
    //贝塞尔路径没有封装即描边又填充的方法
    // 既要填充又要描边，只能使用CG函数
    CGContextDrawPath(ctx, kCGPathFillStroke);
}


## 其他图形画法
```objc
    // 圆形
    // 为什么传入矩形，因为圆内切与矩形
//   UIBezierPath *path = [UIBezierPath bezierPathWithRoundedRect:CGRectMake(10, 10, 200, 200) cornerRadius:100];
//
//    [path stroke];
    // 椭圆
    //CGRectMake(10, 10, 200, 200)中，10，10不是代表圆心，真正的圆心系统会自动算出来，200/2才是半径
//    UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(10, 10, 200, 200)];
//    [path stroke];

    // 圆弧
    // center:圆心
    // radius:半径
    // startAngle:起始角度
    // endAngle:结束角度
    // clockwise: YES:顺时针 NO:逆时针
//   UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:radius startAngle:0 endAngle:M_PI clockwise:YES];
//    [path stroke];
```
