# UITouch
- 父类是NSObject
- 当用户用一根手指触摸屏幕时，会创建一个与手指相关联的UITouch对象
- 作用:保存跟手指相关的信息，比如触摸的位置、时间、阶段
- 当手指移动时，系统会更新同一个UITouch对象，使之能够一直保存该手指在的触摸位置
- 当手指离开屏幕时，系统会销毁相应的UITouch对象- 注意：iPhone开发中，要避免使用双击事件！


##UITouch的的属性
```objc
//触摸产生时所处的窗口
@property(nonatomic,readonly,retain) UIWindow    *window;

//触摸产生时所处的视图
@property(nonatomic,readonly,retain) UIView      *view;

//短时间内点按屏幕的次数，可以根据tapCount判断单击、双击或更多的点击
@property(nonatomic,readonly) NSUInteger          tapCount;

//记录了触摸事件产生或变化时的时间，单位是秒
@property(nonatomic,readonly) NSTimeInterval      timestamp;

//当前触摸事件所处的状态,开始点击？移动？结束点击三种状态
@property(nonatomic,readonly) UITouchPhase        phase;
```

##UITouch的方法
```objc
//返回值表示触摸在view上的位置（以view的左上角为原点(0, 0)）
//调用时传入的view参数为nil的话，返回的是触摸点在UIWindow的位置
- (CGPoint)locationInView:(UIView *)view;

//记录前一个触摸点的位置
- (CGPoint)previousLocationInView:(UIView *)view;

```
