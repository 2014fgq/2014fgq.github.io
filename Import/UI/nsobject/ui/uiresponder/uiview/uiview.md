# UIView
- 什么是控件？
    - 屏幕上的所有UI元素都叫做控件，也有人叫做视图、组件
按钮（UIButton）、文本（UILabel）都是控件
    - 所有的控件最终都继承自UIView
- UI控件没有显示原因总结：没有frame，没有颜色,超出视图，没添加到当前视图
- UI控件通过alloc.init都没有尺寸，颜色等



##父子控件（自定义控件基础）
- 每个控件都是个容器，能容纳其他控件
    - 内部小控件是大控件的子控件
    - 大控件是内部小控件的父控件


###常用属性

```objc
//superview：获得自己的父控件对象
,每个子控件的superview只有一个，只读`
@property(nonatomic,readonly) UIView *superview;


//subviews：获得自己的所有子控件对象,`subviews是一个数组，只读`
@property(nonatomic,readonly,copy) NSArray *subviews;


//tag：控件的ID(标识)，父控件可以通过tag来找到对应的子控件;tag值是随意设置，很容易混乱，能不用就不用
@property(nonatomic) NSInteger tag;


//是否可与用户交互
@property(nonatomic,getter=isUserInteractionEnabled) BOOL userInteractionEnabled;


//裁剪超出范围的控件
@property(nonatomic) BOOL  clipsToBounds;
```

###常用方法
```objc
//添加一个子控件view
-(void)addSubview:(UIView *)view;

//从父控件中移除
-(void)removeFromSuperview;

//根据一个tag标识找出对应的控件（一般都是子控件）
- (UIView *)viewWithTag:(NSInteger)tag;

```


##UIView位置、尺寸属性

```objc
//frame控件矩形框在父控件中的位置和尺寸(以父控件的左上角为坐标原点)
@property(nonatomic) CGRect frame;


//控件矩形框的位置和尺寸(以自己左上角为坐标原点，所以bounds的x、y一般为0)
    - 调整bounds的x,y值没有效果。设置bounds伸缩是以中心点为基准伸缩的
@property(nonatomic) CGRect bounds;


//控件中点的位置(以父控件的左上角为坐标原点)
@property(nonatomic) CGPoint center;


//控件的形变属性(可以设置旋转角度、比例缩放、平移等属性)
 // CGAffineTransformMakeXXX:每次从零开始形变,每次都是相对于最开始的状态形变
//CGAffineTransformXXX：接着上次的状态形变
@property(nonatomic) CGAffineTransform transform;
```


## 修改frame的3种方式(位置，尺寸的修改可参照）
- 不能直接修改：OC对象的结构体属性的成员
- 直接使用CGRectMake函数
    - 0C结构体常用的修改方式
```objc
imageView.frame = CGRectMake(100, 100, 200, 200);
```
- 利用临时结构体变量
    - 使用场景（常用）：针对结构体中某个变量的修改
    - C结构体的修改方式
```objc
CGRect tempFrame = imageView.frame;
tempFrame.origin.x = 100;
imageView.frame = tempFrame;
```

- 使用大括号{}形式
    - C结构体的修改方式，不常用
```objc
imageView.frame = (CGRect){{100, 100}, {200, 200}};
```



2. UIViewContentMode属性
    - 带有`scale`单词的：图片有可能会拉伸
        - `UIViewContentModeScaleToFill`
            - 将图片拉伸至`填充整个imageView`
            - 图片显示的尺寸跟imageView的尺寸是一样的
        - 带有aspect单词的：保持图片原来的宽高比
            - `UIViewContentModeScaleAspectFit`
             - 保证刚好能看到图片的全部(`长边伸缩至与父控件一致`）
            - `UIViewContentModeScaleAspectFill`
                - 拉伸至图片的宽度或者高度跟imageView一样（`短边伸缩至与父控件一致`）

    - 没有`scale`单词的：图片绝对不会被拉伸，`保持图片的原尺寸`
    - UIViewContentModeCenter
    - UIViewContentModeTop
    - UIViewContentModeBottom
    - UIViewContentModeLeft
    - UIViewContentModeRight
    - UIViewContentModeTopLeft
    - UIViewContentModeTopRight
    - UIViewContentModeBottomLeft
    - UIViewContentModeBottomRight



## UIView的封装(自定义控件）
- 如果一个view内部的子控件比较多，一般会考虑自定义一个view，把它内部子控件的创建屏蔽起来，不让外界关心

- 外界可以传入对应的模型数据给view，view拿到模型数据后给内部的子控件设置对应的数据

### 通过纯代码自定义控件
- 继承自`系统控件（UIView等）`，写一个属于自己的控件
- 步骤
    - 新建一个继承`UIView`的类
    - 提供类方法及对象方法（底层可以调用init或initWithFrame方法，`initWithFrame是一定会调用的方法，先调用父类的，再调用子类`；`对象方法不需声明`）
    - 在`initWithFrame:`方法中添加子控件
    - 在`layoutSubviews`方法中设置子控件的frame
        - 一定要调用`[super layoutSubviews]`;
    - 提供一个模型属性，重写模型属性的set方法
        - 在set方法中取出模型属性，给对应的子控件赋值

### 通过xib自定义控件
- 新建一个继承`UIView`的类
- 新建一个xib文件（xib的文件名最好跟控件类名一样）
    - 添加子控件、设置子控件属性
    - 修改`最外面那个控件的class为控件类名`
    - 将子控件进行连线
- 提供一个类方法，加载xib,详见`IB-xib加载`；及对象方法（`不需声明`）
    - xib的加载，初始化时`不会调用initWithFrame:`方法，只会调用`initWithCoder:`方法（`自动`）。直接向用户提供`initWithCoder：`的对象初始化方法，但这里不设置初始化内容，在下一步中实现
    - 初始化完毕后会调用`awakeFromNib`方法，初始化内容可在这里设置(初始化`完毕前`还会调用`-(id)awakeAfterUsingCoder:(NSCoder *)aDecoder`方法）
- 提供`模型属性`，重写`模型属性的set`方法
    - 在set方法中给子控件设置数据


## 渐变动画
1. 头尾式
```objc
[UIView beginAnimations:nil context:nil];
[UIView setAnimationDuration:2.0];
/* 需要执行动画的代码 */
[UIView commitAnimations];
```

- block式
```objc
[    /** block式动画 */
    //在1秒内逐渐显示指示版
    [UIView animateWithDuration:1.0 animations:^{
        self.hudLbl.alpha = 1;//不透明
    } completion:^(BOOL finished) {
        //上一个动画完毕后延迟3秒后，在1秒内执行下列动画
        [UIView animateWithDuration:1.0 delay:3.0 options:kNilOptions animations:^{
            //显示指示版
            self.hudLbl.alpha = 0;//完全透明
        } completion:nil];
    }];
    ```

##UIView界面跳转
- 界面间的跳转就是一层一层叠起来的View。
    - 之所以说叠，是指如果2个控件如果有重叠部分，那么处于上面的那个控件会盖住下面的。
- 从View A 上点击一个按钮跳转到View B，其实就是把 View B“盖” 在 View A 上面。
- “盖”的方式有好多种，通常的方法有3种：
    1. 用 UINavigationController 把 View B push 进来。
```objc
[self.navigationController pushViewController:nextView animated:YES];
```
    - 用 presentModalViewController 方法把 View B 盖在上面。
```objc
[self presentModalViewController:nextView animated:YES];
```
    - 山寨方法，即把 View A 和 View B 都用 addSubView 加到 AppDelegate 类的 self.window 上。然后就可以调用 bringSubviewToFront 把 View B 显示出来;
    `实质是重新调整subView中元素的顺序，最后一个元素首先显示`
```objc
//AppDelegate.m 类
[self.window addSubview:viewB];
[self.window addSubview:viewA];
// 在需要时调用
[self.window bringSubviewToFront:viewB];
```





