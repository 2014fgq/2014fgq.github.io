# NSManagedObject
- 父类是NSObject
- 通过Core Data从数据库取出的对象，默认情况下都是  NSManagedObject对象
NSManagedObject的工作模式有点类似于NSDictionary对  象，通过键-值对来存取所有的实体属性
```objc
setValue:forKey: 存储属性值(属性名为key)
valueForKey: 获取属性值(属性名为key)
```

