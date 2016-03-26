# drawImage(图片）

## 通过UIImage自身方法绘制图片步骤
1. 新建一个类，继承自UIView（略）

2. 在-(void)drawRect:(CGRect)rect方法实现下述几步（略）

- 加载图片

- 设置裁剪区域

- 通过UIImage自身方法绘制图片到view上面

```objc
- (void)drawRect:(CGRect)rect {
## 加载图片
    UIImage *image = [UIImage imageNamed:@"阿狸头像"];

##设置裁剪区域
    // 设置裁剪区域的代码必须要在绘图之前
    // 设置裁剪区域,超出裁剪区域的部分会被裁剪掉
    UIRectClip(CGRectMake(0, 0, 150, 150));

## 绘图（UIImage的方法）
    // 绘图的时候,绘制的内容不能超过当前控件
    // Pattern:平铺
    //  [image drawAsPatternInRect:rect];

    // drawAtPoint:默认绘制的图片跟图片本身一样
    //  [image drawAtPoint:CGPointZero];

    // drawInRect:默认绘制的图片跟控件一样,拉伸跟控件一样大
    [image drawInRect:rect];

## 填充（不必要）
    // 快速的填充一个矩形
    UIRectFill(CGRectMake(0, 0, 50, 50));
}
```


### UIImageView底层实现原理
#### 通过UIImageView方法创建
```objc
    // 第一种方式
//    UIImageView *imageView = [[UIImageView alloc] initWithFrame:CGRectMake(0, 0, 200, 200)];
//    // 默认图片会拉伸成跟控件一样大
//    imageView.image = [UIImage imageNamed:@"汽水"];
//    [self.view addSubview:imageView];

    // 默认控件的尺寸跟图片一样
    UIImageView *imageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"CTO"]];
    _imageV = imageView;
    [self.view addSubview:imageView];
```

#### 模仿底层实现
- 表面加载代码与系统提供的第二种方法一样
```objc
    FZQImageView *imageV = [[FZQImageView alloc] initWithImage:[UIImage imageNamed:@"CTO"]];
    _imageView = imageV;
    [self.view addSubview:imageV];
```

#####底层实现
#####在FZQImageView.h中
```objc
@interface FZQImageView : UIView
//提供加载方法
- (instancetype)initWithImage:(UIImage *)image;
//设置图片属性
@property (nonatomic, strong) UIImage *image;
@end
```

#####在FZQImageView.m中
```objc
@implementation FZQImageView
- (instancetype)initWithImage:(UIImage *)image
{
    if (self = [super init]) {
        self.frame = CGRectMake(0, 0, image.size.width, image.size.height);
        // 赋值并绘图
        _image = image;
    }
    return self;
}
//重写set方法，每次赋值时重绘
- (void)setImage:(UIImage *)image
{
    _image = image;

    ###核心代码
    [self setNeedsDisplay];
}

## 绘制图片到view中（核心代码）
- (void)drawRect:(CGRect)rect {
    [_image drawInRect:rect];
}
@end
```
