# UIControl
- 父类是UIView
- 凡是继承自UIControl的控件，都可以通过addTarget:...方法来监听事件

## 监听按钮点击
```objc
[button addTarget:self action:@selector(buttonClick:) forControlEvents:UIControlEventTouchUpInside];
```

## 如何监听控件的行为
- 通过addTarget:
    - 只有继承自UIControl的控件，才有这个功能
    - UIControlEventTouchUpInside : 点击事件（UIButton）
    - UIControlEventValueChanged : 值改变事件（UISwitch、UISegmentControl、UISlider）
    - UIControlEventEditingChanged : 文字改变事件（UITextField）
- 通过delegate
    - 只有拥有delegate属性的控件，才有这个功能

##常用属性
```objc
//按钮是否可用
@property(nonatomic,getter=isEnabled) BOOL enabled;
```

