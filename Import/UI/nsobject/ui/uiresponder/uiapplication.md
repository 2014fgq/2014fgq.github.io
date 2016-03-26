# UIApplication
- 父类是UIResponder
- 是单例
- 一个iOS程序启动后创建的第一个对象就是UIApplication对象
- 每一个应用程序都有，UIApplication对象是应用程序的象征，利用UIApplication对象，能进行一些应用级别的操作
    1. 应用程序图片的提醒数字
     - 设置应用图标右上角的数字,图标需要手动清除,应用程序关闭,不会自动清除.
     ```objc
     app.applicationIconBadgeNumber = 10;
     ```
    2. 联网状态
     - 显示联网状态,告诉用户此应用正在联网
    ```objc
    app.networkActivityIndicatorVisible = YES;
    ```
    3. 设置状态栏
     - UIApplication管理状态栏.ios7开始默认交给控制器,需要配置下,不交给控制器管理,就会交给UIApplication管理。
    4. 注册用户通知
    ```objc
    // 创建通知
UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeBadge categories:nil];
    // 注册用户通知
[app registerUserNotificationSettings:settings];
```
    5. 打开资源,电话,网页,发短信
        4>打开一个网页资源,UIApplication打开资源的好处:不用判断用什么软件打开,系统会自动根据协议头判断。
            4.1>网络资源URL的组成==协议头://主机域名/路径 http://www.baidu.com/abc/1.png
            4.2>本地资源URL的组成==协议头:///路径 本机域名可以不写 file:///User/apple/Desktop/1.png

```objc
//十分强大的openURL:方法
[app openURL:[NSURL URLWithString:@"http://ios.itcast.cn"]];
//打电话
UIApplication *app = [UIApplication sharedApplication];
[app openURL:[NSURL URLWithString:@"tel://10086"]];
//发短信
[app openURL:[NSURL URLWithString:@"sms://10086"]];
//发邮件
[app openURL:[NSURL URLWithString:@"mailto://12345@qq.com"]];
```


## 程序启动原理
一. `加载程序中类的类对象`

二. `首先找到程序入口,执行main函数`

main -> UIApplicationMain

三. `UIApplicationMain底层做事情`

1. 创建UIApplication对象及子类

2. 创建UIApplication的代理对象,而且给UIApplication对象代理属性赋值

3. 开启主运行循环,作用接收事件,让程序一直运行

4. 加载info.plist,判断有木有指定main.storyboard,如果指定就会去加载

5. 创建窗口UIWindow。在加载info.plist文件之后,程序启动才完成,启动完成之后,就要显示窗口,因此在程序启动完成的时候创建窗口.

6.加载main.storyboard并实例化控制器（在创建窗口之后，详见苹果官方文档）

7.让控制器成为窗口的根控制器，并让窗口成为主窗口并显示,在窗口显示前，会懒加载根控制器的view，最后展示界面

8.程序启动完成


##常用属性
```objc
//设置应用程序图标右上角的红色提醒数字
@property(nonatomic) NSInteger applicationIconBadgeNumber;
//设置联网指示器的可见性
@property(nonatomic,getter=isNetworkActivityIndicatorVisible) BOOL networkActivityIndicatorVisible;
```


## UIApplicationMain函数
- main函数中执行了一个UIApplicationMain这个函数

```objc
//argc、argv：直接传递给UIApplicationMain进行相关处理即可
//principalClassName：指定应用程序类名（app的象征），该类必须是UIApplication(或子类)。如果为nil,则用UIApplication类作为默认值

//delegateClassName：指定应用程序的代理类，该类必须遵守UIApplicationDelegate协议,并让其成为程序类的代理

//程序正常退出时UIApplicationMain函数才返回

int UIApplicationMain(int argc, char *argv[], NSString *principalClassName, NSString *delegateClassName);
```



### iOS7中的状态栏
1. 从iOS7开始，系统提供了2种管理状态栏的方式
    1. 通过UIViewController管理（每一个UIViewController都可以拥有自己不同的状态栏）
    - 通过UIApplication管理（一个应用程序的状态栏都由它统一管理）
- 在iOS7中，默认情况下，状态栏都是由UIViewController管理的，UIViewController实现下列方法就可以轻松管理状态栏的可见性和样式

```objc
//状态栏的样式
- (UIStatusBarStyle)preferredStatusBarStyle
{
    return UIStatusBarStyleLightContent;
};
//状态栏的可见性
// 隐藏状态栏
app.statusBarHidden = YES;
//动画隐藏状态栏
[app setStatusBarHidden:YES withAnimation:UIStatusBarAnimationSlide]
```
