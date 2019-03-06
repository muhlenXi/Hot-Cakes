### 访问控制权限

- 用 **open** 修饰的类、属性和方法可以被任何人（包括其他 module）使用，其他 module 可以继承和 override。
- 用 **public** 修饰的类、属性和方法可以被任何人（包括其他 module）使用，但是其他 module 不能继承和 override。
- 用 **internal（默认访问级别）** 修饰的类、属性和方法只能在当前 module 内使用。
- 用 **fileprivate** 修饰的类、属性和方法只能在当前 swift 源文件中访问，其他 module 无法访问和继承。
- 用 **private** 修饰的类、属性和方法只能在当前类访问。其他 module 无法访问和继承


因此访问权限由大及小依次为：

> open > public > internal > fileprivate > private