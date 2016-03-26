# UIRotationGestureRecognizer（旋转）
- 父类是UIGestureRecognizer
-



// 旋转
- (void)setUpRotation
{
    // 旋转
    UIRotationGestureRecognizer *rotation = [[UIRotationGestureRecognizer alloc] initWithTarget:self action:@selector(rotation:)];

    rotation.delegate = self;

    [self.imageView addGestureRecognizer:rotation];
}

- (void)rotation:(UIRotationGestureRecognizer *)rotation
{
    NSLog(@"%f",rotation.rotation);

    self.imageView.transform = CGAffineTransformRotate(self.imageView.transform, rotation.rotation);
    // 复位
    rotation.rotation = 0;
}
