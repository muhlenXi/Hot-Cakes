聊聊 AVFoundation 框架

AVFoundation 框架是一个用来播放和创建音视频的框架，它提供了一系列 Objective-C 接口让我们操作音视频的细节数据，比如测试、编辑、重编码视频文件和从设备中获取音视频数据等。

在 iOS 平台上，AVFoundation 的层级是这样的。

![](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Art/frameworksBlockDiagram_2x.png)

如果你想执行一些简单的任务，没必要使用 AVFoundation 框架。

- 比如播放视频，使用 AVKit framework 就可以了。
- 比如录音、获取相册中的照片、视频等，用 UIImagePickerController 类就可以了。

在 AVFoundation 中：

- 播放音频数据，你可以使用 AVAudioPlayer 类，如果你想配置音频的行为，你可以使用 AVAudioSession 类。
- 录音，你可以使用 AVAudioRecorder 类。






### 参考资料

- [AVFoundation Programming Guide](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40010188-CH1-SW3)