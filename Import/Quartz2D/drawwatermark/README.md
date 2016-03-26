# drawWaterMark(水印）

##图片水印
1. 水印：在图片上加的防止他人盗图的半透明logo、文字、图标
- 作用:主要是一些网站为了版权问题、广告而添加的

- 手机客户端app中也需要用到水印技术,比如，用户拍完照片后，可以在照片上打个水印，标识这个图片是属于哪个用户的;利用Quartz2D，将水印（文字、LOGO）画到图片的右下角


## 水印图片实现步骤
1. 不需要自定义view,只要画的东西,不显示到view就不需要自定义view

- 不一定要实现-(void)drawRect:(CGRect)rect方法（只有layer上下文才一定要在该方法中实现）

- 加载图片

- 开启位图上下文

- 绘制图片到位图上下文

- 在上下文中添加水印

- 从上下文中获取这张图片

- 关闭上下文

- 保存图片

```objc
## 加载图片
    UIImage *image = [UIImage imageNamed:@"小黄人"];

## 开启位图上下文，这种方法得到的图片最清晰。
    // size:位图上下文,一般根图片一样大
    // opaque:不透明度,只要跟上下文相关的都是叫不透明度,根view相关的叫透明度
    // opaque:YES 不透明 NO:透明,一般都是使用透明的上下文
    // scale: 0 表示不需要缩放
    UIGraphicsBeginImageContextWithOptions(image.size, NO, 0);

## 绘制图片到位图上下文
    [image drawAtPoint:CGPointZero];

## 添加水印
    NSString *str = @"小码哥";
    [str drawAtPoint:CGPointZero withAttributes:@{NSForegroundColorAttributeName : [UIColor redColor]}];

## 从上下文中获取图片
    image = UIGraphicsGetImageFromCurrentImageContext();

## 关闭上下文，否则一致占用内存
    UIGraphicsEndImageContext();

## 保存图片
    // 把图片转换成二进制数据，png格式，质量最高清
    NSData *data = UIImagePNGRepresentation(image);

    // 写入桌面
    [data writeToFile:@"/Users/xiaomage/Desktop/1.png" atomically:YES];
```
