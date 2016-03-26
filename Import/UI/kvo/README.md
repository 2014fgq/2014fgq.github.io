# KVO
## KVO
- 全称：Key Value Observing（键值监听）
- 步骤：
    1. 添加观察者
     - /** addObserver
     *  为对象p添加一个观察者（监听器）
     *
     *  @param Observer 观察者（监听器）
     *  @param KeyPath  属性名（需要监听哪个属性）
     *  @param options  改变的值
     *  @param context 当初addObserver时的context参数值
     */
```objc
[p addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionOld | NSKeyValueObservingOptionNew context:@"test"];```

    - 移除观察者（不确定什么时候该移除，就在代理对象的dealloc方法中移除）
 ```objc
 [p removeObserver:self forKeyPath:@"name"];```

    - /** observeValueForKeyPath
 *  当利用KVO监听到某个对象的属性值发生了改变，就会自动调用这个
 *
 *  @param keyPath 哪个属性被改了
 *  @param object  哪个对象的属性被改了
 *  @param change  改成咋样
 *  @param context 当初addObserver时的context参数值
 */
```objc
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context
{
    NSLog(@"%@ %@ %@ %@", object, keyPath, change, context);
}```
