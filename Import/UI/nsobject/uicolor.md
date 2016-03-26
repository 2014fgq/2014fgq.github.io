#UIColor
- 父类是NSObject

##常用方法
1. 直接创建对应的颜色
```objc
+(UIColor *)blackColor;      // 0.0 white
+(UIColor *)darkGrayColor;   // 0.333 white
+(UIColor *)lightGrayColor;  // 0.667 white
+(UIColor *)whiteColor;      // 1.0 white
+(UIColor *)grayColor;       // 0.5 white
+(UIColor *)redColor;        // 1.0, 0.0, 0.0 RGB
+(UIColor *)greenColor;      // 0.0, 1.0, 0.0 RGB
+(UIColor *)blueColor;       // 0.0, 0.0, 1.0 RGB
+(UIColor *)cyanColor;       // 0.0, 1.0, 1.0 RGB
+(UIColor *)yellowColor;     // 1.0, 1.0, 0.0 RGB
+(UIColor *)magentaColor;    // 1.0, 0.0, 1.0 RGB
+(UIColor *)orangeColor;     // 1.0, 0.5, 0.0 RGB
+(UIColor *)purpleColor;     // 0.5, 0.0, 0.5 RGB
+(UIColor *)brownColor;      // 0.6, 0.4, 0.2 RGB
+(UIColor *)clearColor;      // 透明色```

2. 根据RGB组合创建颜色
//根据三原色设置颜色，并设置透明度，数值从0-1
```objc
+(UIColor *)colorWithRed:(CGFloat)red green:(CGFloat)green blue:(CGFloat)blue alpha:(CGFloat)alpha;```
