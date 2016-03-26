# UI

## storyboard文件的认识
- 作用：描述软件界面
- 程序启动的简单过程
    - 程序一启动，就会加载`Main.storyboard`文件
    - 会创建箭头所指的控制器，并且显示控制器所管理的软件界面
- 配置程序一启动就会加载的storyboard文件
![截图](Images/storyboard启动设置.png)


## 类扩展(Class Extension)
- 作用
    - 能为某个类增加额外的属性、成员变量、方法声明
    - 一般将类扩展写到.m文件中
    - `一般将一些私有的属性写到类扩展`
- 使用格式

```objc
@interface 类名()
/* 属性、成员变量、方法声明 */
@end
```
- 与分类的区别
    - 分类的小括号必须有名字
    ```objc
    @interface 类名(分类名字)
    /* 方法声明 */
    @end

    @implementation 类名(分类名字)
    /* 方法实现 */
    @end
    ```
    - 分类只能扩充方法
    - 如果在分类中声明了一个属性，分类只会生成这个属性的get\set方法声明

## 项目的常见属性
- Product Name
    - 产品名称
    - 项目名称
    - 软件名称
- Organization Name
    - 公司名称
- Organization Identifier
    - 公司的唯一标识
    - 一般用网站域名的反写形式
- Bundle Identifier
    - 软件的唯一标识
    - 默认 == Organization Identifier + Product Name


##UIkit坐标系
- 在UIKit中，坐标系的原点(0，0)在左上角，x值向右正向延伸，y值向下正向延伸

![UIkit坐标系](Images/UIKit坐标系.png)

###NSArray语法
- 数组中各元素执行某方法
```objc
    [self.scrollView.subviews makeObjectsPerformSelector:@selector(removeFromSuperview)];```

##资源管理
- 添加外界的代码\资源到本项目中，建议的设置选项
![](Images/Snip20150617_284.png)

- 查看从外界加进来的代码\资源，有没有打包到本项目
![](Images/Snip20150617_288.png)


- 延迟做一些事情
- NSObject提供的对象方法
```objc
[abc performSelector:@selector(stand:) withObject:@"123" afterDelay:10];
// 10s后自动调用abc的stand:方法，并且传递@"123"参数
```


