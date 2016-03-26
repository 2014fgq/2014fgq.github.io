# UIDatePicker
- 父类是UIControl
- 使用场景：当用户选择日期的时候弹出UIDatePicker给用户选择。
- UIDatePickerios6和ios7的区别
![](UIDatePickerios6和ios7.png)


### UIDatePicker的类型--UIDatePickerMode
```objc
//时间模式：显示时分（PM/AM)，例如下午6：53.显示为（6 | 53 | PM）
datePicker.datePickerMode =UIDatePickerModeTime；

//日期模式：显示年月日，例如2007年11月15日.显示为（November | 15 | 2007）
datePicker.datePickerMode = UIDatePickerModeDate；

//日期时间模式（全模式）
datePicker.datePickerMode = UIDatePickerModeDateAndTime；
// Displays date, hour, minute, and optionally AM/PM designation depending on the locale setting (e.g. Wed Nov 15 | 6 | 53 | PM)

//倒数计秒模式，例如1小时53分，显示为（1 | 53）
datePicker.datePickerMode = UIDatePickerModeCountDownTimer；
```



## UIDatePicker一般用法
```objc
    // 创建一个UIDatePicker
    UIDatePicker *datePicker = [[UIDatePicker alloc] init];

    // 设置日期模型
    datePicker.datePickerMode = UIDatePickerModeDate;

    // 设置地区,zh:中国
    datePicker.locale = [NSLocale localeWithLocaleIdentifier:@"zh"];

    // 监听UIDatePicker的选中的日期
    [datePicker addTarget:self action:@selector(dateChange:) forControlEvents:UIControlEventValueChanged];
```

## UIDatePicker所有属性
```objc
@property (nonatomic) UIDatePickerMode datePickerMode; // default is UIDatePickerModeDateAndTime

@property (nullable, nonatomic, strong) NSLocale   *locale;   // default is [NSLocale currentLocale]. setting nil returns to default

@property (null_resettable, nonatomic, copy)   NSCalendar *calendar; // default is [NSCalendar currentCalendar]. setting nil returns to default

@property (nullable, nonatomic, strong) NSTimeZone *timeZone; // default is nil. use current time zone or time zone from calendar

@property (nonatomic, strong) NSDate *date;        // default is current date when picker created. Ignored in countdown timer mode. for that mode, picker starts at 0:00

@property (nullable, nonatomic, strong) NSDate *minimumDate; // specify min/max date range. default is nil. When min > max, the values are ignored. Ignored in countdown timer mode

@property (nullable, nonatomic, strong) NSDate *maximumDate; // default is nil

@property (nonatomic) NSTimeInterval countDownDuration; // for UIDatePickerModeCountDownTimer, ignored otherwise. default is 0.0. limit is 23:59 (86,399 seconds). value being set is div 60 (drops remaining seconds).

@property (nonatomic) NSInteger      minuteInterval;    // display minutes wheel with interval. interval must be evenly divided into 60. default is 1. min is 1, max is 30
```

## UIDatePicker唯一方法
```objc
//动画设置日期
- (void)setDate:(NSDate *)date animated:(BOOL)animated; // if animated is YES, animate the wheels of time to display the new date
```
