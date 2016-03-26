# drawSnow(动画图片-计时器画雪花）

## 通过UIImage自身方法绘制图片步骤
1. 新建一个类，继承自UIView（略）

2. 创建定时器，定时调用`setNeedsDisplay`

3. 在-(void)drawRect:(CGRect)rect方法实现下述几步

- 加载图片

- 通过UIImage自身方法绘制图片到view上面

- 设置变化

```objc
- (void)awakeFromNib
{
## 创建定时器
// 如果每隔一段时间重绘,一般不使用NSTimer（因为不够及时）// 每次屏幕自动刷新60次/秒,每次刷新的时候只会把当前需要刷新的view重绘（即绑定了刷新标识的view）
// setNeedsDisplay:重绘,实质是给当前控件绑定一个刷新的标识,在下一次屏幕刷新就会重绘

    CADisplayLink *link = [CADisplayLink displayLinkWithTarget:self selector:@selector(setNeedsDisplay)];

    // 添加到主运行循环,才会触发这个定时器
    [link addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSDefaultRunLoopMode];

    // 每次刷新的时候调用,每次刷新就给当前view绑定一个刷新标示,表示每次刷新的时候都需要重新绘制当前view

}

- (void)drawRect:(CGRect)rect {

    static CGFloat snowY = 0;
## 加载图片
    UIImage *image = [UIImage imageNamed:@"雪花"];
## 绘制图片
    [image drawAtPoint:CGPointMake(50, snowY)];

## 设置变化
    snowY += 10;

    if (snowY > rect.size.height) {
        snowY = 0;
    }
}
```
