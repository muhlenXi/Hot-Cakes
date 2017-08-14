#### struct的巧妙使用

需要将不可变字符串作为key，可以定义一个 `enum` 或 `struct`，然后调用 `enum` 或 `struct` 中的元素作为`key`。

如：

```objc
struct TwitterKey {    
	static let Name = "name"
	static let ScreenName = "screen_name"
	static let ID = "id_str"
	static let Verified = "Verified"
	static let ProfileImageURL = "profile_image_url"
}
```

