# NSKeyedArchiver
1. 父类是NSCoder
- 归档成功会保存在Documents下，以".archive"后缀保存
- 如果对象是NSString、NSDictionary、NSArray、NSData、NSNumber等类型，可以直接用NSKeyedArchiver进行归档和恢复
- 不是所有的对象都可以直接用这种方法进行归档，只有遵守了NSCoding协议的对象才可以
    - NSCoding协议有2个方法：
        `encodeWithCoder:`
        - 每次归档对象时，都会调用这个方法。一般在这个方法里面指定如何归档对象中的每个实例变量，可以使用encodeObject:forKey:方法归档实例变量
        `initWithCoder:`
        - 每次从文件中恢复(解码)对象时，都会调用这个方法。一般在这个方法里面指定如何解码文件中的数据为对象的实例变量，可以使用decodeObject:forKey方法解码实例变量

## 系统对象通过NSKeyedArchiver保存与读取数据
```objc
##归档一个NSArray对象到Documents/array.archive
NSArray *array = [NSArray arrayWithObjects:@”a”,@”b”,nil];
[NSKeyedArchiver archiveRootObject:array toFile:path];


##恢复(解码)NSArray对象
NSArray *array = [NSKeyedUnarchiver unarchiveObjectWithFile:path];

//需要遵守<NSCoding>
- (void)encodeWithCoder:(NSCoder *)encoder {
    [encoder encodeObject:self.name forKey:@"name"];
    [encoder encodeInt:self.age forKey:@"age"];
    [encoder encodeFloat:self.height forKey:@"height"];
}
- (id)initWithCoder:(NSCoder *)decoder {
    self.name = [decoder decodeObjectForKey:@"name"];
    self.age = [decoder decodeIntForKey:@"age"];
    self.height = [decoder decodeFloatForKey:@"height"];
    return self;
}
```

## 自定义对象通过NSKeyedArchiver保存与读取数据
## 编码和解码
```objc
归档(编码)
Person *person = [[[Person alloc] init] autorelease];
person.name = @"MJ";
person.age = 27;
person.height = 1.83f;
[NSKeyedArchiver archiveRootObject:person toFile:path];
恢复(解码)
Person *person = [NSKeyedUnarchiver unarchiveObjectWithFile:path];

NSKeyedArchiver-归档对象的注意
如果父类也遵守了NSCoding协议，请注意：
应该在encodeWithCoder:方法中加上一句
[super encodeWithCode:encode];
确保继承的实例变量也能被编码，即也能被归档
应该在initWithCoder:方法中加上一句
self = [super initWithCoder:decoder];
确保继承的实例变量也能被解码，即也能被恢复

###利用归档实现深复制
//比如对一个Person对象进行深复制
// 临时存储person1的数据
NSData *data = [NSKeyedArchiver archivedDataWithRootObject:person1];
// 解析data，生成一个新的Person对象
Student *person2 = [NSKeyedUnarchiver unarchiveObjectWithData:data];
// 分别打印内存地址
NSLog(@"person1:0x%x", person1); // person1:0x7177a60
NSLog(@"person2:0x%x", person2); // person2:0x7177cf0
```

## 3.归档
1.NSKeyedArchiver专门用来做自定义对象归档

```objc
// 什么时候调用:当一个对象要归档的时候就会调用这个方法归档
// 作用:告诉苹果当前对象中哪些属性需要归档
- (void)encodeWithCoder:(NSCoder *)aCoder
{
    [aCoder encodeObject:_name forKey:@"name"];
    [aCoder encodeInt:_age forKey:@"age"];
}

// 什么时候调用:当一个对象要解档的时候就会调用这个方法解档
// 作用:告诉苹果当前对象中哪些属性需要解档
// initWithCoder什么时候调用:只要解析一个文件的时候就会调用
- (id)initWithCoder:(NSCoder *)aDecoder
{
    #warning  [super initWithCoder]
    if (self = [super init]) {
        // 解档
        // 注意一定要记得给成员属性赋值
      _name = [aDecoder decodeObjectForKey:@"name"];
      _age = [aDecoder decodeIntForKey:@"age"];
    }
    return self;
}
