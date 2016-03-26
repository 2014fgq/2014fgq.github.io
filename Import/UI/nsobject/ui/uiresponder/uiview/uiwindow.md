# UIWindow
1. 分类是UIView,它是一个特别的UIView
- 被AppDelegate用强指针指向，所以不会被释放；每个App至少有一个UIWindow，是第一个创建的视图控件
- 特殊的窗口：状态栏(无法通过application.windows打印显示，原因是它没有交给application管理）;键盘.
- 创建步骤：
        1.1 创建窗口

        1.2 加载根控制器

        1.3 设置窗口的根控制器,显示窗口
- 创建时间：程序启动完毕的时候才创建的,如果通过storyboard加载程序,窗口都交给storyboard管理,不需要自己管理
- 旋转事件顺序->UIApplication->Window->rootViewController ->旋转控制器的view
- 为什么要用跟控制器开发,没有跟控制器一样能显示界面。
    - 原因：以后可能会有很多界面,为了避免代码结构混乱,通过一个界面交给一个控制器管理候就更改窗口的根
rootViewControllerd的常见用处:切换控制器,比如新特性界面展示完成,切换到主界面,这时都会交给控制器就好了,新特性界面就会自动销毁,它只需要展示一次就好了。


## UIWindow手动创建步骤

```objc
// 在开发中通常在程序启动完成的时候手动创建窗口
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // 窗口显示的注意点:
    // 1.一定要强引用
    // 2.控件要想显示出来,必须要有尺寸

    // 1.创建窗口
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];

    //不设置窗口颜色，最后会显示的是屏幕的颜色——黑色
    //self.window.backgroundColor = [UIColor yellowColor];

    // 2.创建根控制器,在设置窗口的根控制器
    UIViewController *vc = [[UIViewController alloc] init];

    //每个vc创建都会自动创建一个view的成员变量，当第一次使用时懒加载，详见view的加载
    //vc.view.backgroundColor = [UIColor greenColor];

    // 设置窗口的根控制器,底层会自动把根控制器的view添加到窗口上,并且让控制器的view有旋转功能
    // 如果通过addSubView加载控制器的view,控制器会被释放,控制器就不能处理事件，另外控制器的view不会自动旋转。
    self.window.rootViewController = vc;

    // 3.显示窗口
    // makeKeyAndVisible:让窗口成为应用程序的主窗口,并且显示窗口（hidden属性为NO）；注意，若再创建一个window，调用makeKeyAndVisible方法，并不会使之前的window隐藏
    [self.window makeKeyAndVisible];

    //打印窗口
    NSLog(@"%@",application.windows);//self.windows是代理AppDelegate的窗口，不是应用程序下的窗口
    return YES;
}```


### 窗口的层级
- 设置窗口的层级,层级谁大就显示在最外面
- `UIWindowLevelAlert` > `UIWindowLevelStatusBar` > `UIWindowLevelNormal`（本质是枚举值(数字)，如果UIWindowLevelAlert+1，这时窗口层级会更高）
    - `UIWindowLevelNormal`: 默认窗口的层级
    - `UIWindowLevelStatusBar` : 状态栏,键盘、
    - `UIWindowLevelAlert`:UIActionSheet,UIAlearView

```objc
self.window1.windowLevel = UIWindowLevelAlert```


## 常用方法
```objc
//让当前UIWindow变成keyWindow（主窗口），但要遵守显示层级来显示
- (void)makeKeyWindow;

//让当前UIWindow变成keyWindow，并显示出来
- (void)makeKeyAndVisible;

//在本应用中打开的UIWindow列表
[UIApplication sharedApplication].windows//这样就可以接触应用中的任何一个UIView对象(平时输入文字弹出的键盘，就处在一个新的UIWindow中)

//用来接收键盘以及非触摸类的消息事件的UIWindow
[UIApplication sharedApplication].keyWindow//程序中每个时刻只能有一个UIWindow是keyWindow。如果某个UIWindow内部的文本框不能输入文字，可能是因为这个UIWindow不是keyWindow

//获得某个UIView所在的UIWindow
view.window
```






