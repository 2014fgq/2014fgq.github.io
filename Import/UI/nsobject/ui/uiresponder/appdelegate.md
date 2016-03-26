# AppDelegate
1. 父类是UIResponder
- 所有的移动操作系统都有个致命的缺点：app很容易受到打扰。比如一个来电或者锁屏会导致app进入后台甚至被终止;在app受到干扰时会产生一些系统事件，这时`UIApplication`会通知它的delegate对象来处理，包括：
    1. 应用程序的生命周期事件(如程序启动和关闭)
    - 系统事件(如来电)
    - 内存警告
- 作用:开启事件循环,UIApplicationDelegate不断监听事件。如果产生系统事件,就会通知代理,其他事件,会找到一个最合适的视图处理事件。
    1. delegateClassName不能传nil,这里传nil,意味着application没有代理,就无法监听系统的事件,系统的事件都没法监听,窗口都不知道什么时候去加载,因为视图都是懒加载的,因此就不会创建窗口,什么东西都没有。
    - 不是任何事件都交给他处理,按钮点击,屏幕点击都不是他处理,是由UIApplication处理。
- 遵守UIApplicationDelegate协议，使用以下方法处理系统事件

```objc
// app接收到内存警告时调用:清空不必要的内容
- (void)applicationDidReceiveMemoryWarning:(UIApplication *)application;

// app进入后台时调用（比如按了home键）:一般在这里保存应用的数据(游戏数据,比如暂停游戏)
- (void)applicationDidEnterBackground:(UIApplication *)application;

// app启动完毕时调用,执行初始化的操作
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions;```


###应用程序的生命周期
```objc
// 应用程序启动完成的时候调用
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSLog(@"%s",__func__);
    return YES;
}

// 当我们应用程序即将失去焦点的时候调用
- (void)applicationWillResignActive:(UIApplication *)application {
    NSLog(@"%s",__func__);
}

// 当我们应用程序完全进入后台的时候调用
- (void)applicationDidEnterBackground:(UIApplication *)application{
   NSLog(@"%s",__func__);
}

// 当我们应用程序即将进入前台的时候调用
- (void)applicationWillEnterForeground:(UIApplication *)application {
  NSLog(@"%s",__func__);}

// 当我们应用程序完全获取焦点的时候调用
// 只有当一个应用程序完全获取到焦点,才能与用户交互.
- (void)applicationDidBecomeActive:(UIApplication *)application {
    NSLog(@"%s",__func__);
}

// 当我们应用程序即将关闭的时候调用,一般没什么用，因为应用程序关闭时也不能再进行什么操作
- (void)applicationWillTerminate:(UIApplication *)application {
   NSLog(@"%s",__func__);
}```
