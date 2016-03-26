# drawCircleBorderImage(圆环图片）

## 图片裁剪实现步骤
- 加载图片

- 开启位图上下文（与图片大小一致）

- 绘制大圆，填充颜色（未来成为圆环）

- 设置裁剪区域

- 绘制需裁剪的图片到位图上下文

- 从上下文中获取这张图片

- 关闭上下文

- 利用图片

```objc
## 加载图片
    UIImage *image = [UIImage imageNamed:imageName];

## 开启位图上下文
    UIGraphicsBeginImageContextWithOptions(ctxRect.size, NO, 0);

##  绘制大圆
    //计算圆宽
    CGFloat borderWH = borderW;
    CGFloat ctxWH = image.size.width + 2 * borderWH;
    CGRect ctxRect = CGRectMake(0, 0, ctxWH, ctxWH);

    // 画大圆
    UIBezierPath *bigCirclePath = [UIBezierPath bezierPathWithOvalInRect:ctxRect];

    // 设置圆环的颜色并填充
    [circleColor set];
    [bigCirclePath fill];

##  设置裁剪区域
    CGRect clipRect = CGRectMake(borderWH, borderWH, image.size.width, image.size.height);
    UIBezierPath *clipPath = [UIBezierPath bezierPathWithOvalInRect:clipRect];
    [clipPath addClip];

## 绘制图片到位图上下文
    [image drawAtPoint:CGPointMake(borderWH, borderWH)];

## 从上下文中获取图片
    image = UIGraphicsGetImageFromCurrentImageContext();

## 关闭上下文
    UIGraphicsEndImageContext();

## 利用图片
    return image;
```
