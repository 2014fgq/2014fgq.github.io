# NSObject

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


- 延迟做一些事情
- NSObject提供的对象方法
```objc
[abc performSelector:@selector(stand:) withObject:@"123" afterDelay:10];
// 10s后自动调用abc的stand:方法，并且传递@"123"参数
```

###NSArray语法
- 数组中各元素执行某方法
```objc
    [self.scrollView.subviews makeObjectsPerformSelector:@selector(removeFromSuperview)];```

## @property的使用策略
- assign
    - `基本数据类型`、`枚举`、`结构体`等非OC对象类型
- weak
    - OC对象类型（比如NSArray、NSDate、NSNumber、模型类）
- strong
    - OC对象类型（比如NSArray、NSDate、NSNumber、模型类）
    - 一个对象只要有强指针引用着，就不会被销毁
- copy
    - 一般用在`NSString`、`block`类型上

## 拷贝(copy)
- 原理：拷贝原对象产生副本对象，原对象修改不会影响副本对象，副本对象修改不会影响原对象
- 实现拷贝的方法有2个
    - copy：返回不可变副本
    - mutableCopy：返回可变副本
- 普通对象实现拷贝的步骤
    - 遵守NSCopying协议
    - 实现-copyWithZone:方法(是可变还是不可变看如何实现-copyWithZone:）
        - 创建新对象
        - 给新对象的属性赋值
- 深拷贝与浅拷贝
    - 深拷贝：产生一个新对象
    - 浅拷贝：没有产生一个新对象，只是指针指向了原对象，按拷贝的原理，不应该出现浅拷贝，这是因为苹果考虑到，对于不可变对象的copy,没必要产生新的对象，也能达到原对象不影响副本对象（因为都是不可变的）




三.`函数介绍`:

NSStringFromClass:根据一个类名生成一个类名字符串

NSClassFromString: 根据一个类名字符串生成一个类名

__func__ 获取当前方法是哪个类的方法,不一定在真正实现方法的类中使用，可能是显示调用父类的方法（如果子类没有重写方法）

##单例
1. 整个应用程序只有一份内存.
2. 重写alloc方法,只分配一次.
3. 提供share方法,获取单例对象.
4. 使用静态全局变量保存单例对象.
