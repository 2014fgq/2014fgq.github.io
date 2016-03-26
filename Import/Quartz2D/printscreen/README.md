# printscreen

## 点击实现将控件截图的步骤
- 实现点击方法

- 开启位图上下文（与控件大小一致）

- 获取当前位图上下文

- 将控件的的layer通过renderInContex绘制到位图上下文中

- 从上下文中获取这张图片

- 关闭上下文

- 保存图片

```objc
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
## 开启位图上下文
    UIGraphicsBeginImageContextWithOptions(self.view.bounds.size, NO, 0);

## 获取当前上下文==>位图上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

## 获取view图层
    CALayer *vcLayer = self.view.layer;

## 把控件图层渲染到位图上下文中,图层只能渲染,不能绘制
    [vcLayer renderInContext:ctx];
//    [vcLayer drawInContext:ctx];

## 从上下文中取出图片
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();

## 关闭上下文
    UIGraphicsEndImageContext();

## 保存图片
    // Quality:图片质量
    //jpeg取得图片质量不一定是最好的，设置数值越少质量越差；png的是最好的。上传服务器时最好用jpeg压缩一下，服务器压力没那么大
   NSData *data = UIImageJPEGRepresentation(image, 0.0000001);

    [data writeToFile:@"/Users/xiaomage/Desktop/view.jpg" atomically:YES];
}
```
