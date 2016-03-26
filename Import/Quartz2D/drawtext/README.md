# drawText（绘制文本）

### 通过NSString自身方法绘制文本步骤
1. 新建一个类，继承自UIView（略）

2. 在-(void)drawRect:(CGRect)rect方法实现下述几步（略）

- 加载文字

- 设置文本属性

- 通过NSString自身方法绘制文本显示到view上面

```objc
- (void)drawText
{
## 加载文字
    NSString *str = @"hello world hello world hello world";

## 设置文本属性
    // Attributes:给文字添加属性,富文本,字体,颜色,空心,阴影
    // 用一个字典去描述文本属性
    NSMutableDictionary *strAttr = [NSMutableDictionary dictionary];
    // 字体
    strAttr[NSFontAttributeName] = [UIFont systemFontOfSize:50];
    // 颜色
    strAttr[NSForegroundColorAttributeName] = [UIColor redColor];
    // 边线宽度
    strAttr[NSStrokeWidthAttributeName] = @2;
    //边线颜色
    //    strAttr[NSStrokeColorAttributeName] = [UIColor greenColor];
    // 设置阴影
    NSShadow *shadow = [[NSShadow alloc] init];
    shadow.shadowOffset = CGSizeMake(10, 10);//阴影偏移
    shadow.shadowColor = [UIColor yellowColor];
    shadow.shadowBlurRadius = 5;//污迹半径
    strAttr[NSShadowAttributeName] = shadow;

##渲染
    // drawAtPoint不会自动换行
    //    [str drawAtPoint:CGPointZero withAttributes:strAttr];

    // drawInRect可以自动换行
    [str drawInRect:self.bounds withAttributes:strAttr];

}
```
