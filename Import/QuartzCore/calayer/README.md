# CALayer
- 父类是NSObject
- layer(图层）：在iOS中，UIView之所以能显示在屏幕上，完全是因为它内部的一个图层，它本身不具备显示的功能，这个图层(即CALayer对象)，可以通过layer属性可以访问这个层
```objc
@property(nonatomic,readonly,retain) CALayer *layer;
```
- 创建时间：创建UIView时就生成成员属性layer
- 显示时间：当UIView需要显示到屏幕上时，会调用drawRect:方法进行绘图，并且会将所有内容绘制在自己的图层上，绘图完毕后，系统会将图层拷贝到屏幕上，于是就完成了UIView的显示
- 通过操作CALayer对象，可以很方便地调整UIView的一些外观属性
    -  阴影
    -  圆角大小
    -  边框宽度和颜色
    - 给图层添加动画，来实现一些比较炫酷的效果等


##常见属性
```objc
宽度和高度
@property CGRect bounds;

位置(默认指中点，具体由anchorPoint决定)
@property CGPoint position;

锚点(x,y的范围都是0-1)，决定了position的含义
@property CGPoint anchorPoint;

背景颜色(CGColorRef类型)
@property CGColorRef backgroundColor;

形变属性
类型：CATransform3DMakeScale、CATransform3DMakeRotation等
@property CATransform3D transform;

边框颜色(CGColorRef类型)
@property CGColorRef borderColor;

边框宽度
@property CGFloat borderWidth;

//圆角半径
@property CGColorRef borderColor;

内容(比如设置为图片CGImageRef)
@property(retain) id contents;

// 超出主层边框的都会被裁减调用
@property BOOL masksToBounds
_imageView.layer.masksToBounds = YES;

```

###关于CALayer的疑惑
1. 首先CALayer是定义在QuartzCore框架中的，CGImageRef、CGColorRef两种数据类型是定义在CoreGraphics框架中的，UIColor、UIImage是定义在UIKit框架中的

- 其次QuartzCore框架和CoreGraphics框架是可以跨平台使用的，在iOS和Mac OS X上都能使用但是UIKit只能在iOS中使用

- 为了保证可移植性，QuartzCore不能使用UIImage、UIColor，只能使用CGImageRef、CGColorRef



###UIView和CALayer的选择
1. 如果显示出来的东西需要跟用户进行交互的话，用UIView；

- 如果不需要跟用户进行交互，用UIView或者CALayer都可以
    - CALayer的性能会高一些，因为它少了事件处理的功能，更加轻量级



###position和anchorPoint
- CALayer有2个非常重要的属性：position和anchorPoint

- position:
    - 用来设置CALayer在父层中的位置
    - 以父层的左上角为原点(0, 0)
`@property CGPoint position;`

- anchorPoint(定位点，锚点）:
    - 决定着CALayer身上的哪个点会在position属性所指的位置
    - 以自己的左上角为原点(0, 0)
    - 它的x、y取值范围都是0~1，默认值为（0.5, 0.5），是比例值。
    - 如默认值（0.5, 0.5），即layer的中心点在positon位置上；若是（1，1），则是layer的右下角的点在position的位置上，对平常的UIView来说，锚点相当于（0，0）
`@property CGPoint anchorPoint;`



## 隐式动画
- 隐式动画:当对非Root Layer的部分属性进行修改时，默认会自动产生一些动画效果,而这些属性称为Animatable Properties(可动画属性)
    1. 根层：每一个UIView内部都默认关联着一个CALayer，我们可用称这个Layer为Root Layer（根层）

    - 非根层：所有的非Root Layer，也就是手动创建的CALayer对象，都存在着隐式动画

- 几个常见的Animatable Properties：
    1. bounds：用于设置CALayer的宽度和高度。修改这个属性会产生缩放动画
    - backgroundColor：用于设置CALayer的背景色。修改这个属性会产生背景色的渐变动画
    - position：用于设置CALayer的位置。修改这个属性会产生平移动画

- 可以通过动画事务(CATransaction)关闭默认的隐式动画效果
```objc
    // 开启事务
    [CATransaction begin];

    // 设置动画时长
    [CATransaction setAnimationDuration:0.5];

    // 动画
    // 可动画属性
    _layer.position = CGPointMake(arc4random_uniform(250), arc4random_uniform(500));

    // 背景颜色
    _layer.backgroundColor = [self randomColor].CGColor;

    // 圆角半径
    // 长方形的时候不能设置圆角变成圆
    // 做动画,尽量不要使用修改圆角半径做动画
    _layer.cornerRadius = arc4random_uniform(50);

    // 阴影（阴影设置的效果是太阳效果）
    // 显示阴影
    _redView.layer.shadowOpacity = 1;
    //    _redView.layer.shadowOffset = CGSizeMake(10, 10);
    _redView.layer.shadowColor = [UIColor yellowColor].CGColor;

    _redView.layer.shadowRadius = 10;

    // 边框
    _layer.borderWidth = arc4random_uniform(10);

    _layer.borderColor = [self randomColor].CGColor;

    // 提交事务
    [CATransaction commit];
```

## 常用方法
```objc
[self.view.layer addSublayer:layer];
```

###特殊的View
- UIImgeView + UIImageView圆角半径 + 主层和contents + 裁剪 + 阴影无效，达到效果，圆形头像
- 默认UIImageView的图片不是显示到主层的子层中,而是在content层中


### 时钟实现案例
- 实现步骤：
    - 布局控件：指针都是绕圆心旋转，所以要设置锚点和positon在圆心
    - 开启计时，调用时间改变方法
    - 生成日历对象，获取当前时间
    - 计算时分秒转动角度，设置形变旋转
    - 初始化时针，分针，秒针位置

```objc
// 每秒秒针转6°
#define kPerSecondA 6
// 每分钟分针转6°
#define kPerMinuteA 6

// 每小时时针转30°
#define kPerHourA 30

// 每分钟时针转0.5°
#define kPerMinHourA 0.5
// 每秒分针转0.1°
#define kPerMinSecA 0.1

@interface ViewController ()
@property (weak, nonatomic) IBOutlet UIImageView *imageView;

@property (weak, nonatomic)  CALayer *secLayer;

@property (weak, nonatomic)  CALayer *minLayer;

@property (weak, nonatomic)  CALayer *hourLayer;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    // 小时
    [self setUpHourLayer];

    // 分
    [self setUpMinLayer];

    // 秒
    [self setUpSecLayer];

    // 定时器
    [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(timeChange) userInfo:nil repeats:YES];

    // 初始化始终
    [self timeChange];
}

- (void)timeChange
{
##核心代码
    // 获取当前是多少秒
    // 获取当前的日历对象
    NSCalendar *calendar = [NSCalendar currentCalendar];

    // 日期组件:秒,分,小时,年,月,日......
    // NSCalendarUnit:表示日期组件有哪些组成
    NSDateComponents *dataCmp = [calendar components:NSCalendarUnitSecond | NSCalendarUnitMinute | NSCalendarUnitHour fromDate:[NSDate date]];


    // 获取分
    NSInteger minute = dataCmp.minute;
##
    // 计算下分针转多少度
    CGFloat minA = (min * kPerMinuteA + sec * kPerMinSecA) / 180.0 * M_PI;

    // 旋转分针
    _minLayer.transform = CATransform3DMakeRotation(minA, 0, 0, 1);


    // 获取小时
    NSInteger hour = dataCmp.hour;

    // 计算时针转多少度 = 当前有多少小时 * 每小时转的度数30° + 当前多少分钟 * 每分钟时针转多少0.5
    CGFloat hourA = (hour * kPerHourA + minute * kPerMinHourA) / 180.0 * M_PI;

    // 旋转时针
    _hourLayer.transform = CATransform3DMakeRotation(hourA, 0, 0, 1);

    // 获取秒数
    NSInteger sec = dataCmp.second;

    // 秒针转多少度
    CGFloat secA = (sec * kPerSecondA) / 180.0 * M_PI;

    // 旋转秒针
    _secLayer.transform = CATransform3DMakeRotation(secA, 0, 0, 1);
}

// 秒
- (void)setUpSecLayer
{
    CALayer *layer = [CALayer layer];

    _secLayer = layer;

    layer.backgroundColor = [UIColor redColor].CGColor;

    // 绕着锚点旋转,设置锚点
    layer.anchorPoint = CGPointMake(0.5, 1);

    layer.position = CGPointMake(_imageView.bounds.size.width * 0.5, _imageView.bounds.size.height * 0.5);

    layer.bounds = CGRectMake(0, 0, 1, 80);

    [_imageView.layer addSublayer:layer];
}

// 分
- (void)setUpMinLayer
{
    CALayer *layer = [CALayer layer];

    _minLayer = layer;

    layer.cornerRadius = 4;

    layer.backgroundColor = [UIColor blackColor].CGColor;

    // 绕着锚点旋转,设置锚点
    layer.anchorPoint = CGPointMake(0.5, 1);

    layer.position = CGPointMake(_imageView.bounds.size.width * 0.5, _imageView.bounds.size.height * 0.5);

    layer.bounds = CGRectMake(0, 0, 4, 80);

    [_imageView.layer addSublayer:layer];
}

// 小时
- (void)setUpHourLayer
{
    CALayer *layer = [CALayer layer];

    _hourLayer = layer;

    layer.cornerRadius = 4;

    layer.backgroundColor = [UIColor blackColor].CGColor;

    // 绕着锚点旋转,设置锚点
    layer.anchorPoint = CGPointMake(0.5, 1);

    layer.position = CGPointMake(_imageView.bounds.size.width * 0.5, _imageView.bounds.size.height * 0.5);

    layer.bounds = CGRectMake(0, 0, 4, 75);

    [_imageView.layer addSublayer:layer];
}
```
