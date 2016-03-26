# UIGestureRecognizer
- 父类是NSObject
- 利用手势识别器---UIGestureRecognizer，能轻松识别用户在某个view上面做的一些常见手势
- UIGestureRecognizer是一个抽象类，定义了所有手势的基本行为，使用它的子类才能处理具体的手势
    1. UITapGestureRecognizer(敲击)
    - UIPinchGestureRecognizer(捏合，用于缩放)
    - UIPanGestureRecognizer(拖拽)
    - UISwipeGestureRecognizer(轻扫)
    - UIRotationGestureRecognizer(旋转)
    - UILongPressGestureRecognizer(长按)
- 一个手势只能支持一个方向
- 默认只支持一个手势，若要实现多个手势，需要设置手势代理，遵守<UIGestureRecognizerDelegate>，并实现以下方法：

```objc
// Simultaneously:同时
// 是否允许同时支持多个手势
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer
{
    return YES;
}
```


##手势识别的状态
```objc
typedef NS_ENUM(NSInteger, UIGestureRecognizerState) {
    // 没有触摸事件发生，所有手势识别的默认状态
    UIGestureRecognizerStatePossible,
    // 一个手势已经开始但尚未改变或者完成时
    UIGestureRecognizerStateBegan,
    // 手势状态改变
    UIGestureRecognizerStateChanged,
    // 手势完成
    UIGestureRecognizerStateEnded,
    // 手势取消，恢复至Possible状态
    UIGestureRecognizerStateCancelled,
    // 手势失败，恢复至Possible状态
    UIGestureRecognizerStateFailed,
    // 识别到手势识别
    UIGestureRecognizerStateRecognized = UIGestureRecognizerStateEnded
};
```

#####UITapGestureRecognizer(敲击)
`详见“NSObject - UIGestureRecognizer - UITapGestureRecognizer”`

#####UIPinchGestureRecognizer(捏合，用于缩放)
`详见“NSObject - UIGestureRecognizer - UIPinchGestureRecognizer”`

#####UIPanGestureRecognizer(拖拽)
`详见“NSObject - UIGestureRecognizer - UIPanGestureRecognizer”`

#####UISwipeGestureRecognizer(轻扫)
`详见“NSObject - UIGestureRecognizer - UISwipeGestureRecognizer”`

#####UIRotationGestureRecognizer(旋转)
`详见“NSObject - UIGestureRecognizer - UIRotationGestureRecognizer”`

#####UILongPressGestureRecognizer(长按)
`详见“NSObject - UIGestureRecognizer - UILongPressGestureRecognizer”`



