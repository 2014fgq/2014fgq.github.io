# NSData
- 父类是NSObject
- 使用archiveRootObject:toFile:方法可以将一个对象直接写入到一个文件中，但有时候可能想将多个对象写入到同一个文件中，那么就要使用NSData来进行归档对象
- NSData可以为一些数据提供临时存储空间，以便随后写入文件，或者存放从磁盘读取的文件内容。可以使用[NSMutableData data]创建可变数据空间

##NSData归档与恢复
```objc
##归档（编码）
// 新建一块可变数据区
NSMutableData *data = [NSMutableData data];
// 将数据区连接到一个NSKeyedArchiver对象
NSKeyedArchiver *archiver = [[[NSKeyedArchiver alloc] initForWritingWithMutableData:data] autorelease];
// 开始存档对象，存档的数据都会存储到NSMutableData中
[archiver encodeObject:person1 forKey:@"person1"];
[archiver encodeObject:person2 forKey:@"person2"];
// 存档完毕(一定要调用这个方法)
[archiver finishEncoding];

##恢复（解码）
// 从文件中读取数据
NSData *data = [NSData dataWithContentsOfFile:path];
// 根据数据，解析成一个NSKeyedUnarchiver对象
NSKeyedUnarchiver *unarchiver = [[NSKeyedUnarchiver alloc] initForReadingWithData:data];
Person *person1 = [unarchiver decodeObjectForKey:@"person1"];
Person *person2 = [unarchiver decodeObjectForKey:@"person2"];
// 恢复完毕
[unarchiver finishDecoding];
```

