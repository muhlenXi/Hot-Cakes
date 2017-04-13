### UIGestureRecognizer 知识点

#### 1、添加捏合手势（pinch）

```objc
// View
@IBOutlet weak var faceView: HappynessView! {
    didSet {
        faceView.dataSource = self
            
        //添加捏合手势
        let pinGesture = UIPinchGestureRecognizer.init(target: faceView, action: #selector(faceView.pinchScale(gesture:)))
        faceView.addGestureRecognizer(pinGesture)
            
    }
}

//捏合手势响应
func pinchScale(gesture: UIPinchGestureRecognizer) {
        
    if gesture.state == .changed {
         
         //处理手势响应   
         scale *= gesture.scale   
         gesture.scale = 1   //注意每次置1
    }
}


```