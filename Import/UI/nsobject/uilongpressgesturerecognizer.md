# UILongPressGestureRecognizer（长按）
- 父类是UIGestureRecognizer
- 一般开发中,长按操作只会做一次
- 假设在一开始长按的时候就做一次操作
- 默认调用两次

- (void)setUpLongPress
{
    // 长按
    UILongPressGestureRecognizer *longPress = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longPress:)];

    [self.imageView addGestureRecognizer:longPress];
}

// 长按图片的时候就会触发长按手势
- (void)longPress:(UILongPressGestureRecognizer *)longPress
{
    // 一般开发中,长按操作只会做一次
    // 假设在一开始长按的时候就做一次操作

    if (longPress.state == UIGestureRecognizerStateBegan) {

        NSLog(@"%ld",longPress.state);
    }


}
