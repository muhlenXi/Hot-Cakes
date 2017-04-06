### Timer 知识点

#### 启动一个定时器

```objc
let userInfo = ["one":"1"]  //传参
let timer = Timer.scheduledTimer(timeInterval: 2.0, target: self, selector: #selector(ViewController.fire(timer:)), userInfo: userInfo, repeats: true)
timer.tolerance = 2.0 / 10  //容忍误差，用于提高性能
```

*定时器的事件响应方法：*

```objc
func fire(timer:Timer) {
	let timerUserInfo = timer.userInfo
	print(timerUserInfo)
	print("fire...")
        
	//终止定时器
	timer.invalidate()
}
```