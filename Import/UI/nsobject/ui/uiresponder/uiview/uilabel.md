# UILabel
- UILabel极其常用，功能比较专一：显示文字
- 必须设置frame，text，加入视图
- 要注意UILabel如果不设置frame,还是会有一点小黑点出现在屏幕中的

##UILabel创建步骤

###常用属性
1. text：显示的文字
```objc
@property(nonatomic,copy)   NSString    *text;```

2. font：字体
```objc
@property(nonatomic,retain) UIFont             *font```

3. textColor：文字颜色
```objc
@property(nonatomic,retain) UIColor   *textColor;```

4. textAlignment：对齐模式（比如左对齐、居中对齐、右对齐
```objc
@property(nonatomic)  NSTextAlignment  textAlignment;```

5.  umberOfLines：文字行数
```objc
@property(nonatomic) NSInteger numberOfLines;```

6. lineBreakMode：换行模式
```objc
@property(nonatomic)   NSLineBreakMode lineBreakMode;```


## UILabel实现包裹内容
- storyboard
    - 设置宽度约束为 <= 固定值
    - 设置位置约束
    - 不用去设置高度约束
- 手动实现
```objc
NSDictionary *TEXTAttributes = @{NSFontAttributeName : [UIFont systemFontOfSize:14]};
    CGSize TEXTMaxSize = CGSizeMake(TEXTW, MAXFLOAT);
    CGFloat TEXTH = [self.text boundingRectWithSize:TEXTMaxSize options:NSStringDrawingUsesLineFragmentOrigin attributes:TEXTAttributes context:nil].size.height;```


## UILabel指示器效果
- 第三方控件：MBProgressHUD-master；SVProgressHUD-master
- 指示器、HUD、遮盖、蒙板、弹框
- 半透明的指示器如何实现？
    - 指示器的alpha = 1.0
    - 指示器的背景色是半透明的
```objc
self.hudLbl.alpha = 1;//1=不透明,0=完全透明
self.hudLbl.backgroundColor = [UIColor colorWithRed:1 green:1 blue:0 alpha:0.5];//颜色由三原色组成，不需在渐进式动画中设置```

###UIAlertControllerStyleActionSheet实现指示器（不可以加文本框）
```objc
/**
 *  UIAlertController的使用:UIAlertControllerStyleActionSheet
 */
- (void)useAlertController2
{
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"警告" message:@"确定要删除它？" preferredStyle:UIAlertControllerStyleActionSheet];
    // 添加按钮
    UIAlertAction *sure = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleDestructive handler:^(UIAlertAction *action) {
        NSLog(@"点击了【确定】按钮");
    }];
    [alertController addAction:sure];
    [alertController addAction:[UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction *action) {
        NSLog(@"点击了【取消】按钮");
    }]];
    [alertController addAction:[UIAlertAction actionWithTitle:@"随便" style:UIAlertActionStyleDefault handler:^(UIAlertAction *action) {
        NSLog(@"点击了【随便】按钮");
    }]];
    // 在当前控制器上面弹出另一个控制器：alertController
    [self presentViewController:alertController animated:YES completion:nil];
}```


###UIAlertControllerStyleAlert实现指示器效果
```objc
/**
 *  UIAlertController的使用:UIAlertControllerStyleAlert
 */
- (void)useAlertController
{
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"警告" message:@"确定要删除它？" preferredStyle:UIAlertControllerStyleAlert];

    // 添加按钮
    UIAlertAction *sure = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleDestructive handler:^(UIAlertAction *action) {
        NSLog(@"点击了【确定】按钮");
    }];
    [alertController addAction:sure];

    [alertController addAction:[UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction *action) {
        NSLog(@"点击了【取消】按钮");
    }]];

    // 添加文本框
    [alertController addTextFieldWithConfigurationHandler:^(UITextField *textField) {
        textField.textColor = [UIColor redColor];
        textField.secureTextEntry = YES; // 暗文
        textField.placeholder = @"请输入密码";
    }];

    // 在当前控制器上面弹出另一个控制器：alertController
    [self presentViewController:alertController animated:YES completion:nil];
}```

###UIActionSheet的实现指示器效果
```objc
/**
 *  UIActionSheet的使用
 */
- (void)useActionSheet
{
    UIActionSheet *sheet = [[UIActionSheet alloc] initWithTitle:@"警告：确定要删除它？" delegate:self cancelButtonTitle:@"取消" destructiveButtonTitle:@"确定" otherButtonTitles:@"随便", nil];

    [sheet showInView:self.view];
}

#pragma mark - <UIActionSheetDelegate>
- (void)actionSheet:(UIActionSheet *)actionSheet clickedButtonAtIndex:(NSInteger)buttonIndex
{

}```

###UIAlertView的使用实现指示器效果
```objc
/**
 *  UIAlertView的使用
 */
- (void)useAlertView
{
    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"警告" message:@"是否要删除它？" delegate:self cancelButtonTitle:@"是" otherButtonTitles:@"否", nil];
    alertView.alertViewStyle = UIAlertViewStyleLoginAndPasswordInput;
    [alertView show];
}

#pragma mark - <UIAlertViewDelegate>
- (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex
{
    NSLog(@"点击了第%zd个按钮 - %@", buttonIndex, [alertView textFieldAtIndex:0].text);
}

###自己实现指示器效果
/**
 *  自己写一个指示器
 */
- (void)diyHUD
{
    // 创建遮盖
    UIView *cover = [[UIView alloc] init];
    cover.backgroundColor = [UIColor colorWithRed:0 green:0 blue:0 alpha:0.7];
    cover.bounds = CGRectMake(0, 0, 200, 150);
    cover.layer.cornerRadius = 10;
    cover.center = CGPointMake(self.view.frame.size.width * 0.5, self.view.frame.size.height * 0.5);
    [self.view addSubview:cover];

    // 往遮盖中添加菊花
    UIActivityIndicatorView *loadingView = [[UIActivityIndicatorView alloc] initWithActivityIndicatorStyle:UIActivityIndicatorViewStyleWhiteLarge];
    [loadingView startAnimating];
    loadingView.center = CGPointMake(cover.frame.size.width * 0.5, cover.frame.size.height * 0.5);
    [cover addSubview:loadingView];

    // 往遮盖中添加提醒文字
    UILabel *label = [[UILabel alloc] init];
    label.text = @"正在拼命加载中...";
    label.textAlignment = NSTextAlignmentCenter;
    CGFloat labelH = 70;
    label.textColor = [UIColor whiteColor];
    label.frame = CGRectMake(0, cover.frame.size.height - labelH, cover.frame.size.width, labelH);
    [cover addSubview:label];

    //    [NBHUD show:@"哥正在帮你加载中..."];
    //
    //    [NBHUD hide];
}```

###SVProgressHUD实现指示器效果
```objc
/**
 *  SVProgressHUD
 */
- (void)sv
{
    // 下面这些消息需要主动调用dismiss方法来隐藏
//    [SVProgressHUD show];
//    [SVProgressHUD showWithMaskType:SVProgressHUDMaskTypeBlack]; // 增加灰色蒙板
//    [SVProgressHUD showWithStatus:@"正在加载中..."];


    // 下面这些消息会自动消失
//    [SVProgressHUD showInfoWithStatus:@"数据加载完毕！"];
//    [SVProgressHUD showSuccessWithStatus:@"成功加载到4条新数据！"];
    [SVProgressHUD showErrorWithStatus:@"网络错误，请稍等！"];

    // 延迟2秒后做一些事情
//    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
//        [SVProgressHUD dismiss];
//    });
}```

###MBProgressHUD实现指示器效果
```objc
//计时器配合加载效果
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    [NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(changeProgress) userInfo:nil repeats:YES];
}

double progress = 0.0;
- (void)changeProgress
{
    progress += 0.1;
    [SVProgressHUD showProgress:progress status:[NSString stringWithFormat:@"已下载%.0f%%", progress * 100]];
}

/**
 *  MBProgressHUD
 */
- (void)mb
{
    MBProgressHUD *hud = [MBProgressHUD showHUDAddedTo:self.view animated:YES];
    hud.labelText = @"正在加载中";
    hud.removeFromSuperViewOnHide = YES;

    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        [hud hide:YES];
    });
    //    [hud hide:YES afterDelay:2.0];
}```
