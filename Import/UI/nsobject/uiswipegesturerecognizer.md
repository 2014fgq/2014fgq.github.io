# UISwipeGestureRecognizer（轻扫）
- 父类是UIGestureRecognizer
- 默认轻扫方向是向右轻扫

```objc
swipeD.direction = UISwipeGestureRecognizerDirectionDown;
```


// 轻扫
- (void)setUpSwipe
{
    // 轻扫
    // 一个手势只能支持一个方向
    UISwipeGestureRecognizer *swipe = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipe:)];
    // 轻扫方向:默认是右边
    swipe.direction = UISwipeGestureRecognizerDirectionUp;

    [self.imageView addGestureRecognizer:swipe];

    UISwipeGestureRecognizer *swipeD = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipe:)];
    // 轻扫方向:下边
    swipeD.direction = UISwipeGestureRecognizerDirectionDown;

    [self.imageView addGestureRecognizer:swipeD];

}

- (void)swipe:(UISwipeGestureRecognizer *)swipe
{
    if (swipe.direction == UISwipeGestureRecognizerDirectionDown) {
        NSLog(@"下");
    }else{

    }

}
