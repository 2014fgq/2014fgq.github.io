# UIControl
- 父类是UIView
- 凡是继承自UIControl的控件，都可以通过addTarget:...方法来监听事件

## 监听按钮点击
```objc
[button addTarget:self action:@selector(buttonClick:) forControlEvents:UIControlEventTouchUpInside];
```
