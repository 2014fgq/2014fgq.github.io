# CABasicAnimation
- 父类是CAPropertyAnimation


###CABasicAnimation——基本动画
- 基本动画，是CAPropertyAnimation的子类

- 属性说明:
    - fromValue：keyPath相应属性的初始值
    - toValue：keyPath相应属性的结束值

- 动画过程说明：
    - 随着动画的进行，在长度为duration的持续时间内，keyPath相应属性的值从fromValue渐渐地变为toValue
    - keyPath内容是CALayer的可动画Animatable属性
    - 如果fillMode=kCAFillModeForwards同时removedOnComletion=NO，那么在动画执行完毕后，图层会保持显示动画执行后的状态。但在实质上，图层的属性值还是动画执行前的初始值，并没有真正被改变。

###形变
```objc
    // 1.创建核心动画
    CABasicAnimation *anim = [CABasicAnimation animation];

    // 2.描述修改Layer哪个属性
    anim.keyPath = keyPath(_redView.layer, position);

    // 3.描述修改layer属性的值
    // 动画的起点
    //    anim.fromValue =
    // 动画的终点
    anim.toValue = [NSValue valueWithCGPoint:CGPointMake(300, 400)];

    // 动画时长
    anim.duration = 1;

    // 取消反弹
    // 1.在动画完成的时候不要给我把动画销毁
    anim.removedOnCompletion = NO;

    // 2.动画永远保持最新的状态
    anim.fillMode = kCAFillModeForwards;


    // 添加核心动画
    [_redView.layer addAnimation:anim forKey:nil];
```


### 缩放,心脏跳动
```objc
    // 1.创建动画对象
    CABasicAnimation *anim = [CABasicAnimation animation];

    // 2.描述修改layer的属性
    anim.keyPath = keyPath(_imageView.layer, transform);

    // 3.修改layer的值
    anim.toValue = [NSValue valueWithCATransform3D:CATransform3DMakeScale(0.5, 0.5, 1)];

    // 设置动画执行次数
    anim.repeatCount = MAXFLOAT;
    
    //自动翻转
    anim.autoreveres = YES;

    // 4.添加到图层
    [_imageView.layer addAnimation:anim forKey:nil];
```
