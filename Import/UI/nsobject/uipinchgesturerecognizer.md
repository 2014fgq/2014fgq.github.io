# UIPinchGestureRecognizer（捏合）
- 父类是UIGestureRecognizer


- (void)setUpPinch
{
    // 捏合
    UIPinchGestureRecognizer *pinch = [[UIPinchGestureRecognizer alloc] initWithTarget:self action:@selector(pinch:)];
    pinch.delegate = self;


    [self.imageView addGestureRecognizer:pinch];
}

- (void)pinch:(UIPinchGestureRecognizer *)pinch
{
    NSLog(@"%f",pinch.scale);
    self.imageView.transform = CGAffineTransformScale(self.imageView.transform, pinch.scale, pinch.scale);

    // 复位
    pinch.scale = 1;

}
