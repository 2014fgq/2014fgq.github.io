# UITextField

##常见属性
```objc
//设置文本框左面控件
self.textField.leftView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 8, 0)];
//设置文本框左面控件的模式
self.textField.leftViewMode = UITextFieldViewModeAlways;

//边缘属性（设置为圆形边框）
    tf.borderStyle = UITextBorderStyleRoundedRect;```



##常见用法
```objc
    //文本框编辑时调用，调用textChange方法
    UITextField *tf = [[UITextField alloc] init];
    [tf addTarget:self action:@selector(textChange:) forControlEvents:UIControlEventEditingChanged];```



##UITextFieldDelegate协议

```objc
//文本框点击时调用，表示是否允许编辑，NO表示不允许编辑
- (BOOL)textFieldShouldBeginEditing:(UITextField *)textField;

//文本框点击时调用，让textField成为第一响应者，开始编辑
- (void)textFieldDidBeginEditing:(UITextField *)textField;

//文本框编辑时调用，YES允许输入，NO禁止输入。可用if设置那些允许输入，哪些不允许输入。
//可用于拦截用户输入
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string;

//键盘return键点击时调用。YES结束输入(继续输入的内容是独立的），NO则无效果（继续输入的与之前内容是一个整体）
    -收回键盘重新打开时与return NO一致，所以设置了return YES和收回键盘与return NO和收回键盘效果一致
- (BOOL)textFieldShouldReturn:(UITextField *)textField{
    if (textField == self.nameField) {
        // 让phoneField成为第一响应者（phoneField会出聚焦）
        [self.phoneField becomeFirstResponder];
    } else if (textField == self.phoneField) {
        [self.addressField becomeFirstResponder];
    } else if (textField == self.addressField) {
        [self.view endEditing:YES];
    }
    return YES;
}

//是否结束编辑文本框，YES表示结束，NO表示不结束
- (BOOL)textFieldShouldEndEditing:(UITextField *)textField;

//文本框结束编辑时调用（强迫停止也会调用）。
- (void)textFieldDidEndEditing:(UITextField *)textField;

//清除按钮点击时调用。NO忽略清除命令
- (BOOL)textFieldShouldClear:(UITextField *)textField;```




##textField与键盘
- 注意：如果一个键盘想要弹出来,必须把textField添加到一个控件上

```objc
    //设置键盘(自定义键盘）
UIView *temp = [[UIView alloc] init];
temp.frame = CGRectMake(0, 0, 0, 300);
temp.backgroundColor = [UIColor redColor];
self.nameField.inputView = temp;

    //收回键盘（该方法在tableView中失效）
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    // 文本框不再是第一响应者，就会退出键盘
//  [self.textField resignFirstResponder];

//  [self.textField endEditing:YES];

    [self.view endEditing:YES];
}
    // 设置键盘的辅助控件
    //增加按钮
self.nameField.inputAccessoryView = [UIButton buttonWithType:UIButtonTypeContactAdd];

    //开关按钮
self.phoneField.inputAccessoryView = [[UISwitch alloc] init];

    //分段按钮
self.addressField.inputAccessoryView = [[UISegmentedControl alloc] initWithItems:@[@"1", @"2"]];

    //加载toolbar
    XMGToolbar *toolbar = [[[NSBundle mainBundle] loadNibNamed:@"XMGToolbar" owner:nil options:nil] lastObject];

    //设置文本框辅助控件
    self.nameField.inputAccessoryView = toolbar;
    self.phoneField.inputAccessoryView = toolbar;
    self.addressField.inputAccessoryView = toolbar;```



###toolbar
- 自行设置位置，不需要关心位置
- 可以放置弹簧等工具，更好的适配位置（弹簧不会显示）




##第三方框架
- 第三方控件：IQKeyboardManager-master
- 应用示例
```objc
// 键盘管理者
    IQKeyboardManager *mgr = [IQKeyboardManager sharedManager];
    // 当点击键盘以外的区域，会自动退出键盘
    mgr.shouldResignOnTouchOutside = YES;

    // 不要在toolbar上面显示占位文字
    mgr.shouldShowTextFieldPlaceholder = NO;

    // 关闭toolbar
//    mgr.enableAutoToolbar = NO;
    // toolbar是否要使用文本框的tintColor(光标的颜色)
    mgr.shouldToolbarUsesTextFieldTintColor = YES;

    // 管理return key
    self.handler = [[IQKeyboardReturnKeyHandler alloc] initWithViewController:self];
    // 设置最后一个文本框的return key为“完成”
    self.handler.lastTextFieldReturnKeyType = UIReturnKeyDone;```
