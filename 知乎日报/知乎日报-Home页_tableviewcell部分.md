# 知乎日报-Home-tableviewcell部分

## 1、包含文件

* HomeViewController.h, HomeViewController.m
* HomeViewModel.h, HomeViewModel.m
* StoryModel.h, StoryModel.m
* CarouseView.h, CarouseView.m
* HomeViewCell.h, HomeViewCell.m
* SectionTitleView.h, SectionTitleView.m
* RefreshView.h, RefreshView.m
* TopStoryView.h, TopStoryView.m

## 2、数据刷新

```
- (instancetype)initWithViewModel:(HomeViewModel *)vm {
    self = [super init];
    if (self) {
        self.viewmodel = vm;
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(loadingLatestDaily:) name:@"LoadLatestDaily" object:nil];
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(loadingPreviousDaily:) name:@"LoadPreviousDaily" object:nil];
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(updateLatestDaily:) name:@"UpdateLatestDaily" object:nil];
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(mainScrollViewToTop:) name:@"TapStatusBar" object:nil];
        [self.viewmodel getLatestStories];
    }
    return self;
}
```

## 3、UI加载
* 创建UITableView，frame为（0, 20, 屏幕宽度， 屏幕高度-20）, 其中**20**是系统状态栏高度

```
- (void)initSubViews {
    float tvOffset = 20.f;//20.f;
    UITableView *tv = [[UITableView alloc] initWithFrame:CGRectMake(0.f, tvOffset, kScreenWidth, kScreenHeight-tvOffset)];
    tv.delegate = self;
    tv.dataSource = self;
    tv.rowHeight = kRowHeight;
    tv.tableHeaderView = [[UIView alloc] initWithFrame:CGRectMake(0.f, 0.f, kScreenWidth, 200.f)];
    [self.view addSubview:tv];
    _mainTableView = tv;
    
    //滚动视图(CarouseView) 的创建加载
    //仿navigationview 的创建加载
    //"今日新闻"        的创建加载
    //RefreshView      的创建加载
    //menuBtn          的创建加载
    
    [_mainTableView registerNib:[UINib nibWithNibName:@"HomeViewCell" bundle:[NSBundle mainBundle]] forCellReuseIdentifier:@"HomeViewCell"];
    [_mainTableView registerClass:[SectionTitleView class] forHeaderFooterViewReuseIdentifier:NSStringFromClass([SectionTitleView class])];
}
```

## 4、自定义UITableViewCell

```
#pragma mark - UITableViewDataSource
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return [self.viewmodel numberOfSections];
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return [self.viewmodel numberOfRowsInSection:section];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    static NSString *identifier = @"HomeViewCell";
    HomeViewCell *cell = [tableView dequeueReusableCellWithIdentifier:identifier];
    cell.selectionStyle = UITableViewCellSelectionStyleNone;
    StoryModel *story = [self.viewmodel storyAtIndexPath:indexPath];
    [cell setStoryModel:story];
    return cell;
}
```

## 5、[自定义headerview](./知乎日报-Home-HeaderView.md)
