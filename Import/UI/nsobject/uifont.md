# UIFont
- 父类是NSObject
- UIFont代表字体，常见创建方法有以下几个：

-  系统默认字体（参数是多少号字）
```objc
+ (UIFont *)systemFontOfSize:(CGFloat)fontSize; ```

-  粗体（粗体是字体的一种，参数是多少号字）
```objc
+ (UIFont *)boldSystemFontOfSize:(CGFloat)fontSize; ```

-  斜体（斜体是字体的一种，参数是多少号字）
    - 斜体效果没有，不知道是不是苹果已经失效了
```objc
+ (UIFont *)italicSystemFontOfSize:(CGFloat)fontSize; ```


