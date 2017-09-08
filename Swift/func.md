## 函数

函数在 Swift 是一等公民意味着：

函数类型和 Swift 其他类型有完全相同的语法功能。包括：

* 可以用来定义变量；
* 可以当成函数参数；
* 可以被函数返回。

### internal name and external name

```swift
func mul(value m: Int, of n: Int) {
	return m * n
}
```

如上：value 和 of 为 external name，m 和 n 为 internal name；

* External name 为了让函数在调用的时候，呈现更好的语义；
* Internal name 为了让函数在实现的时候，呈现更好的实现逻辑；



