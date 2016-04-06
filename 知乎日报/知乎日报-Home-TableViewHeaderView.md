# 知乎日报-Home-TableViewHeaderView

## 1、创建图片滚动控件(CarouseView)

```
- (void)initSubViews {
    //其他创建UITableView的代码
    tv.tableHeaderView = [[UIView alloc] initWithFrame:CGRectMake(0.f, 0.f, kScreenWidth, 200.f)];
  
    //创建其他控件
  
    CarouseView *cv = [[CarouseView alloc] initWithFrame:CGRectMake(0.f, -40.f, kScreenWidth, 260.f)];
    cv.delegate = self;
    cv.clipsToBounds = YES;
    [self.view addSubview:cv];
    _carouseView = cv;
}
```

## 2、滚动时，调整_carouseView的frame

* 向下拉时，调整_carouseView的y值和高度，并调整内部子控件的Y值
* 向上拉时，调整_carouseView的frame,因为_carouseView不是UIableView的子控件，不会自己滚动
> 建议将下面三句话, 一句一句的注释掉，然后看看效果

```
#pragma mark - UIScrollViewDelegate

- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    
    if ([scrollView isEqual:_mainTableView]) {
        CGFloat offSetY = scrollView.contentOffset.y;
        if (offSetY<=0&&offSetY>=-80) {
            //其他操作
            _carouseView.frame = CGRectMake(0, -40-offSetY/2, kScreenWidth, 260-offSetY/2);
            [_carouseView updateSubViewsOriginY:offSetY];
        }else if(offSetY<-80){
            //其他操作
        }else if(offSetY <= 300) {
            //其他操作
            _carouseView.frame = CGRectMake(0, -40-offSetY, kScreenWidth, 260);
        }
        
        //其他操作
    }
}
```

