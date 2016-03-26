# UITapGestureRecognizer（点按）
- 父类是UIGestureRecognizer


##UITapGestureRecognizer应用
```objc
//创建手势识别器对象
UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] init];

//设置手势识别器对象的具体属性
// 连续敲击2次
tap.numberOfTapsRequired = 2;
// 需要2根手指一起敲击
tap.numberOfTouchesRequired = 2;

//添加手势识别器到对应的view上
[self.iconView addGestureRecognizer:tap];

//监听手势的触发
[tap addTarget:self action:@selector(tapIconView:)];
```
