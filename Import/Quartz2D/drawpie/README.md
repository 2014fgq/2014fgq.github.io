# drawPie(饼图）

### 绘制饼图步骤
1. 新建一个类，继承自UIView（略）

2. 在-(void)drawRect:(CGRect)rect方法实现下述几步

3. 取得跟当前view相关联的图形上下文（贝塞尔路径不用）

4. 拼接路径，绘制相应的图形内容
    - 根据饼图大小分比例，写入数组中
    - 根据比例大小，设置圆弧旋转角度（即将360分比例），遍历数组画出所有圆弧；存在规律startA = endA； angle = 比例 * 总度数360°； endA = startA + angle；
    - 每次画出的圆弧都要将下面3步完成，即每画一个圆弧就要渲染。

- 把路径添加到上下文（贝塞尔路径不用）

- 设置绘图状态

- 利用图形上下文将绘制的所有内容渲染显示到view上面

```objc
- (void)drawRect:(CGRect)rect {
    // Drawing code

    NSArray *data = @[@25,@25,@50];

    // 圆心
   CGPoint center = CGPointMake(rect.size.width * 0.5, rect.size.height * 0.5);
    CGFloat startA = 0;
    CGFloat endA = 0;
    CGFloat angle = 0;

##拼接路径
    for (NSNumber *num in data) {
        startA = endA;
        //根据占比绘制圆弧，最终凑成一个圆
        angle = [num integerValue] / 100.0 * M_PI * 2;
        endA = startA + angle;
// 画第一个扇形
    UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:100 startAngle:startA endAngle:endA clockwise:YES];

// 连接一根线到圆心
    [path addLineToPoint:center];

## 设置绘图状态
    [[self randomColor] set];

## 渲染
    [path fill];
    }
}

// 封装随机颜色
- (UIColor *)randomColor
{
    CGFloat r = arc4random_uniform(256) / 255.0;
    CGFloat g = arc4random_uniform(256) / 255.0;
    CGFloat b = arc4random_uniform(256) / 255.0;

    return [UIColor colorWithRed:r green:g blue:b alpha:1];
}

//点击重绘（不必要）
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    [self setNeedsDisplay];
}
```
