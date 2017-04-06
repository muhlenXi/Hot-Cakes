### UIAlertController知识点

### 显示一个通用的Alert

```objc
// 1、初始化 alert
let alert = UIAlertController(title: "温馨提示", message: "请检查网络并重试", preferredStyle:.alert)

// 2、添加 action
alert.addAction(UIAlertAction(title: "确定", style: .default, handler: { (alertAction) in
        
	print("确定")
}))
alert.addAction(UIAlertAction(title: "取消", style: .cancel, handler: { (alertAction) in
            
	print("取消")
}))

// 3、显示
present(alert, animated: true, completion: nil)
```

### 往Alert中添加文本框

*可以添加多个文本框。*

```objc
alert.addTextField { (textField) in
            
	textField.placeholder = "输入您的启动密码"
}
```

*获取文本框中的输入内容。*

```objc
let tf = alert.textFields?.first
if tf != nil {
	print("text = \(tf!.text)")
}
```

### 显示一个通用的ActionSheet

```objc
// 1、初始化 actionSheet
let actionSheet = UIAlertController(title: "分享", message: "选择您的分享方式", preferredStyle: UIAlertControllerStyle.actionSheet)

// 2、添加 action
actionSheet.addAction(UIAlertAction(title: "微信分享", style: UIAlertActionStyle.default, handler: { (alertAction) in
            
	print("微信分享")
}))
actionSheet.addAction(UIAlertAction(title: "微博分享", style: UIAlertActionStyle.default, handler: { (alertAction) in
            
	print("微博分享")
}))
actionSheet.addAction(UIAlertAction(title: "QQ分享", style: UIAlertActionStyle.default, handler: { (alertAction) in
            
	print("QQ分享")
 }))
actionSheet.addAction(UIAlertAction(title: "删除消息", style: .destructive, handler: { (alertAction) in
            
	print("删除消息")
}))
actionSheet.addAction(UIAlertAction(title: "取消分享", style:UIAlertActionStyle.cancel, handler: { (alertAction) in
	print("取消分享了")
}))

// 3、显示
present(actionSheet, animated: true, completion: nil)
```

**如果是设备时iPad初始化后需要设置显示方式为Popover！**

```objc
actionSheet.modalPresentationStyle = .popover
let ppv = actionSheet.popoverPresentationController
ppv?.barButtonItem = actionSheetItem
```