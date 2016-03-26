# UIResponder
- 父类是NSObject

##UIView的触摸事件处理（谁是响应者，谁调用）
```objc
//手指开始触摸view时，系统自动调用
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event

//手指在view上移动，系统自动调用（随着手指的移动，会持续调用该方法）
- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event
{
    ##拖动位移效果
    // 拿到UITouch就能获取当前点
    UITouch *touch = [touches anyObject];

    // 获取当前点
    CGPoint curP = [touch locationInView:self];

    // 获取上一个点
    CGPoint preP = [touch previousLocationInView:self];

    // 获取手指x轴偏移量
    CGFloat offsetX = curP.x - preP.x;

    // 获取手指y轴偏移量
    CGFloat offsetY = curP.y - preP.y;

    // 移动当前view
    self.transform = CGAffineTransformTranslate(self.transform, offsetX, offsetY);
}

//手指离开view，系统自动调用
- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event

//触摸结束前，某个系统事件(例如电话呼入)会打断触摸过程，系统自动调用
- (void)touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event
//提示：touches中存放的都是UITouch对象
```

### UIView的触摸系列方法touches和event参数
- 一次完整的触摸过程，会经历3个状态：
    1. 触摸开始：
    `- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event`

    - 触摸移动：
    `- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event`

    - 触摸结束：
    `- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event`

    - 触摸取消（可能会经历）：
    `- (void)touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event`

- 4个触摸事件处理方法中，都有NSSet *touches和UIEvent *event两个参数

    - event参数：一次完整的触摸过程中，只会产生一个事件对象，4个触摸方法都是同一个event参数

    - touches参数（根据touches中UITouch的个数可以判断出是单点触摸还是多点触摸）：

         1. 如果两根手指同时触摸一个view，那么view只会调用一次touchesBegan:withEvent:方法，touches参数中装着2个UITouch对象

         - 如果这两根手指一前一后分开触摸同一个view，那么view会分别调用2次touchesBegan:withEvent:方法，并且每次调用时的touches参数中只包含一个UITouch对象



##UIView的加速计事件处理
```objc
- (void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event;
- (void)motionEnded:(UIEventSubtype)motion withEvent:(UIEvent *)event;
- (void)motionCancelled:(UIEventSubtype)motion withEvent:(UIEvent *)event;
```

##UIView的远程控制事件处理
```objc
- (void)remoteControlReceivedWithEvent:(UIEvent *)event;
```
