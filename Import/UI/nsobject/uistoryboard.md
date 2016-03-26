# UIStoryboard
- 父类是NSObject
- 没有成员变量
- 只有类创建方法

```objc
+ (UIStoryboard *)storyboardWithName:(NSString *)name bundle:(nullable NSBundle *)storyboardBundleOrNil;```

## 系统创建UIStoryboard的流程
1. 程序启动完成时，会判断主界面是否设置了main storyboard，如果有，就会加载storyboard,自动创建好窗口和根控制器。

- 如果设置的不是main storyboard，而是我们创建的其他storyboard，就会加载我们创建的。（如果没能成功，需要清除缓存）

- 如果没有设置，就会根据对应的代码创建对应的storyboard及其描述的控制器。必须要有storyboard,创建UIStoryboard对象才有意义,alloc init创建UIStoryboard对象没有意义

```objc
    // 1.根据storyboard名称和地址创建storyboard，并创建描述的控制器
    // Name:storyboard文件名
    // nil = [NSBundle mainBundle]
    UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"Main" bundle:nil];

    //2.根据标识符加载描述的控制器（已创建storyboard)
    //标识不能乱传,必须storyboard有这个标识，否则报错
//    UIViewController *vc = [storyboard instantiateViewControllerWithIdentifier:@"vc1"];;

    // 3.加载箭头指向的控制器（已创建storyboard)
    // instantiateInitialViewController:
    UIViewController *vc = [storyboard instantiateInitialViewController];```


## storyboard创建的控制器
- 现在创建的控制器都不能处理事件,如果需要处理事件,需要自定义控制器。
    1. 原因:当通过storyboard创建控制器对象,默认都是系统自带的控制器对象,系统自带的是不能处理事件的。他不能写监听方法
    - 如果要让控制器能处理事件，需要自定义控制器，并设置storyboard中custom class类型。

