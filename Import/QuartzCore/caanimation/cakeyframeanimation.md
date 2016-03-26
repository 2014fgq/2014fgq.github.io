# CAKeyframeAnimation
- 父类是CAPropertyAnimation


###CAKeyframeAnimation——关键帧动画
- 是CAPropertyAnimation的子类，与CABasicAnimation的区别是：
    - CABasicAnimation只能从一个数值（fromValue）变到另一个数值（toValue），而CAKeyframeAnimation会使用一个NSArray保存这些数值

- 属性说明：
    - values：上述的NSArray对象。里面的元素称为“关键帧”(keyframe)。动画对象会在指定的时间（duration）内，依次显示values数组中的每一个关键帧
    - path：可以设置一个CGPathRef、CGMutablePathRef，让图层按照路径轨迹移动。path只对CALayer的anchorPoint和position起作用。如果设置了path，那么values将被忽略
    - keyTimes：可以为对应的关键帧指定对应的时间点，其取值范围为0到1.0，keyTimes中的每一个时间值都对应values中的每一帧。如果没有设置keyTimes，各个关键帧的时间是平分的

- CABasicAnimation可看做是只有2个关键帧的CAKeyframeAnimation


### 图案绕圆运行
```objc
    // 创建帧动画
    CAKeyframeAnimation *anim = [CAKeyframeAnimation animation];

    anim.keyPath = @"position";

    // 创建路径
    UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:_imageView.center radius:100 startAngle:0 endAngle:M_PI * 2 clockwise:YES];

    anim.path = path.CGPath;

    anim.repeatCount = MAXFLOAT;
    
    //去掉停顿效果（计算会有停顿）
    anim.calaulationMode = @"cubicpaced";
    
    //自动旋转
    anim.rotationMode = "autoReverse";

    anim.duration = 1;

    [_imageView.layer addAnimation:anim forKey:nil];
```

### 图形抖动
```objc
#####define angle2Radion(angle) (angle / 180.0 * M_PI)
// 帧动画
    CAKeyframeAnimation *anim = [CAKeyframeAnimation animation];

    // 描述修改layer的属性,transform.rotation只能二维旋转
    anim.keyPath = @"transform.rotation";

    // 修改Layer值
    anim.values = @[@(angle2Radion(-5)),@(angle2Radion(5)),@(angle2Radion(-5))];

    anim.duration = 2;

    // 动画执行次数
    anim.repeatCount = MAXFLOAT;

    [_imageView.layer addAnimation:anim forKey:nil];
```
