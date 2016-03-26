# CAAnimation
- 父类是NSObject

###CAAnimation常用属性
```objc
duration：动画的持续时间
repeatCount：重复次数，无限循环可以设置HUGE_VALF或者MAXFLOAT
repeatDuration：重复时间
removedOnCompletion：默认为YES，代表动画执行完毕后就从图层上移除，图形会恢复到动画执行前的状态。如果想让图层保持显示动画执行后的状态，那就设置为NO，不过还要设置fillMode为kCAFillModeForwards
fillMode：决定当前对象在非active时间段的行为。比如动画开始之前或者动画结束之后
beginTime：可以用来设置动画延迟执行时间，若想延迟2s，就设置为CACurrentMediaTime()+2，CACurrentMediaTime()为图层的当前时间
timingFunction：速度控制函数，控制动画运行的节奏
delegate：动画代理(不需要遵守协议！）
```


###fillMode——动画填充模式
- 若想fillMode有效，最好设置removedOnCompletion = NO

```objc
kCAFillModeRemoved //这个是默认值，也就是说当动画开始前和动画结束后，动画对layer都没有影响，动画结束后，layer会恢复到之前的状态

kCAFillModeForwards //当动画结束后，layer会一直保持着动画最后的状态

kCAFillModeBackwards //在动画开始前，只需要将动画加入了一个layer，layer便立即进入动画的初始状态并等待动画开始。

kCAFillModeBoth //这个其实就是上面两个的合成.动画加入后开始之前，layer便处于动画初始状态，动画结束后layer保持动画最后的状态
```

###CAMediaTimingFunction——速度控制函数
```objc
kCAMediaTimingFunctionLinear（线性）：匀速，给你一个相对静态的感觉
kCAMediaTimingFunctionEaseIn（渐进）：动画缓慢进入，然后加速离开
kCAMediaTimingFunctionEaseOut（渐出）：动画全速进入，然后减速的到达目的地
kCAMediaTimingFunctionEaseInEaseOut（渐进渐出）：动画缓慢的进入，中间加速，然后减速的到达目的地。这个是默认的动画行
```


###CAAnimation——动画代理方法
CAAnimation在分类中定义了代理方法
```objc
@interface NSObject (CAAnimationDelegate)

/* Called when the animation begins its active duration. */

- (void)animationDidStart:(CAAnimation *)anim;

/* Called when the animation either completes its active duration or
 * is removed from the object it is attached to (i.e. the layer). 'flag'
 * is true if the animation reached the end of its active duration
 * without being removed. */

- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag;
@end
```


###CALayer上动画的暂停和恢复
#pragma mark 暂停CALayer的动画
```objc
-(void)pauseLayer:(CALayer*)layer
{
    CFTimeInterval pausedTime = [layer convertTime:CACurrentMediaTime() fromLayer:nil];

    // 让CALayer的时间停止走动
      layer.speed = 0.0;
    // 让CALayer的时间停留在pausedTime这个时刻
    layer.timeOffset = pausedTime;
}
```

####CALayer上动画的恢复
#pragma mark 恢复CALayer的动画
```objc
-(void)resumeLayer:(CALayer*)layer
{
    CFTimeInterval pausedTime = layer.timeOffset;
    // 1. 让CALayer的时间继续行走
      layer.speed = 1.0;
    // 2. 取消上次记录的停留时刻
      layer.timeOffset = 0.0;
    // 3. 取消上次设置的时间
      layer.beginTime = 0.0;
    // 4. 计算暂停的时间(这里也可以用CACurrentMediaTime()-pausedTime)
    CFTimeInterval timeSincePause = [layer convertTime:CACurrentMediaTime() fromLayer:nil] - pausedTime;
    // 5. 设置相对于父坐标系的开始时间(往后退timeSincePause)
      layer.beginTime = timeSincePause;
}
```
