# UIPageControl
- 父类是UIControl
- 配合scrollview分页使用

##常用属性
1. numberOfPages：一共有多少页
```objc
@property(nonatomic) NSInteger numberOfPages;```

- currentPage：当前显示的页码
```objc
@property(nonatomic) NSInteger currentPage;```

- hidesForSinglePage：只有一页时，是否需要隐藏页码指示器
```objc
@property(nonatomic) BOOL hidesForSinglePage;```

- pageIndicatorTintColor：其他页码指示器的颜色
```objc
@property(nonatomic,retain) UIColor *pageIndicatorTintColor;```

- currentPageIndicatorTintColor：当前页码指示器的颜色
```objc
@property(nonatomic,retain) UIColor *currentPageIndicatorTintColor;```

- currentPageIndicatorTintColor：当前页码指示器的颜色
```objc
@property(nonatomic,retain) UIColor *currentPageIndicatorTintColor;```

###通过KVC访问系统私有属性
```objc
// 设置页码图片
    [self.pageControl setValue:[UIImage imageNamed:@"current"] forKeyPath:@"currentPageImage"];
    [self.pageControl setValue:[UIImage imageNamed:@"other"] forKeyPath:@"_pageImage"];```

