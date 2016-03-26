# UIButton
- 继承自UIControl
- 非常重要的UI控件---UIButton，俗称“按钮”
    - 一般情况下，点击某个控件后，会做出相应反应的都是按钮
    - 按钮的功能比较多，既能显示文字，又能显示图片，还能随时调整内部图片和文字的位置


##UIButton的状态
1. normal（普通状态）
    - 默认情况（Default）
    - 对应的枚举常量：UIControlStateNormal

- highlighted（高亮状态）
    - 按钮被按下去的时候（手指还未松开）
    - 对应的枚举常量：UIControlStateHighlighted
    - 为了保证高亮状态下的图片正常显示，必须设置按钮的type为custom

- disabled（失效状态，不可用状态）
    - 如果enabled属性为NO，就是处于disable状态，代表按钮不可以- 被点击
    - 对应的枚举常量：UIControlStateDisabled

##UIButton 常用方法
1. 设置按钮的文字
```objc
-(void)setTitle:(NSString *)title forState:(UIControlState)state;```

- 设置按钮的文字颜色
```objc
-(void)setTitleColor:(UIColor *)color forState:(UIControlState)state;```

- 设置按钮内部的小图片
```objc
-(void)setImage:(UIImage *)image forState:(UIControlState)state;```

- 设置按钮的背景图片
```objc
-(void)setBackgroundImage:(UIImage *)image forState:(UIControlState)state;```

- 设置按钮的文字字体（需要拿到按钮内部的label来设置）
```objc
btn.titleLabel.font = [UIFont systemFontOfSize:13];```

- 获得按钮的文字
```objc
-(NSString *)titleForState:(UIControlState)state;```

- 获得按钮的文字颜色
```objc
-(UIColor *)titleColorForState:(UIControlState)state;```

- 获得按钮内部的小图片
```objc
-(UIImage *)imageForState:(UIControlState)state;```

- 获得按钮的背景图片
```objc
-(UIImage *)backgroundImageForState:(UIControlState)state;```


###设置按钮的步骤

1. 创建一个自定义的按钮
```objc
UIButton *btn = [UIButton buttonWithType:UIButtonTypeCustom];```

- 默认状态的背景
```objc
[btn setBackgroundImage:[UIImage imageNamed:@"btn_01"] forState:UIControlStateNormal];```

- 默认状态的文字
```objc
[btn setTitle:@"点我啊" forState:UIControlStateNormal];```

- 默认状态的文字颜色
```objc
[btn setTitleColor:[UIColor redColor] forState:UIControlStateNormal];```

##自定义按钮
- 手段：代码和xib都可以
- 步骤：提供类和对象初始化方法-设置父子控件基本属性-通过layoutSubviews布局（xib可以不用）-通过模型属性set方法设置子控件变动属性
- 与通过UIView创建的差异：
    - 按钮是分状态显示的，所以图片和文本等要`分状态设置`，而UIView自定义控件不用；
    - 按钮有图片和背景图片两种，UIImageView只有一种
    - 通过往UIView中放入`imageView`和`label`设置图片和文本；而按钮自带的图片和文本分别是`imageView`和`titleLabel`
    - 调整内部子控件的frame
    - 方式1：实现`titleRectForContentRect:`和`imageRectForContentRect:`方法，分别返回titleLabel和imageView的frame
    - 方式2：在`layoutSubviews`方法中设置
- 按钮内边距
```objc
// 设置按钮内容的内边距（影响到imageView和titleLabel）（统一移动，例如left 4表示往右移，下同）
@property(nonatomic)          UIEdgeInsets contentEdgeInsets;
// 设置titleLabel的内边距（图片不动，文本动）
@property(nonatomic)          UIEdgeInsets titleEdgeInsets;
// 设置imageView的内边距（文本不动，图片动）
@property(nonatomic)          UIEdgeInsets imageEdgeInsets;
```


