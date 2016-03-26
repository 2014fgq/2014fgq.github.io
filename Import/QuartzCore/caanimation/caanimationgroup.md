# CAAnimationGroup
- 父类是CAAnimation

###CAAnimationGroup——动画组
- 动画组，是CAAnimation的子类，可以保存一组动画对象，将CAAnimationGroup对象加入层后，组中所有动画对象可以同时并发运行
- 属性说明：
    - animations：用来保存一组动画对象的NSArray
- 默认情况下，一组动画对象是同时运行的，也可以通过设置动画对象的beginTime属性来更改动画的开始时间


```objc
// 动画组
    CAAnimationGroup *group = [CAAnimationGroup animation];

    // 平移
    CABasicAnimation *anim = [CABasicAnimation animation];
    anim.keyPath = @"position";
    anim.toValue = [NSValue valueWithCGPoint:CGPointMake(arc4random_uniform(300), arc4random_uniform(500))];

    // 缩放
    CABasicAnimation *anim1 = [CABasicAnimation animation];
    anim1.keyPath = @"transform.scale";
    anim1.toValue = @0.5;

    // 旋转
    CABasicAnimation *anim2 = [CABasicAnimation animation];
    anim2.keyPath = @"transform.rotation";
    anim2.toValue = @(M_PI);

    // 设置动画时长
    group.duration = 2;
    group.removedOnCompletion = NO;
    group.fillMode = kCAFillModeForwards;

    // 给动画组添加动画
    group.animations = @[anim,anim1,anim2];

    [_blueView.layer addAnimation:group forKey:nil];

```
