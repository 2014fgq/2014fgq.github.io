# drawProgress(进度条）

### 重绘-下载进度条(setNeedsDisplay)的步骤
1. 布局控件：UISliderView,自定义view用来画圆弧进度条,UILabel

- 功能一：UILabel的text值与UISliderView相关，监听UISliderView的进度，显示text值

- 功能二：圆弧进度条的长度与与UISliderView相关，监听UISliderView的进度，显示圆弧进度条
    1. 绘制圆弧

    - 根据新进度值重新绘制圆弧


###在progressView.m中
```objc
- (void)setProgress:(CGFloat)progress
{
    _progress = progress;
    // 每次获取新进度后重绘
    [self setNeedsDisplay];
}

- (void)drawRect:(CGRect)rect {

    // Drawing code
    CGPoint center = CGPointMake(rect.size.width * 0.5, rect.size.height * 0.5);

    //- M_PI_2表示从12点钟位置开始绘制
    CGFloat endA = - M_PI_2 + M_PI * 2 * _progress;

    // 圆弧
    UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:rect.size.width * 0.5 - 4 startAngle:-M_PI_2 endAngle:endA clockwise:YES];

    [path stroke];

}
```

###在vc.m中
```objc
@interface ViewController ()
@property (weak, nonatomic) IBOutlet UILabel *labelView;
@property (weak, nonatomic) IBOutlet ProgressView *progressView;

@end

@implementation ViewController
//监控进度条UISliderView
- (IBAction)valueChange:(UISlider *)sender {
    // 在@“”中转义字符%% = %
    _labelView.text = [NSString stringWithFormat:@"%.2f%%",sender.value * 100];

    // 传入下载进度
    _progressView.progress = sender.value;
}
```
