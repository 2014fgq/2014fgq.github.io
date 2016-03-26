# UIImageView
- 父类是UIView
- UIImageView极其常用，功能比较专一：显示图片


##常见属性

1. image：显示的图片
```objc
@property(nonatomic,retain) UIImage *image;```

- animationImages：显示的动画图片
```objc
@property(nonatomic,copy) NSArray *animationImages;```

- animationDuration：动画图片的持续时间
```objc
@property(nonatomic) NSTimeInterval animationDuration;```

- animationRepeatCount：动画的播放次数（默认是0，代表无限播放
```objc
@property(nonatomic) NSInteger      animationRepeatCount;```


##常见方法
1. startAnimating：开始动画
```objc
-(void)startAnimating; ```

- stopAnimating：停止动画
    - 使用场景：根据播放时间设置停止动画
```objc
-(void)stopAnimating; ```

- isAnimating：是否正在执行动画
```objc
-(BOOL)isAnimating; ```

- initWithImage：利用initWithImage创建出来的imageView的尺寸和传入的图片尺寸一样
```objc
-(nullable instancetype)initWithImage:(UIImage *)image;```

