# NSNotificationCenter（通知中心）
- 每一个应用程序都有一个通知中心(NSNotificationCenter)实例，专门负责协助不同对象之间的消息通信
- 任何一个对象都可以向通知中心发布通知(NSNotification)，描述自己在做什么。其他感兴趣的对象(Observer)可以申请在某个特定通知发布时(或在某个特定的对象发布通知时)收到这个通知
- [NSNotificationCenter defaultCenter]是单例

##发送通知和接收通知步骤
1. 初始化一个通知（NSNotification）对象
```objc
 /**
    * (NSString *)name：通知的名称
    * (id)object：通知发布者(是谁要发布通知)
    * (NSDictionary*)userInfo：一些额外的信息(通知发布者传递给通知接收者的信息内容)
    */
+(instancetype)notificationWithName:(NSString *)aName object:(id)anObject userInfo:(NSDictionary *)aUserInfo;
-(instancetype)initWithName:(NSString *)name object:(id)object userInfo:(NSDictionary *)userInfo;```

- 在通知中心注册监听器
    - 在通知中心(NSNotificationCenter)注册监听器(Observer)，当通知中心出现某类信息后，回传给对应的监测对象，并让监听对象执行某个方法

```objc
 /**
    * observer：监听对象，即谁要接收这个通知
    * aSelector：收到通知后，回调监听对象的这个方法，并且把通知对象当做参数传入
    * aName：通知的名称。如果为nil，那么无论通知的名称是什么，监听对象都能收到这个通知
    * anObject：发布通知对象。如果为anObject和aName都为nil，监听对象都收到所有的通知
    */
-(void)addObserver:(id)observer selector:(SEL)aSelector name:(NSString *)aName object:(id)anObject;```

```objc
 /**
    * name：通知的名称
    * obj：发布通知者
    * block：收到对应的通知时，会回调这个block
    * queue：决定了block在哪个操作队列中执行，如果传nil，默认在当前操作队列中同步执行
    */
- (id)addObserverForName:(NSString *)name object:(id)obj queue:(NSOperationQueue *)queue usingBlock:(void (^)(NSNotification *note))block;```

- 发送通知

```objc
- (void)releasingNoticesWithDict:(NSDictionary*) dict{
    [[NSNotificationCenter defaultCenter] postNotificationName:@"shopNotification" object:self userInfo:dict];
}```

- 取消注册监听器
    - 通知中心不会保留(retain)监听器对象，在通知中心注册过的对象，必须在该对象释放前取消注册。否则，当相应的通知再次出现时，通知中心仍然会向该监听器发送消息。因为相应的监听器对象已经被释放了，所以可能会导致应用崩溃

```objc
    //一般在监听器销毁之前取消注册（如在监听器中加入下列代码）：
-(void)dealloc {
	//[super dealloc];  非ARC中需要调用此句
    [[NSNotificationCenter defaultCenter] removeObserver:self];//取消这个监测对象所有注册信息
    //取消这个监测对象某个信息的注册
    //-(void)removeObserver:(id)observer name:(NSString *)aName object:(id)anObject;
    }```

###键盘通知
- 我们经常需要在键盘弹出或者隐藏的时候做一些特定的操作,因此需要监听键盘的状态

- 键盘状态改变的时候,系统会发出一些特定的通知
```objc
// 键盘即将显示
UIKeyboardWillShowNotification
// 键盘显示完毕
UIKeyboardDidShowNotification
// 键盘即将隐藏
UIKeyboardWillHideNotification
// 键盘隐藏完毕
UIKeyboardDidHideNotification
// 键盘的位置尺寸即将发生改变
UIKeyboardWillChangeFrameNotification
// 键盘的位置尺寸改变完毕
UIKeyboardDidChangeFrameNotification ```

- 系统发出键盘通知时,会附带一下跟键盘有关的额外信息(字典),字典常见的key如下:

```objc
// 键盘刚开始的frame
UIKeyboardFrameBeginUserInfoKey
// 键盘最终的frame(动画执行完毕后)
UIKeyboardFrameEndUserInfoKey
// 键盘动画的时间
UIKeyboardAnimationDurationUserInfoKey
// 键盘动画的执行节奏(快慢)
UIKeyboardAnimationCurveUserInfoKey ```
