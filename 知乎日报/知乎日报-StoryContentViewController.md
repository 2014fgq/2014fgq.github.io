# 知乎日报-StoryContentViewController

## 包含文件

* StoryContentViewController.h, StoryContentViewController.m

## 1、获取数据，绑定数据源

```
- (instancetype)initWithViewModel:(StoryContentViewModel *)vm {
    self = [super init];
    if (self) {
        _viewmodel = vm;
        [_viewmodel addObserver:self forKeyPath:@"storyModel" options:NSKeyValueObservingOptionNew context:nil];
        [_viewmodel getStoryContentWithStoryID:_viewmodel.loadedStoryID];
    }
    return self;
}

- (void)dealloc {
    [_viewmodel removeObserver:self forKeyPath:@"storyModel"];
}
```

## 2、在viewDidLoad里面，实现子控件的创建

```
- (void)initSubViews {
    
    self.automaticallyAdjustsScrollViewInsets = NO;
    
    _webView = [[UIWebView alloc] initWithFrame:CGRectMake(0, 20, kScreenWidth, kScreenHeight-20-43)];
    _webView.scrollView.delegate = self;
    _webView.backgroundColor = [UIColor whiteColor];
    [self.view addSubview:_webView];

    _headerView = [[UIView alloc] initWithFrame:CGRectMake(0, -40, kScreenWidth, 260)];
    _headerView.clipsToBounds = YES;
    [self.view addSubview:_headerView];

    _imageView = [[UIImageView alloc] initWithFrame:CGRectMake(0.f, 0.f, kScreenWidth, 300.f)];
    _imageView.contentMode = UIViewContentModeScaleAspectFill;
    [_headerView addSubview:_imageView];
    
    _titleLab = [[UILabel alloc] initWithFrame:CGRectZero];
    _titleLab.numberOfLines = 0;
    [_headerView addSubview:_titleLab];
    
    _imaSourceLab = [[UILabel alloc] initWithFrame:CGRectMake(10, 240, kScreenWidth-20, 20)];
    _imaSourceLab.textAlignment = NSTextAlignmentRight;
    _imaSourceLab.font = [UIFont systemFontOfSize:12];
    _imaSourceLab.textColor = [UIColor whiteColor];
    [_headerView addSubview:_imaSourceLab];
    
    UIView *toolBar = [[UIView alloc] initWithFrame:CGRectMake(0.f, kScreenHeight-43, kScreenWidth, 43)];
    
    UIButton *backBtn = [[UIButton alloc] initWithFrame:CGRectMake(0, 0, kScreenWidth/5, 43)];
    [backBtn setImage:[UIImage imageNamed:@"News_Navigation_Arrow"] forState:UIControlStateNormal];
    [backBtn addTarget:self action:@selector(backAction:) forControlEvents:UIControlEventTouchUpInside];
    [toolBar addSubview:backBtn];
    
    UIButton *nextBtn = [[UIButton alloc] initWithFrame:CGRectMake(kScreenWidth/5,0 ,kScreenWidth/5 , 43)];
    [nextBtn setImage:[UIImage imageNamed:@"News_Navigation_Next"] forState:UIControlStateNormal];
    [nextBtn addTarget:self action:@selector(nextStoryAction:) forControlEvents:UIControlEventTouchUpInside];
    [toolBar addSubview:nextBtn];
    
    UIButton *votedBtn = [[UIButton alloc] initWithFrame:CGRectMake((kScreenWidth/5)*2, 0 ,kScreenWidth/5 , 43)];
    [votedBtn setImage:[UIImage imageNamed:@"News_Navigation_Voted"] forState:UIControlStateNormal];
    [toolBar addSubview:votedBtn];
    
    UIButton *sharedBtn = [[UIButton alloc] initWithFrame:CGRectMake((kScreenWidth/5)*3, 0 ,kScreenWidth/5 , 43)];
    [sharedBtn setImage:[UIImage imageNamed:@"News_Navigation_Share"] forState:UIControlStateNormal];
    [toolBar addSubview:sharedBtn];
    
    UIButton *commentdBtn = [[UIButton alloc] initWithFrame:CGRectMake((kScreenWidth/5)*4, 0 ,kScreenWidth/5 , 43)];
    [commentdBtn setImage:[UIImage imageNamed:@"News_Navigation_Comment"] forState:UIControlStateNormal];
    [toolBar addSubview:commentdBtn];
    
    [self.view addSubview:toolBar];
    
    _preView = [[PreView alloc] initWithFrame:kScreenBounds];
    [self.view addSubview:_preView];
}
```

## 3、KVO更新数据

```
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSString *,id> *)change context:(void *)context {
    if ([keyPath isEqualToString:@"storyModel"]) {
        [_imageView sd_setImageWithURL:[NSURL URLWithString:_viewmodel.imageURLString]];
        CGSize size = [_viewmodel.titleAttText boundingRectWithSize:CGSizeMake(kScreenWidth-30, 60) options:NSStringDrawingUsesLineFragmentOrigin|NSStringDrawingUsesFontLeading context:nil].size;
        _titleLab.frame = CGRectMake(15, _headerView.frame.size.height-20-size.height, kScreenWidth-30, size.height);
        _titleLab.attributedText = _viewmodel.titleAttText;
        _imaSourceLab.text = _viewmodel.imaSourceText;
        [_webView loadHTMLString:_viewmodel.htmlStr baseURL:nil];
        dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
            [_preView removeFromSuperview];
        });
    }
}
```

## 4、按键和滚动功能实现数据更新

```
- (void)nextStoryAction:(id)sender {
    [_viewmodel getNextStoryContent];
}


- (void)scrollViewDidScroll:(UIScrollView *)scrollView {

    CGFloat offSetY = scrollView.contentOffset.y;
    if (-offSetY<=80&&-offSetY>=0) {
        _headerView.frame = CGRectMake(0, -40-offSetY/2, kScreenWidth, 260-offSetY/2);
        [_imaSourceLab setTop:240-offSetY/2];
        [_titleLab setBottom:_imaSourceLab.bottom-20];
        if (-offSetY>40&&!_webView.scrollView.isDragging){
            [self.viewmodel getPreviousStoryContent];
        }
    }else if (-offSetY>80) {
        _webView.scrollView.contentOffset = CGPointMake(0, -80);
    }else if (offSetY <=300 ){
        _headerView.frame = CGRectMake(0, -40-offSetY, kScreenWidth, 260);
    }
    if (offSetY + kScreenHeight > scrollView.contentSize.height + 160&&!_webView.scrollView.isDragging) {
        [self.viewmodel getNextStoryContent];
    }
}
```

## 5、按钮实现返回上级目录

```
- (void)backAction:(id)sender {
    [self.navigationController popViewControllerAnimated:YES];
}
```