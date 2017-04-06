### Animation动画 

#### 动画的种类

##### Animating UIview properties （视图动画）

* frame
* transform
* alpha
 
```objc
let myView = UIView(frame: CGRect(x: 50, y: 150, width: 250, height: 300))
myView.backgroundColor = UIColor.red
view.addSubview(myView)
        
if myView.alpha == 1.0 {
            UIView.animate(withDuration: 3.0, delay: 2.0, options: UIViewAnimationOptions.curveEaseOut, animations: { 
                myView.alpha = 0
	}, completion: { (ret) in
      	if ret {
       	myView.removeFromSuperview()
    	}
	})
}
```
 
##### Animation of View Controller transitions （转场动画）
#####  Core Animation （核心动画）
#####  Dynamic Animation （力学动画）