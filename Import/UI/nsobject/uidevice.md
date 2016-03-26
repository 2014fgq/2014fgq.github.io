# UIDevice
- UIDevice类提供了一个单粒对象，它代表着设备，通过它可以获得一些设备相关的信息，比如电池电量值(batteryLevel)、电池状态(batteryState)、设备的类型(model，比如iPod、iPhone等)、设备的系统(systemVersion)

- 通过`[UIDevice currentDevice]`可以获取这个单例对象

- UIDevice对象会不间断地发布一些通知，下列是UIDevice对象所发布通知的名称常量：
```objc
// 设备旋转
UIDeviceOrientationDidChangeNotification
// 电池状态改变
UIDeviceBatteryStateDidChangeNotification
// 电池电量改变
UIDeviceBatteryLevelDidChangeNotification
// 近距离传感器(比如设备贴近了使用者的脸部)
UIDeviceProximityStateDidChangeNotification ```
