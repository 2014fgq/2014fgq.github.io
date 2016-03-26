# clearImage

## 图片擦除实现步骤
- 加载图片，并添加拖动手势；另外加载一张背景图片，用于图片被擦除后显示，置于擦除图片下方

- 开启位图上下文（与图片大小一致）

- 获取当前位图上下文

- 将图片控件的layer通过renderInContex绘制到位图上下文中

- 清除上下文中某一部分

- 从上下文中获取这张图片

- 关闭上下文

- 将擦除后的图片显示回控件中

```objc
## 加载图片，并添加拖动手势；另外加载一张背景图片，用于图片被擦除后显示，置于擦除图片下方，已通过storyboard实现

//监视拖动
- (IBAction)pan:(UIPanGestureRecognizer *)sender {

## 开启位图上下文
    UIGraphicsBeginImageContextWithOptions(sender.view.bounds.size, NO, 0);

## 获取当前位图上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

## 渲染控件，相当于控件的layer图层加载到位图上下文中
    [sender.view.layer renderInContext:ctx];

##  清除上下文中某一部分
    //获取当前触摸点
    CGPoint curP =[sender locationInView:sender.view];

    //计算擦除区域
    CGFloat wh = 30;
    CGFloat x = curP.x - wh * 0.5;
    CGFloat y = curP.y - wh * 0.5;
    CGRect clearR = CGRectMake(x, y, wh, wh);

    ### 核心代码
    CGContextClearRect(ctx, clearR);

## 从上下文中生成一张图片
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();

## 关闭上下文
    UIGraphicsEndImageContext();

## 将擦除后的图片显示回控件中
    UIImageView *imageV = (UIImageView *)sender.view;
    imageV.image = image;

}
```
