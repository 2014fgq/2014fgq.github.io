# KVC
## KVC
- NSObject分类（NSKeyValueCoding）方法，NSArray,NSSet等也创建了这个分类
- 全称：Key Value Coding（键值编码）
- 赋值

```objc
// 能修改私有成员变量
- (void)setValue:(id)value forKey:(NSString *)key;
- (void)setValue:(id)value forKeyPath:(NSString *)keyPath;
- (void)setValuesForKeysWithDictionary:(NSDictionary *)keyedValues;
```

- 取值

```objc
// 能取得私有成员变量的值
- (id)valueForKey:(NSString *)key;
- (id)valueForKeyPath:(NSString *)keyPath;//如果valueForKeyPath:方法的调用者是数组，那么就是去访问数组元素的属性值
- (NSDictionary *)dictionaryWithValuesForKeys:(NSArray *)keys;
```


三.KVC底层实现
```objc
// setValuesForKeysWithDictionary底层实现
//  利用KVC字典转模型,
    [flag setValuesForKeysWithDictionary:dict];

    // 1.遍历字典中的所有key
    [dict enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
        // 2.给模型的属性赋值,利用KVC,把字典中的key当做模型的属性名使用,字典中的值传递给模型的属性.
        [flag setValue:obj forKey:key];
        // name -> icon
        // KeyPath:模型中的属性名
        // 属性的值

//        [flag setValue:dict[@"name"] forKey:@"name"];
//        [flag setValue:dict[@"icon"] forKey:@"icon"];
    }];


// setValue:forKey:底层实现
// 给模型中的icon属性赋值
// [flag setValue:dict[@"icon"] forKey:@"icon"];

// 1.首先去寻找模型中有木有setIcon:方法,直接调用setIcon:方法,[flag setIcon:dict[@"icon"]]

// 2.接着寻找模型中有没有icon的属性名,如果有,就直接赋值 icon = dict[@"icon"]

// 3.接着寻找模型中有没有_icon的属性名,如果有,就直接赋值 _icon = dict[@"icon"]

// 4.找不到,直接报错,setValue:forUndefinedKey:

```
