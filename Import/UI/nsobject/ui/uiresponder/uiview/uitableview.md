# UITableView
- UITableView需要一个数据源(dataSource)来显示数据
- 父类是UIScrollView,可以滚动，性能极佳

- UITableView会向数据源（遵守UITableViewDataSource协议的OC对象）查询一共有多少行数据以及每一行显示什么数据等

- 没有设置数据源的UITableView只是个空壳


## 如何让tableView展示数据
- 设置数据源对象

```objc
self.tableView.dataSource = self;
```

- 数据源对象要遵守协议

```objc
@interface ViewController () <UITableViewDataSource>

@end
```

- 实现数据源方法

```objc
// 多少组数据
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView;

// 每一组有多少行数据
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;

// 每一行显示什么内容
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath;

// 每一组的头部
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section;

// 每一组的尾部
- (NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section

/** 返回头部高度 */
-(CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
{
    return 50;
}

/** 返回脚部高度 */
-(CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
{
    return 50;
}```


## tableView的常见设置
```objc
// 设置每一行cell的统一高度
self.tableView.rowHeight = 100;

// 设置每一组头部的统一高度
self.tableView.sectionHeaderHeight = 50;

// 设置每一组尾部的统一高度
self.tableView.sectionFooterHeight = 50;

// 设置分割线颜色
self.tableView.separatorColor = [UIColor redColor];
// 设置分割线样式（默认是一条线，但宽度不是整个屏幕的宽度）
self.tableView.separatorStyle = UITableViewCellSeparatorStyleNone;//不显示分隔线；
// 设置表头控件
self.tableView.tableHeaderView = [[UISwitch alloc] init];
// 设置表尾控件
self.tableView.tableFooterView = [UIButton buttonWithType:UIButtonTypeContactAdd];

// 设置右边索引文字的颜色
self.tableView.sectionIndexColor = [UIColor redColor];
// 设置右边索引文字的背景色
self.tableView.sectionIndexBackgroundColor = [UIColor blackColor];
```


##常见方法
1.创建索引条
```objc
/**
 *  创建索引条
 *
 *  @param tableView 需要增加索引条的tableView
 *
 *  @return 索引条名字数组
 */
-(NSArray<NSString *> *)sectionIndexTitlesForTableView:(UITableView *)tableView
{
    //通过KVC查找key为“header”的值组成一个数组返回
    return [self.carGroup valueForKeyPath:@"header"];
}```


## 数据刷新
- 添加数据
- 删除数据
- 更改数据

## 全局刷新方法(最常用)
```objc
[self.tableView reloadData];
// 屏幕上的所有可视的cell都会刷新一遍
```

## 局部刷新方法（要先修改Model中的数据再刷新）
- 添加数据

```objc
NSArray *indexPaths = @[
                        [NSIndexPath indexPathForRow:0 inSection:0],
                        [NSIndexPath indexPathForRow:1 inSection:0]
                        ];
[self.tableView insertRowsAtIndexPaths:indexPaths withRowAnimation:UITableViewRowAnimationRight];
```

- 删除数据

```objc
NSArray *indexPaths = @[
                        [NSIndexPath indexPathForRow:0 inSection:0],
                        [NSIndexPath indexPathForRow:1 inSection:0]
                        ];
[self.tableView deleteRowsAtIndexPaths:indexPaths withRowAnimation:UITableViewRowAnimationMiddle];
```

- 更新数据（没有添加和删除数据，仅仅是修改已经存在的数据）

```objc
NSArray *indexPaths = @[
                        [NSIndexPath indexPathForRow:0 inSection:0],
                        [NSIndexPath indexPathForRow:1 inSection:0]
                        ];
[self.tableView relaodRowsAtIndexPaths:indexPaths withRowAnimation:UITableViewRowAnimationMiddle];
```

## 左滑出现删除按钮
- 需要实现tableView的代理方法

```objc
/**
 *  只要实现了这个方法，左滑出现Delete按钮的功能就有了
 *  点击了“左滑出现的Delete按钮”会调用这个方法
 */
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 删除模型
    [self.wineArray removeObjectAtIndex:indexPath.row];

    // 刷新
    [tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationLeft];
}

/**
 *  修改Delete按钮文字为“删除”
 */
- (NSString *)tableView:(UITableView *)tableView titleForDeleteConfirmationButtonForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return @"删除";
}
```

## 左滑出现N个按钮
- 需要实现tableView的代理方法

```objc
/**
 *  只要实现了这个方法，左滑出现按钮的功能就有了
 (一旦左滑出现了N个按钮，tableView就进入了编辑模式, tableView.editing = YES)
 */
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
{

}

/**
 *  左滑cell时出现什么按钮
 */
- (NSArray *)tableView:(UITableView *)tableView editActionsForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewRowAction *action0 = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"关注" handler:^(UITableViewRowAction *action, NSIndexPath *indexPath) {
        NSLog(@"点击了关注");

        // 收回左滑出现的按钮(退出编辑模式)
        tableView.editing = NO;
    }];

    UITableViewRowAction *action1 = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleDefault title:@"删除" handler:^(UITableViewRowAction *action, NSIndexPath *indexPath) {
        [self.wineArray removeObjectAtIndex:indexPath.row];
        [tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationAutomatic];
    }];

    return @[action1, action0];
}
```

## 进入编辑模式
```objc
// self.tabelView.editing = YES;
[self.tableView setEditing:YES animated:YES];
// 默认情况下，进入编辑模式时，左边会出现一排红色的“减号”按钮
```

## 在编辑模式中多选
```objc
// 编辑模式的时候可以多选
self.tableView.allowsMultipleSelectionDuringEditing = YES;
// 进入编辑模式
[self.tableView setEditing:YES animated:YES];

// 获得选中的所有行
self.tableView.indexPathsForSelectedRows;
```
