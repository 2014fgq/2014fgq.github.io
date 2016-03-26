# AVPlayer
- 音频文件的简单播放

```objc
// 创建一个音频文件的URL(URL就是文件路径对象)
NSURL *url = [[NSBundle mainBundle] URLForResource:@"音频文件名" withExtension:@"音频文件的扩展名"];
// 创建播放器
self.player = [AVPlayer playerWithURL:url];
// 播放
[self.player play];
```

