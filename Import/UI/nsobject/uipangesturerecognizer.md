# UIPanGestureRecognizer（拖动）
- 父类是UIGestureRecognizer


// 拖拽
- (void)setUpPan
{
    // 拖拽
    UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(pan:)];

    [self.imageView addGestureRecognizer:pan];

}

- (void)pan:(UIPanGestureRecognizer *)pan
{
    // 返回的是相对于最原始的手指的偏移量

    CGPoint transP = [pan translationInView:self.imageView];

    // 移动图片控件
    self.imageView.transform = CGAffineTransformTranslate(self.imageView.transform, transP.x, transP.y);

    // 复位,表示相对上一次
    [pan setTranslation:CGPointZero inView:self.imageView];
}
