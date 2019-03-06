---
title: 初识 Swift3 
date: 2016-08-19 09:37:28
categories: Swift
tags: [Swift3初体验]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/08/19/Swift3](http://muhlenxi.com/2016/08/19/Swift3)*

### 导语：

> Swift 是一个新的编程语言，使用它可以开发 iOS、macOS、watchOS 和 tvOS 等平台上的 APP ，它比 C 和 Objective-C 语言更好的胜任这项工作，Swift 采用安全的编程模式，它加入了一些现代的特色使得编程更加容易、更加灵活和更加有趣，Swift 基于成熟的受人喜爱的 Cocoa 和 CocoaTouch 框架，这是一个重新定义软件是如何开发的机会！Just enjoy it！。

点击阅读全文来了解一下详情吧。

<!-- more -->

### Swift 之初体验

#### 单一变量

##### *在屏幕上打印 hello，world!*

```objc
print("hello,world!") 
```
##### *定义一个变量和改变变量的值*

```objc
var myVariable = 42
myVariable = 50
```

##### *定义一个常量 *

```objc
let myConstant = 38
```

*我们定义常量和变量的时候可以不显示的声明常量或变量的数据类型，编译器会根据你设置的初始值去推测数据的类型。*

*带数据类型的常量的声明是这样的。*

```objc
let myWeight:Double = 54.0
```

##### *字符串类型转换。*

```objc
let label = "the width is "
let width = 94
let widthLabel = label + String(width)
```

*还有个更简单的方式。*

```objc
let apples = 3
let oranges = 5
let appleSummary = "I hava \(apples) apples"
let fruitSummary = "I have \(apples + oranges) pieces of fruit"
```

##### *定义一个不可变数组 *

```objc
let animals = ["猴子","大象","兔子"]
```

##### *定义一个可变数组 *

```objc
var fruitLists = ["苹果汁","香蕉","梨","葡萄"]
fruitLists[0] = "苹果"       //修改 
fruitLists.append("猕猴桃")  //增加        
fruitLists.remove(at: 4)    //删除
```

*定义一个空的数组*

```objc
let animals = [String]()
```

##### *定义一个不可变字典 *

```objc
let wheelsOfCar = ["独轮车":1,"自行车":2,"小汽车":4]
```
*定义一个空的字典*

```objc
let emptyDictionary = [String: NSInteger]()
```
##### *定义一个可变字典 *

```objc
var wheelsOfCar = ["独轮车":1,"自行车":2,"小汽车":4]
wheelsOfCar["三轮车"] = 3   //增加或修改
wheelsOfCar.removeValue(forKey: "三轮车")  //删除
print("\(wheelsOfCar)")
```

#### 控制流

##### * `if - else` 条件判断语句 *

```objc
let scores = [75,43,103,87]
var count = 0
	for score in scores {
    	if score > 50 {
	       count += 1
       } else {
                count += 0
       }
    }
print("分数大于零的科目个数：\(count)")
```

**if或else后面的`（）`可以省略，但是 `{}` 不能省略！**

##### *可选值操作*

```objc
let optionalName:String? = "张三"
var greeting = "hello!"
if let name = optionalName {
	     greeting = "hello,\(name)"
    }else {
        greeting = "optionalName is nil"  //当名字为nil时会执行这句
    }
print(greeting)
```

*我们也可以用操作符 `??` 给可选值设定一个默认值，当可选值为 nil 时会自动以默认值填充！*

```objc
let nickName:String? = nil
let defaultName = "李四"
let informalGreeting = "Hi \(nickName ?? defaultName)"
print(informalGreeting)
```

##### *switch - case 条件判断语句*

```objc
let vegetable = "土豆"
switch vegetable {
case "辣椒":
      print("辣椒吃多了容易上火。")
case "红薯", "土豆":
      print("这种蔬菜淀粉含量比较高。")
case let x where x.hasSuffix("瓜"):
      print("\(x) 属于瓜类的一种")
default:
      print("这种蔬菜的信息暂未获得。")
}
```

##### *通过 for - in 来遍历数组或字典*

```objc
let intersetingNumbers = ["素数":[2,3,5,7,11,13],"斐波纳契数":[1,1,2,3,5,8],"平方数":[1,4,9,16,25]]
var largest = 0
//找出这些数中的值最大的数
for (_,numbers) in intersetingNumbers {
            
    for number in numbers {
                
        if number > largest {
           largest = number
        }
     }
}
print(largest)
```

##### *可以通过 for-in 来控制循环次数*

```objc
//全闭区间
var sum = 0
for i in 0...4 {
            
   sum = sum + i  //循环5次
}
print(sum)
        
//半开半闭区间
var sum1 = 0
for i in 0..<4 {
            
   sum1 = sum1 + i  //循环4次
}
print(sum1)

```

##### *`while` 循环 和 `repeat - while` 循环 *

```objc
//先判断后执行
var n = 2
while n < 100 {  
     n = n * 2
}
print(n)  //输出128

//先执行后判断        
var m = 2
repeat {       
	m = m * 2
}while m < 100
print(m)  //输出128
```

#### 函数 和 闭包

##### *用 `func` 声明一个函数，`()`中是参数，`->`后面是返回值的类型*

```objc
func greet(person: String,day :String) -> String {
        
    return "你好 \(person),今天是 \(day)。"
}
```

*函数的调用：* 

```objc
let str:String = greet(person: "张三", day: "星期天")  //调用函数
print(str)
```

##### *禁止设置默认参数名*

*当我们声明函数时不指定参数的名字时，参数的名字默认为形式参数的名字，当然，我们可以通过`_`来禁止设置默认参数名*

```objc
func greet(_ person:String, on day:String) -> String {
        
    return "Hello \(person),today is \(day)"
}
```

*方法调用是这样：*

```objc
print(greet("Tom", on: "Monday"))
```

##### *使用元组 (tuple) 来创建复合值*

```objc
func calculateStatistics(scores:[Int]) -> (min:Int,max:Int,sum:Int) {
        
        var min = scores[0]
        var max = scores[0]
        var sum = 0

        for score in scores {
            
            if score > max {
                max = score
            } else if score < min {
                min = score
            }
            
            sum += score
        }
        return (min,max,sum)
}
```

*函数调用时这样的：*

```objc
let statistics = calculateStatistics(scores: [5,3,100,25,4])
print("min = \(statistics.min)")
print("max = \(statistics.max)")
print("sum = \(statistics.2)")
```

*函数的参数个数也可以是不确定的 *

```objc
func sumOf(numbers:Int...) -> Int {
        
   var sum = 0
   for num in numbers {
       sum += num
   }
   return sum
}
```

*调用是这样的：*

```objc
print(sumOf())                       //输出0
print(sumOf(numbers: 100,200,50))    //输出350
```

##### 函数可以嵌套，嵌套的函数可以读取或修改外部函数的变量的值。

```objc
//函数定义
func returnFifteen() -> Int {
     var y = 10
     func add() {
         y += 5
     }
     add()
     return y;
}

//函数调用：
print(returnFifteen())
```

##### 函数的 返回值 也可以是函数。

```objc
//生成一个增量器
func makeIncrementer() -> ((Int) -> Int) {
        
   func addOne(number:Int) -> Int {
       return number + 1
   }
   return addOne;
}

//方法调用
let increment = makeIncrementer()
print(increment(7))
```

##### 函数的 参数 也可以是函数。

```objc
func lessThanTen(number:Int) -> Bool {
        
    return number < 10
}
    
//检查数组中是否包含小于10的元素
func checkArray(list:[Int],condition:(Int) -> Bool)  -> Bool {
        
 for item in list {
            
      if condition(item) {
           return true
      }
  }
        
	return false
}
```

*函数调用是这样的：*

```objc
let numbers = [18,9,17,12]
let ret = checkArray(list: numbers, condition: lessThanTen)
print(ret)
```

##### 函数是闭包的一种特殊形式,可以用`{}`实现一个闭包，在闭包中用`in`来分离参数和返回值类型 。

```objc
let numbers = [20,19,7,12]

//将数组中的元素的值加倍
let mappedNumbers = numbers.map({number in number * 2})
print(mappedNumbers)

//逆序排列数组中的元素
let sortedNumbers = numbers.sorted{$0 > $1}
print(sortedNumbers)
```

#### 对象 和 类

##### 用 `class` 来创建一个类，属性的声明和常量、变量的声明方式一样，方法和函数的声明方式也一样。

*可以通过 `init` 来生成一个 `initializer` 来初始化对象。*

*可以通过 `deinit` 来生成一个 `deinitializer`，当对象被释放时做一些清理工作。*

*类的声明如下：*

```objc
//声明一个几何体
class Shape {
        
     var numberOfSides = 0   //边的个数
     var name :String        //几何体的名字
        
        
     init(name:String) {
         self.name = name
     }
        
     //简单描述
     func simpleDescription() -> String {
          return "这个叫\(name)的几何体有\(numberOfSides)条边"
     }
}
```
*类的对象的生成：*

```objc
let shape = Shape.init(name: "七边形")
shape.numberOfSides = 7
print(shape.simpleDescription())
```

##### 类的继承

*如果该类是继承某一父类的子类，则声明子类的时候要用 `:`+ 父类名 来表明其继承自什么类。当没有父类的时候，可以省略不写。*

*当子类要覆写父类的方法时，子类方法前要添加关键词 `override`*

*声明一个继承Shape的Square类*

```objc
//正方形
class Square: Shape {
        
     var sideLength:Double   //边长
        
     init(sideLength:Double,name:String) {
            
         self.sideLength = sideLength
         super.init(name: name)
         numberOfSides = 4
     }
        
     override func simpleDescription() -> String {
            
     return "这个叫\(name)的几何体有\(numberOfSides)条边,边长为\(sideLength)cm"
     }
        
     //计算面积
     func area() -> Double {
            
            return sideLength * sideLength
     }
        
 }```

*子类对象的创建:*

```objc
let square = Square.init(sideLength: 4, name: "正方形")
print("面积是：\(square.area())")
print(square.simpleDescription())
```

##### 类的属性，也可以自己生成 getter 和 setter 方法：

```objc
//等边三角形
class EquialateralTriangle: Shape {
        
     var sideLenth :Double = 0
        
     init(sideLength:Double,name:String) {
         self.sideLenth = sideLength
         super.init(name: name)
         numberOfSides = 3
     }
        
     //周长
     var perimeter:Double {
         set {
                sideLenth = newValue / 3.0
            }
         get {
                return sideLenth * 3.0
            }
     }
        
     override func simpleDescription() -> String {
         return "这是一个边长为\(sideLenth)的等边三角形"
     }
}
```

*等边三角形对象的创建:*

```objc
let triangle = EquialateralTriangle.init(sideLength: 5, name: "等边三角形")
//获取周长
print("周长是：\(triangle.perimeter)")
triangle.perimeter = 12
//获取修改周长后的边长
print("边长是：\(triangle.sideLenth)")
print(triangle.simpleDescription())
```

#### 枚举 和 结构体

##### 用 `enum` 来声明一个枚举，枚举中也可以关联函数

*第一个枚举值默认是0，我们也可以指定第一个枚举值为我们想要的值。*

*举个 🌰 ：*

```objc
enum Direction : NSInteger {
     case up = 1,
     down,
     left,
     right
        
     func description() -> String {
        switch self {
            case .up:
                return "向上"
            case .down:
                return "向下"
            case .left:
                return "向左"
            case .right:
                return "向右"
         }
      }
}
```

*调用如下：*

```objc
let direc = Direction.down
print(direc)
print(direc.rawValue)
print(direc.description())
```

*我们也可以通过 `rawValue` 来初始化一个枚举的选项，得到的是一个可能值。*

```objc
let direc = Direction(rawValue: 2)
print(direc!.description())
```

*枚举中的选项还可以和不同的常量值关联，当我们创建一个枚举的实例时，就可以初始化不同的常量值。*

```objc
enum ServerResponse {
     case result(String,String)
     case failure(String)
}
```

##### 用 `struct` 来声明一个结构体，枚举中也可以关联函数

*举个 🌰 ：*

```objc
//矩形类
struct Rectangle {
        
     var origin:CGPoint
     var width:Float
     var height:Float
        
     func area() -> String {
            
            let area = width * height
            return "\(area)"
     }
        
     func perimeter() -> Float {
            return (width + height) * 2
     }
}
```

*简单运用*

```objc
let myRectangle = Rectangle(origin: CGPoint.init(x: 0, y: 0), width: 5, height: 6)
print("周长：\(myRectangle.perimeter())")
print("面积：\(myRectangle.area())")
```

#### 协议 和 类扩展

##### 用 `protocol` 来声明一个协议。

```objc
protocol TestProtocol {
    var simpleDescription:String { get }
    mutating func adjust()
}
```

*`类`、`枚举`和`结构体`都可以遵循协议*

```objc
class TestClass: TestProtocol {
        
    var value:Int = 108

    var  simpleDescription: String = "这是一个测试类"
    func adjust() {
         simpleDescription.append("+测试值是\(value)")
    }
}
    
struct TestStruct:TestProtocol {
        
    var simpleDescription: String = "一个测试结构体"
    mutating func adjust() {
            simpleDescription.append("+ 遵循协议")
    }
}
```

*简单运用*

```objc
let test = TestClass()
test.adjust()
print(test.simpleDescription)
        
var testStruct = TestStruct()
testStruct.adjust()
print(testStruct.simpleDescription)
```

##### 用 `extension` 来声明一个类扩展，给存在的类增加一个方法或算值属性 。

*举个 🌰 ：*

```objc
extension Int
{
    //简介 算值属性
    var simpleDescription:String {
        return "the number \(self)"
    }
    //自增2
    mutating func addTwo() {
        self = self + 2
    }
    
}
```

*简单使用*

```objc
var num:Int = 8
print(num.simpleDescription)
num.addTwo()
print(num)
```

**用 `extension` 也可以让存在的类或数据类型遵循某项协议。**

```objc
protocol testProtocol {
    var simpleDescription:String {
        get
    }
    mutating func adjust()
}

extension Double: testProtocol
{
    var simpleDescription: String
        {
        return "Double value \(self)"
    }
    
    mutating func adjust() {
        self *= 2
    }
}
```

*简单运用：*

```objc
var pi:Double = 3.14
print(pi.simpleDescription)
pi.adjust()
print(pi)
```

#### 错误处理

*通过遵循错误协议 `Error` 来声明一个错误类型。*

```objc
enum PrinterError:Error {
    case outOfPaper
    case noToner
    case onFire
}
```

*用 `throw` 来抛出错误和用 `throws` 来标记一个可以抛出错误的函数，当函数抛出错误时，函数会立即返回，并调用错误处理方法。*

```objc
func send(job:Int, toPrinter printerName:String) throws ->String {
     if printerName == "Nerver Has Toner" {
         throw PrinterError.noToner
     }
     return "Job sent"
}
```

*用 `do-catch` 来处理错误，在 `do代码块` 中，调用可抛出错误的函数时，前面需要用 `try` 来标记,而在 `catch代码块` 中，错误的默认名字是 error，除非设置了一个别名*。

```objc
do {
        let printerResponse = try send(job: 1040, toPrinter: "Nerver Has Toner")
        print(printerResponse)
        
    } catch {
        print(error)
  	}
}
```

*可以用多重 `catch代码块` 来详细处理错误*

```objc
do {
        let printerResponse = try send(job: 1040, toPrinter: "Nerver Has Toner")
            print(printerResponse)
        } catch PrinterError.onFire {
            print("打印机过热，休息一会儿再使用")
        } catch PrinterError.noToner{
            print("打印机没墨了，请添加墨水")
        } catch {
            print("纸张用完，请添加纸张")
    }
```

*另一种处理错误是用 `try?` 将结果转换成一个可选值，如果函数抛出错误，这个错误将被丢弃，结果为nil，否则返回值是一个包含结果的可选值。*

```objc
let printerSuccess = try? send(job: 1884, toPrinter: "hello world")
let printerFailure = try? send(job: 1884, toPrinter: "Nerver Has Toner")
```

*使用`defer代码块`来声明函数返回前需要执行的语句，该语句不管函数是否抛出错误都会执行。*

```objc
var fridgeIsOpen = false
let fridgeContent = ["milk","eggs","apple"]
    
func fridgeContains(food:String) -> Bool {
        
        fridgeIsOpen = true
        defer {
            fridgeIsOpen = false
        }
        
        let result = fridgeContent.contains(food)
        return result
        
}   
```

*简单运用*

```objc
let ret = fridgeContains(food: "banana")
print(ret)
print(fridgeIsOpen)
```

#### 泛型

##### *用 `<name>` 声明一个泛型或泛型方法。*

```objc
func makeArray<Item>(repeat item:Item, numberOfItems:Int) -> [Item] {
        
        var result = [Item]()
        for _ in 0..<numberOfItems {
            result.append(item)
        }
        return result
        
}

//函数调用
print(makeArray(repeat: "Apple", numberOfItems: 7))

```

*函数、类、枚举、结构体等也可以声明成泛型的方式*

```objc
enum OptionalValue<Wrapped> {
    case none
    case some(Wrapped)
}

//简单调用
var possibleValue:OptionalValue<Int> = .none
possibleValue = .some(200)
print(possibleValue)
```

*在方法名后面来使用 `where` 指定对类型的需求---比如，需要都实现一个协议，需要两个对象的类型一样，或都用于共同的父类等*

```objc
//判断是否有共同元素
    func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
        where T.Iterator.Element: Equatable, T.Iterator.Element == U.Iterator.Element {
            for lhsItem in lhs {
                for rhsItem in rhs {
                    if lhsItem == rhsItem {
                        return true
                    }
                }
            }
            return false
}
```

*简单运用*

```objc
let ret = anyCommonElements([1,2,3], [5])
print(ret)
```

### 结束语：

在这里查阅[Swift3官方文档](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/GuidedTour.html#//apple_ref/doc/uid/TP40014097-CH2-ID1)

*感谢阅读，有什么不对的地方可以给我留言，一起学习，一起进步！*