### tabbar 的高度不一致

同一 APP 在不同的 iPhone 设备上的 tabbar 的高度不一致，有可能是加载页 `Launcher Image` 的配置不全导致的。

**一般`LauncherImage`需要适配`iOS 7.0 and Later` 和 `iOS 7.0 and Later` ！，以下是这两项分别所需图片的规格大小：**

#### iOS 7.0 and Later

    2x        对应的图片 640 x 960 pixels
    Retina 4  对应的图片 640 x 1136 pixels

#### iOS 8.0 and Later

    Retina HD 5.5  对应的图片 1242 x 2208 pixels
    Retina HD 4.7  对应的图片 750 x 1334 pixels

*注意：如果只配置其中一项，则会导致 tabbar 的高度不一致，笔者的亲身经历过这样诡异的事情。*
