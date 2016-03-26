# NSTimer
- 父类是NSObject
- 名称：“定时器”
- 作用：
    - 在指定的时间执行指定的任务
    - 每隔一段时间执行指定的任务


##使用方法
1. 开启一个定时任务
```objc
//每隔ti秒，调用一次aTarget的aSelector方法，yesOrNo决定了是否重复执行这个任务.
//会返回一个NSTimer类型的指针，让其成为控制器的成员变量，方便日后关闭定时器，因为定时器的关闭方法是对象方法
+(NSTimer *)scheduledTimerWithTimeInterval:(NSTimeInterval)ti 		target:(id)aTarget
	selector:(SEL)aSelector
	userInfo:(id)userInfo
	repeats:(BOOL)yesOrNo;```

- 停止定时器
```objc
//一旦定时器被停止了，就不能再次执行任务。只能再创建一个新的定时器才能执行新的任务
-(void)invalidate;```


###解决定时器在主线程不工作的问题
```objc
//创建新的NSTimer,并每隔2秒后执行'next'方法，返回指针
NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:2 target:self selector:@selector(next) userInfo:nil repeats:YES];
//让主线程抽空关注Timer的执行
[[NSRunLoop mainRunLoop] addTimer:timer forMode:NSRunLoopCommonModes];```
