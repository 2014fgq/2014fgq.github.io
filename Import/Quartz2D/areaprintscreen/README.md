# areaPrintScreen(局部截屏）

## 局部截屏的步骤
- 设置控件并添加滚动手势

- 根据`手势计算截屏区域`

- 懒加载蒙板，并添加到`view(不是imageView)`中，设置`蒙板frame是截图区域`

- `手势结束时`，开启位图上下文（与控件大小一致）

- 获取当前位图上下文

- 将控件的的layer通过renderInContex绘制到位图上下文中

- 从上下文中获取这张图片

- 关闭上下文

- 显示裁剪后的图片

```objc
@interface ViewController ()
//图片控件
@property (weak, nonatomic) IBOutlet UIImageView *imageView;
//起始点
@property (nonatomic, assign) CGPoint startP;
//选择区域的蒙板
@property (nonatomic, weak) UIView *cover;
@end

@implementation ViewController
//懒加载蒙板
- (UIView *)cover
{
    if (_cover == nil) {
        UIView *cover = [[UIView alloc] init];
        cover.backgroundColor = [UIColor blackColor];
        cover.alpha = 0.6;
        //将蒙板加入到主视图而不是图片控件中，是不想让蒙板加入到位图上下文，影响图片的处理
        [self.view addSubview:cover];
        _cover = cover;
    }
    return _cover;
}

## 计算裁剪区域
// 当手指在UIImageView上移动的时候调用
- (IBAction)pan:(UIPanGestureRecognizer *)sender {

    // 获取当前触摸点
    CGPoint curP = [sender locationInView:_imageView];
    if (sender.state == UIGestureRecognizerStateBegan) { // 开始拖动的时候调用
        // translationInView:获取手指的偏移量
        // locationInView:获取手指触摸点

        // 记录起始点
        _startP = curP;
    }

    // 计算裁剪区域
    CGFloat clipW = curP.x - _startP.x;
    CGFloat clipH = curP.y - _startP.y;
    CGRect clipR = CGRectMake(_startP.x, _startP.y, clipW, clipH);

## 设置灰色蒙版尺寸
    self.cover.frame = clipR;

    if (sender.state == UIGestureRecognizerStateEnded) { // 手指抬起时，裁剪图片

## 开启位图上下文
        UIGraphicsBeginImageContextWithOptions(_imageView.bounds.size, NO, 0);

## 设置裁剪区域
        UIBezierPath *path = [UIBezierPath bezierPathWithRect:clipR];
        [path addClip];

## 将图片控件的图层渲染到上下文中
        [_imageView.layer renderInContext:UIGraphicsGetCurrentContext()];

## 从上下文中获取图片
       UIImage *image =  UIGraphicsGetImageFromCurrentImageContext();

## 关闭上下文
        UIGraphicsEndImageContext();

## 重新显示图片
        _imageView.image = image;

## 移除蒙板
        // 若蒙板是加入到view而不是imageView中，则只要移除即可，若加入到imageView中，需在绘制到上下文前移除
        [self.cover removeFromSuperview];

    }
}
```
