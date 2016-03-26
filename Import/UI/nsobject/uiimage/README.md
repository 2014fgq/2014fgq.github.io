# UIImage
- 继承自NSObject

#常用方法
- 图片的加载方式
    - 有缓存：imageNamed
    ```objc
    UIImage *image = [UIImage imageNamed:@"图片名"];
    ```
        - 使用场合：图片比较小、使用频率较高
        - 建议把需要缓存的图片直接放到Images.xcassets（新版xcode叫Assets.xcassets）

    - 无缓存：imageWithContentsOfFile
    ```objc
    NSString *file = [[NSBundle mainBundle] pathForResource:@"图片名" ofType:@"图片的扩展名"];
    UIImage *image = [UIImage imageWithContentsOfFile:@"图片文件的全路径"];
    ```
        - 使用场合：图片比较大、使用频率较小
        - 不需要缓存的图片不能放在Images.xcassets（新版xcode叫Assets.xcassets）
    - 放在Images.xcassets里面的图片，只能通过图片名去加载图片，这是因为在app中，会将这里的文件压缩，无法取得全地址


## 图片拉伸
- iOS5之前

```objc
// 只拉伸中间的1x1区域
- (UIImage *)stretchableImageWithLeftCapWidth:(NSInteger)leftCapWidth topCapHeight:(NSInteger)topCapHeight;
```

- iOS5开始

```objc
//默认是平铺模式
-(UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets;
//比上面方法多了伸缩方式（包括平铺和拉伸）
-(UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets resizingMode:(UIImageResizingMode)resizingMode;```


## 图片渲染
```objc
// 在iOS7之后,默认会导航条上按钮的图片渲染成蓝色
    // 获取图片
    UIImage *image = [UIImage imageNamed:@"navigationbar_friendsearch"];
    // 告诉这个图片不要渲染
    // 返回一个没有渲染的图片给你
    image = [image imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
```


