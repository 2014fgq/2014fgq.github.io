##MVC
- MVC是一种设计思想，贯穿于整个iOS开发中
- MVC中的三个角色
    - M：Model，模型数据
        - ## 第三方框架（模型）
         - JSONModel：所有模型都必须继承自JSONModel
         - Mantle：所有模型都必须继承自MTModel
         - MJExtension
    - V：View，视图（界面）
    - C：Control，控制中心
- MVC的几个明显的特征和体现：
    - View上面显示什么东西，取决于Model
    - 只要Model数据改了，View的显示状态会跟着更改
    - Control负责初始化Model，并将Model传递给View去解析展示

##封装的思路：
- 抽取重复代码
    - 将相同代码放到一个新的方法中
    - 不用的东西就变成方法的参数

##模型
- 概念
    - 专门用来`存放数据`的对象
- 特点
    - 一般直接继承自NSObject
    - 在.h文件中声明一些用来存放数据的属性
    - 保证数据的正确性使用模型访问属性时，编译器会提供一系列的提示，提高编码效率
- 模型定义示例
```objc
@interface Shop : NSObject
/** 名字 */
@property (nonatomic, strong) NSString *name;
/** 图标 */
@property (nonatomic, strong) NSString *icon;
@end
```
- 字典转模型示例
```objc
Shop *shop = [[Shop alloc] init];
shop.name = dict[@"name"];
shop.icon = dict[@"icon"];
```
- 字典转模型的过程最好封装在模型内部
- 模型应该提供一个可以传入字典参数的构造方法
```objc
-(instancetype)initWithDict:(NSDictionary *)dict;
+(instancetype)xxxWithDict:(NSDictionary *)dict;```


## iOS监听某些事件的方法
- 通知（NSNotificationCenter\NSNotification，详见NSNotificationCenter章节）
    - 任何对象之间都可以传递消息
    - 使用范围
        - 1个对象可以发通知给N个对象
        - 1个对象可以接受N个对象发出的通知
    - 必须得保证通知的名字在发出和监听时是一致的
- KVO
    - 仅仅是能监听对象属性的改变（灵活度不如通知和代理）
- 代理
    - 使用范围
        - 1个对象只能设置一个代理(假设这个对象只有1个代理属性)
        - 1个对象能成为多个对象的代理
    - 比`通知`规范
    - 建议使用`代理`多于`通知`



###通知和代理的选择
共同点
利用通知和代理都能完成对象之间的通信
(比如A对象告诉D对象发生了什么事情, A对象传递数据给D对象)

不同点
代理 : 1个对象只能告诉另1个对象发生了什么事情
通知 : 1个对象能告诉N个对象发生了什么事情, 1个对象能得知N个对象发生了什么事情


## 代理的使用步骤
- 定义一份代理协议
    - 协议名字的格式一般是：类名 + Delegate
        - 比如UITableViewDelegate
    - 代理方法细节
        - 一般都是@optional
        - 方法名一般都以类名开头
            - 比如`-(void)scrollViewDidScroll:`
        - 代理对象一般是`控制器对象`
        - 一般都需要将对象本身传出去（对象本身作为参数传递）
    - 必须要遵守NSObject协议
        - 比如`@protocol XMGWineCellDelegate <NSObject>`
- 声明一个代理属性，`弱指针`
    - 代理的类型格式：id<协议> delegate

```objc
@property (nonatomic, weak) id<XMGWineCellDelegate> delegate;
```
- 设置代理对象

```objc
xxx.delegate = yyy;
```

- 代理对象遵守协议，实现协议里面相应的方法

- 当控件内部发生了一些事情，就可以调用代理的代理方法通知代理
    - 如果代理方法是@optional，那么需要判断方法是否有实现

```objc
if ([self.delegate respondsToSelector:@selector(wineCellDidClickPlusButton:)]) {
    [self.delegate wineCellDidClickPlusButton:self];
}
```
