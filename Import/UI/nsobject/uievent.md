# UIEvent
- 父类是NSObject
- 每产生一个事件，就会产生一个UIEvent对象
- UIEvent：称为事件对象，记录事件产生的时刻和类型


##常见属性
```objc
事件类型
@property(nonatomic,readonly) UIEventType     type;
@property(nonatomic,readonly) UIEventSubtype  subtype;

事件产生的时间
@property(nonatomic,readonly) NSTimeInterval  timestamp;
```

##常见方法
- UIEvent还提供了相应的方法可以获得在某个view上面的触摸对象（UITouch）

