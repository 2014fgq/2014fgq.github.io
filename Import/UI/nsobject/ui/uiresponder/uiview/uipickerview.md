# UIPickerView
- 父类是UIView
- 需要设置代理，让代理遵守<UIPickerViewDataSource,UIPickerViewDelegate>协议
- 使用场景：通常在注册模块,当用户需要选择一些东西的时候,比如说城市,往往弹出一个PickerView给他们选择。
- 老虎机效果：
![](UIPickViewios6和ios7.png)


##  UIPickerView所有属性
```objc
//数据源代理
@property(nullable,nonatomic,weak) id<UIPickerViewDataSource> dataSource;

//UIPickerViewDelegate代理
@property(nullable,nonatomic,weak) id<UIPickerViewDelegate>   delegate;

//是否显示选择指示器
@property(nonatomic)        BOOL                       showsSelectionIndicator;   // default is NO

//组数
@property(nonatomic,readonly) NSInteger numberOfComponents;
```


## UIPickerView所有方法
```objc
//设置某组某行的视图
- (nullable UIView *)viewForRow:(NSInteger)row forComponent:(NSInteger)component;

//更新所有组或某组的数据
- (void)reloadAllComponents;
- (void)reloadComponent:(NSInteger)component;

//选中第几组第几行
- (void)selectRow:(NSInteger)row inComponent:(NSInteger)component animated:(BOOL)animated;

//返回在选中的组中的第几行，没有选中则返回-1
- (NSInteger)selectedRowInComponent:(NSInteger)component;
```


## UIPickerViewDataSource
```objc
// 返回有多少列
- (NSInteger)numberOfComponentsInPickerView:(UIPickerView *)pickerView;

// 返回第component有多少行
- (NSInteger)pickerView:(UIPickerView *)pickerView numberOfRowsInComponent:(NSInteger)component;
```


## UIPickerViewDelegate
```objc
// 返回第component列多宽
- (CGFloat)pickerView:(UIPickerView *)pickerView widthForComponent:(NSInteger)component

// 返回第component列多高
- (CGFloat)pickerView:(UIPickerView *)pickerView rowHeightForComponent:(NSInteger)component

// 返回第component列第row行标题
- (NSString *)pickerView:(UIPickerView *)pickerView titleForRow:(NSInteger)row forComponent:(NSInteger)component

// NSAttributedString:富文本,可以描述文本的外观属性,颜色,字体,阴影,空心,图文混排
//- (NSAttributedString *)pickerView:(UIPickerView *)pickerView attributedTitleForRow:(NSInteger)row forComponent:(NSInteger)component

// 返回第component列第row行视图控件
- (UIView *)pickerView:(UIPickerView *)pickerView viewForRow:(NSInteger)row forComponent:(NSInteger)component reusingView:(UIView *)view

// 当用户选中某一行的时候调用
// 选中第component列第row行的时候调用
// 可以监听pickerView滚动
- (void)pickerView:(UIPickerView *)pickerView didSelectRow:(NSInteger)row inComponent:(NSInteger)component

```
