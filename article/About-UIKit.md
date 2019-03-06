---
title: App Frameworks 之 UIKit 框架
date: 2017-02-01 11:20:19
categories: blog
tags: [UIKit]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: <http://muhlenxi.com/2017/02/01/About-UIKit>*

### 导语：

> 本文是关于 UIKit 框架的描述。

<!-- more -->

如果你想要学 mac OS / iOS App 开发，你需要参考的第一手资料则是 [Apple Developer Documentation](https://developer.apple.com/documentation),在这里，Apple 公司会不定期更新最新的 Kit(开发包)，包括 API reference（API 接口）、技术文章和示例代码等。

### UIKit

UIKit 用于为你的 iOS App 或者 tvOS App 用户交互界面的构建、绘制和事件驱动。

UIKit 框架为你的 iOS App 或者 tvOS App 提供必要的基础结构。比如，用于实现界面的 window 和 view 结构，用于多点触控的事件处理结构，还有一个 main run loop 来管理用户交互、系统和 App。除此之外还包括，动画支持，文件支持，绘制和打印支持，当前设备的信息，文本的管理和显示，搜索支持，可访问性（accessibility）支持，app 扩展支持和 资源管理。

**切记：** 大部分来说，只在主线程或主队列中使用 UIKit 的类对象。 该限制适用于 `UIResponder` 的派生类，或者涉及 app 用户的界面的任何操作。

#### UIKit 基础

UIkit 管理 app 与系统之间的交互，提供用于管理 app 数据和资源的类。[传送门](https://developer.apple.com/documentation/uikit)

###### Core App

用于管理 app 的数据模型和与系统的交互 。[详情传送门](https://developer.apple.com/documentation/uikit/core_app)

包括：

**class** :  `UIApplication`,`UIDevice`,`UITraitCollection`,`UIDocument`,`UIManagedDocument`,

`UIPasteboard`, `UIPasteConfiguration`

**protocol** : `UIApplicationDelegate`,`UITraitEnvironment,UIAdaptPresentationControllerDelegate`,`UIPasteConfigurationSupporting`,

`UIDataSourceModelAssociation`,`UIGuideAccessRestrictionDelegate`

###### Resource Management

用于管理 image, string, storyboard 和 存储在主可执行文件之外的 nib 文件。[详情传送门](https://developer.apple.com/documentation/uikit/resource_management)

包括：

**class** : `UIStoryboard`,`UIStoryboardSegue`,`UIStoryboardUnwindSegueSource`,`UIImageAsset`,

`NSDataAsset`,`UINib`


###### App Extensions

将 app 的基础功能扩展到系统的其他部分。[详情传送门](https://developer.apple.com/documentation/uikit/app_extensions)

包括：

**class** : `UIDocumentPickerExtensionViewController`,`NSFileProviderExtension`,`UIInputViewController`,`UILexicon`,`UILexiconEntry`

**protocol** : `UITextDocumentProxy`,`UIInputViewAudioFeedback`


View 帮你显示屏幕内容和方便用户交互；view controller 帮你管理 view 和 界面的结构。

###### Views and Controls

以特定的方式呈现屏幕内容，并定义与内容相关的交互。[详情传送门](https://developer.apple.com/documentation/uikit/views_and_controls)

包括：

**class** : `UIView`,`UIStackView`,`UIScrollView`,`UIActivityIndicatorView`,`UIImageView`,

`UIPickerView`,`UIProgressView`,`UIWebView`,`UIControl`,`UIButton`,

`UIDatePicker`,`UIPageControl`,`UISegmentedControl`,`UISlider`,`UIStepper`,`UISwitch`,

`UILabel`,`UITextField`,`UITextView`,`UIBarItem`,`UIBarButtonItem`,

`UIBarButtonItemGroup`,`UINavigationBar`,`UISearchBar`,`UIToolbar`,`UITabBar`,

`UITabBarItem`,`UIMenuController`,`UIMenuItem`,`UIVisualEffect`,`UIVisualEffectView`,

`UIVibrancyEffect`,`UIBlurEffect`

**Container** : `Collection Views`, `Table Views`

**protocol** : `UIBarPositioning`,`UIBarPositioningDelegate`,`UIAppearance`,`UIAppearanceContainer`

**struct** : `UIEdgeInsets`,`NSDirectionalEdgeInsets`,`UIOffset`

###### View management

用 view controller 和 navigation 来显示不同的界面的内容。[详情传送门](https://developer.apple.com/documentation/uikit/view_management)

包括：

**class** : `UIViewController`,`UIPresentationController`,`UISplitViewController`,`UINavigationController`,

`UINavigationItem`,`UINavigationBar`,`UIPageViewController`,`UITabbarController`,

`UITabBar`,`UITabBarItem`,`UISearchContainerViewController`,`UISearchBar`,`UISearchController`,

`NSLayoutConstraint`,`UILayoutGuide`,`NSLayoutAnchor`,`NSLayoutAnchor`,`NSLayoutDimension`,

`NSLayoutXAxisAnchor`,`NSLayoutYAxisAnchor`,`UIFocusGuide`,`UIFocusSystem`,`UIFocusUpdateContext`,

`UIFocusAnimationCoordinator`,`UIFocusDebugger`

**protocol** : `UIContentContainer`,`UISearchBarDelegate`,`UISearchResultsUpdating`,`UILayoutSupport`,

`UIFocusItem`,`UIFocusEnvironment`,`UIViewControllerRestoration`,`UIObjectRestoring`

###### System View Controller

使用内建的 UIKit view controller 来挑选照片，编辑视频，分享内容，打印文件等等。[详情传送门](https://developer.apple.com/documentation/uikit/system_view_controllers)

包括：

**class** : `UIImagePickerController`,`UIVideoEditorController`,`UIDocumentBrowserViewController`,`UIDocumentBrowser`,

`UIDocumentBrowserTransitionController`,`UIDocumentPickerViewController`,`UIDocumentInteractionController`,`UICloudSharingController`,

`UIActivityViewController`,`UIActivity`,`UIActivityItemProvider`,`UIPrinterPickerController`,

`UIReferenceLibraryViewController`

**protocol** : `UIVideoEditorControllerDelegate`,`UIActivityItemSource`,`UIPrinterPickerControllerDelegate`

**enum** : `UIDocumentBrowserError.Code`

**let** : `UIDocumentBrowserErrorDomain`

###### Drag and Drop

使用交互 API 为你的 view 添加 drag and drop 功能。[详情传送门](https://developer.apple.com/documentation/uikit/drag_and_drop)

包括：

**protocol** : `UIDragInteractionDelegate`,`UIDropInteractionDelegate`,`UIInateraction`,`UISpringLoadedInteractionBehavior`,

`UISpringLoadedInteractionSupporting`,`UISpringLoadedInteractionContext`,`UISpringLoadedInteractionEffect`,`UIDragDropSession`,

`UIDragSession`,`UIDragAnimating`,`UIDropSession`,`NSItemProviderReading`,`NSItemProviderWriting`,

`UIPasteConfigurationSupporting`

**class** : `UIDragInteraction`,`UIDropInteraction`,`UISpringLoadedInteraction`,`UIDragItem`,`UIDropProposal`,

`NSItemProvider`,`UIPasteConfiguration`,`UIDragPreviewParameters`,`UIDragPreview`,`UIDragPreviewTarget`,

`UITargetedDragPreview`

**enum** : `UIDropOperation`,`UIDropSessionProgressIndicatorStyle`

###### Accessibility

让残疾用户更容易的使用你的 app。[详情传送门](https://developer.apple.com/documentation/uikit/accessibility)

包括：

**protocol** : `UIAccessibilityIdentification`,`UIAccessibilityReadingContent`,`UIAccessibilityContentSizeCategoryImageAdjusting`,

`UIScrollViewAccessibilityDelegate`,`UIPickerViewAccessibilityDelegate`,`UIAccessibilityContainerDataTable`,

`UIAccessibilityContainerDataTableCell`

**class** : `UIAccessibilityCustomAction`,`UIAccessibilityElement`,`UIAccessibilityCustomRotor`,`UIAccessibilityCustomRotorItemResult`,

`UIAccessibilityCustomRotorSearchPredicate`,`UIAccessibilityLocationDescriptor`

**enum** : `UIAccessibilityContainerType`

**struct** : `UIAccessibilityHearingDeviceEar`

###### Animation and Haptics

使用基于 view 的动画和触觉向用户提供反馈。[详情传送门](https://developer.apple.com/documentation/uikit/animation_and_haptics)

包括：

*class* : `UIFeedbackGenerator`,`UIImpactFeedbackGenetator`,`UINotificationFeedbackGenetator`,`UISelectionFeedbackFenerator`

###### Windows and Screens

为你的 view 层次结构和其他内容提供容器。[详情传送门](https://developer.apple.com/documentation/uikit/windows_and_screens)

包括：

**class** : `UIWindow`,`UIPopoverPresentationController`,`UIPopoverBackgroundView`,`UIAlertController`, `UIAlertAction`,

`UIScreen`,`UIScreenMode`

**protocol** : `UICoordinateSpace`,`UIPopoverBackgroundViewMethods`

### 结束语

*欢迎在本文下面留言一起交流心得或错误指正...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*
    




