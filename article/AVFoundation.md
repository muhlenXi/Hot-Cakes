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

### 聊聊 Asset 和 Video 的播放

AVAsset 是 AVFoundation 框架中最基础的代表视频资源的类。一个 AVAsset 的实例可以理解为是一种包含多种数据的一个集合。我们可以从文件、Photo library 或 iPod library 中生成这个实例。

可以通过一个 URL 来生成一个 AVURLAsset 的实例。其中第二个 options 参数，用来表示是否允许随机精确时间访问。如果你仅仅是为了播放 asset，这个参数可以传 nil，如果你想要加入 AVMutableComposition 中进行视频编辑，则需要传入一个包含 key 为 AVURLAssetPreferPreciseDurationAndTimingKey， value 为 @YES 的 dictionary。

```objc
NSURL *url = <#A URL that identifies an audiovisual asset such as a movie file#>;
NSDictionary *options = @{AVURLAssetPreferPreciseDurationAndTimingKey : @YES };
AVURLAsset *anAssetToUseInAComposition = [[AVURLAsset alloc] initWithURL:url options:options];
```

【1】、如果想要访问用户相册中的 asset，可以通过 Photos framework 中的 PHPhotoLibrary 类来管理和访问 photo library，在相册中分别用 PHAsset 和 PHCollection 来表示相册和分组信息。


【2】、如果想要获取一个 video 中的任意时刻的 image，可以通过 AVAssetImageGenerator 来生成。有几个事项需要注意下：

- 1、确保这个视频的 AVAsset 中有视频轨数据，可以通过 `[anAsset tracksWithMediaType:AVMediaTypeVideo] count]` 是否大于 0 来判断。
- 2、当生成多张 image 时，generator 是一张一张异步生成的，我们需要对 generator 强引用，直到所有的 image 都创建完成。


如果想要对 video 编辑、截取等处理，可以通过 AVAssetExportSession 类来完成。底部有示例 demo 地址。

【3】、如果想要播放 video，根据播放源的不同，目前有两种视频播放方式，一种是基于文件（来源于本地资源）的，另一种是基于数据流（来源于互联网）的。

加载并播放一个本地视频可以简单概括为以下步骤：

- 1、用 AVURLAsset 创建一个 asset。
- 2、用 AVPlayerItem 创建一个以 asset 为参数的 playItem，然后添加状态的 KVO。
- 3、用 AVPlayer 创建一个以 palyItem 为参数的 player。
- 4、用 AVPlayerLayer 创建一个以 player 为参数的 playerLayer。
- 5、将 playerLayer 添加到当前视图的 layer 中，当 status 为 AVPlayerItemStatusReadyToPlay 时就可以播放了。 


```objc
// 添加状态观察
[playerItem addObserver:self forKeyPath:@"status" options:0 context:&ItemStatusContext];
```

【4】、如果想要将播放点切到指定时间点，可以通过调用 player 的 `seekToTime ` 来完成。

【5】、如果你想要在 video 播放结束的时候，重头开始播放。你可以对当前播放的 playItem 添加一个 name 为 AVPlayerItemDidPlayToEndTimeNotification 的 observer，这样在收到通知的时候将播放进度调整到 0 即可。

```objc
[player seekToTime:kCMTimeZero];
```

【6】、如果你想要获取当前播放的进度来刷新 UI 状态，你可以通过调用 player 的 `addPeriodicTimeObserverForInterval:queue:usingBlock: ` 方法来实现， 在 block 回调中做你想做的事情。

【7】、想要播放多个 playItem 时，可以使用 AVQueuePlayer 来播放，调用 `advanceToNextItem ` 来播放下一个资源。

【8】、如果想要对 video 编辑、截取等处理，可以通过 AVAssetExportSession 类来完成。底部有示例 demo 地址。

### 聊聊 Video 的编辑

学习 video 编辑，我们要掌握一个概念 Composition, video 编辑的 API 都是基于 Composition 开展的。Composition 可以理解成一个 video 资源的集合。在这个集合中包含一个或多个 media assets。

- 一个 AVAsset 中通常包含一个音轨和视频轨。
- 用 AVMutableCompositionTrack 类来表示一个轨道。
- AVMutableComposition 类可以用来组装、管理这些轨道，比如插入，删除等。
- AVMutableAudioMix 类可以对 composition 中的音轨进行操作，比如设置音量、音阶等。
- AVMutableVideoComposition 类可以对 composition 中的视频轨进行操作，比如设置输出视频的渲染尺寸、比例、帧率等。
- AVMutableVideoCompositionInstruction 类可以修改视频的背景色、以及进行一些变换。
- animationTool 可以将一些  Core Animation framework 的动画效果添加到视频中。
- AVAssetExportSession 最终将这些音轨、视频轨、配置数据 合并成一个新的 video。


下图是一个简单的 export 示意图：

![](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Art/puttingitalltogether_2x.png)

### 聊聊 Video 拼接

举个 demo 中拼接两个 video 的例子，废话少说，直接上代码。

我们需要经历以下几个过程：

【1】生成 Composition 、音频轨、视频轨。

```objc
// 创建可变集合对象 Composition
AVMutableComposition *mutableComposition = [AVMutableComposition composition];

// 创建视频轨对象
AVMutableCompositionTrack *videoCompositionTrack = [mutableComposition addMutableTrackWithMediaType:AVMediaTypeVideo preferredTrackID:kCMPersistentTrackID_Invalid];

// 创建音频轨对象
AVMutableCompositionTrack *audioCompositionTrack = [mutableComposition addMutableTrackWithMediaType:AVMediaTypeAudio preferredTrackID:kCMPersistentTrackID_Invalid];

```

【2】添加 Asset 中的视频轨数据、音频轨数据。

```objc
// 构建 视频1 视频2 的 asset
NSDictionary *opts = [NSDictionary dictionaryWithObject:@(YES) forKey:AVURLAssetPreferPreciseDurationAndTimingKey];
AVURLAsset *firstVideoAsset = [[AVURLAsset alloc] initWithURL:firstVideoPath options:opts];
AVURLAsset *secondVideoAsset = [[AVURLAsset alloc] initWithURL:secondVideoPath options:opts];


// 提取 视频1 视频2 的视频轨数据
AVAssetTrack *firstVideoAssetTrack = [[firstVideoAsset tracksWithMediaType:AVMediaTypeVideo] objectAtIndex:0];
AVAssetTrack *secondVideoAssetTrack = [[secondVideoAsset tracksWithMediaType:AVMediaTypeVideo] objectAtIndex:0];

// 添加 视频1 视频2 的视频轨数据到 视频轨对象中
[videoCompositionTrack insertTimeRange:CMTimeRangeMake(kCMTimeZero, firstVideoAssetTrack.timeRange.duration) ofTrack:firstVideoAssetTrack atTime:kCMTimeZero error:nil];
[videoCompositionTrack insertTimeRange:CMTimeRangeMake(kCMTimeZero, secondVideoAssetTrack.timeRange.duration) ofTrack:secondVideoAssetTrack atTime:firstVideoAssetTrack.timeRange.duration error:nil];

// 提取 视频1 视频2 的音频轨数据
AVAssetTrack *firstVideoAudioTrack = [[firstVideoAsset tracksWithMediaType:AVMediaTypeAudio] firstObject];
AVAssetTrack *secondVideoAudioTrack = [[secondVideoAsset tracksWithMediaType:AVMediaTypeAudio] firstObject];
    
// 添加 视频1 视频2 的音频轨数据到 音频轨对象中
[audioCompositionTrack insertTimeRange:CMTimeRangeMake(kCMTimeZero, firstVideoAudioTrack.timeRange.duration) ofTrack:firstVideoAudioTrack atTime:kCMTimeZero error:nil];
[audioCompositionTrack insertTimeRange:CMTimeRangeMake(kCMTimeZero, secondVideoAudioTrack.timeRange.duration) ofTrack:secondVideoAudioTrack atTime:firstVideoAssetTrack.timeRange.duration error:nil];

```

【3】检查两个 video 的方向，方向不同的 video 不能拼接

```objc
BOOL isFirstVideoPortrait = NO;
CGAffineTransform firstTransform = firstVideoAssetTrack.preferredTransform;
if (firstTransform.a == 0 && firstTransform.d == 0 && (firstTransform.b == 1.0 || firstTransform.b == -1.0) && (firstTransform.c == 1.0 || firstTransform.c == -1.0)) {
    isFirstVideoPortrait = YES;
}
    
BOOL isSecondVideoPortrait = NO;
CGAffineTransform secondTransform = secondVideoAssetTrack.preferredTransform;
if (secondTransform.a == 0 && secondTransform.d == 0 && (secondTransform.b == 1.0 || secondTransform.b == -1.0) && (secondTransform.c == 1.0 || secondTransform.c == -1.0)) {
    isSecondVideoPortrait = YES;
}
    
if ((isFirstVideoPortrait && !isSecondVideoPortrait) || (!isFirstVideoPortrait && isSecondVideoPortrait)) {
    NSError *error = [self createNSErrorWithCode:400 errorReason:@"视频方向不一致，无法拼接"];
    resultHandler(nil, error);
    return;
}

 ```

【4】添加视频处理命令

```objc
// 构建视频1 的操作命令
AVMutableVideoCompositionInstruction *firstVideoCompositionInstruction = [AVMutableVideoCompositionInstruction videoCompositionInstruction];
firstVideoCompositionInstruction.timeRange = CMTimeRangeMake(kCMTimeZero, firstVideoAssetTrack.timeRange.duration);
AVMutableVideoCompositionLayerInstruction *firstVideoLayerInstruction = [AVMutableVideoCompositionLayerInstruction videoCompositionLayerInstructionWithAssetTrack:videoCompositionTrack];
    
// 微信拍摄的视频tx错位，需要修复
CGRect firstRect = {{0, 0}, naturalSizeFirst};
CGRect firstTransformedRect = CGRectApplyAffineTransform(firstRect, firstTransform);
firstTransform.tx -= firstTransformedRect.origin.x;
firstTransform.ty -= firstTransformedRect.origin.y;
[firstVideoLayerInstruction setTransform:firstTransform atTime:kCMTimeZero];
    
firstVideoCompositionInstruction.layerInstructions = @[firstVideoLayerInstruction];
    
    
// 构建视频2 的操作命令
AVMutableVideoCompositionInstruction * secondVideoCompositionInstruction = [AVMutableVideoCompositionInstruction videoCompositionInstruction];
secondVideoCompositionInstruction.timeRange = CMTimeRangeMake(firstVideoAssetTrack.timeRange.duration, CMTimeAdd(firstVideoAssetTrack.timeRange.duration, secondVideoAssetTrack.timeRange.duration));
AVMutableVideoCompositionLayerInstruction *secondVideoLayerInstruction = [AVMutableVideoCompositionLayerInstruction videoCompositionLayerInstructionWithAssetTrack:videoCompositionTrack];
    
// 微信拍摄的视频tx错位，需要修复
CGRect secondRect = {{0, 0}, naturalSizeSecond};
CGRect secondTransformedRect = CGRectApplyAffineTransform(secondRect, secondTransform);
secondTransform.tx -= secondTransformedRect.origin.x;
secondTransform.ty -= secondTransformedRect.origin.y;
    
[secondVideoLayerInstruction setTransform:secondTransform atTime:firstVideoAssetTrack.timeRange.duration];
secondVideoCompositionInstruction.layerInstructions = @[secondVideoLayerInstruction];
    
AVMutableVideoComposition *mutableVideoComposition = [AVMutableVideoComposition videoComposition];
mutableVideoComposition.instructions = @[firstVideoCompositionInstruction, secondVideoCompositionInstruction];

```

【5】添加渲染尺寸和帧率

```objc
float renderWidth = MAX(fixedSizeFirst.width, fixedSizeSecond.width);
float renderHeight = MAX(fixedSizeFirst.height, fixedSizeSecond.height);
    
mutableVideoComposition.renderSize = CGSizeMake(renderWidth, renderHeight);
mutableVideoComposition.frameDuration = CMTimeMake(1,30);

```

【6】导出视频并保存到相册

```objc
AVAssetExportSession *exporter = [[AVAssetExportSession alloc] initWithAsset:mutableComposition presetName:AVAssetExportPresetHighestQuality];
exporter.outputURL = videoURL;
exporter.outputFileType = AVFileTypeMPEG4;
exporter.shouldOptimizeForNetworkUse = YES;
exporter.videoComposition = mutableVideoComposition;
    
[exporter exportAsynchronouslyWithCompletionHandler:^{
    dispatch_async(dispatch_get_main_queue(), ^{
        if (exporter.status == AVAssetExportSessionStatusCompleted) {
            resultHandler(exporter.outputURL, nil);
        } else {
            NSError *error = [self createNSErrorWithCode:200 errorReason:@"视频导出失败"];
            resultHandler(nil, error);
        }
    });
}];

```

### 聊聊 Video 裁剪

视频裁剪的 demo，这里就不聊了。感兴趣的可以自己下载底部 demo 研究。


### 参考资料

- [AVFoundation Programming Guide](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40010188-CH1-SW3)
- [示例代码 demo](https://github.com/muhlenXi-Team/video-edit-demo)