### 类

```python
"""A general person class."""
class Person:
	# 初始化方法
	def __init__(self, name, age):
		self.name = name
		self.age = age
		
	# 析构方法
	def __del__(self):
		print(self.name + ' is reclaimed.')
		
	# 自定义方法
	def wakeup(self, at):
		print('{0} wakeup at {1} am'.format(self.name, at))
	
	
# 创建类对象及访问
mars = Person('Mars', 30)

mars.name
mars.age

# 查询对象是否包含某个属性
hasattr(mars, 'name')

# 读取或设置对象属性
getattr(mars, "name")
setattr(mars, "name", "11")

# 删除对象的属性
delattr(mars, "name")

# 删除对象
del(mars)
```

#### 类的继承

```python
from Person import Person

class Employee(Person):
	__count = 0 # 静态属性
	
	def __init__(self, work_id, name, age):
		Person.__init__(self, name, age)
		self.work_id = work_id	
		
		Employee.__count +=  1


# 初始化
mars = Employee(11, 'Mars', 30)

# 判断是否是另一个类的子类
issubclass(Employee, Person)

# 判断是否是某各类的对象
isinstance(mars, Person)
isinstance(mars, Employee)
```

#### `__repl__` 和 `__str__`

* 首先，`__str__` 是通过 `__repr__` 实现的，因此， 它的默认实现没有什么特别的用途；
*  其次， `__repr__` 是给一个类型的开发者使用的，它应该包含关于类型的更多信息；而 `__str__` 的描述则是针对类型的使用者，它应该尽可能易读，友好；

Mark: [from Boxue](https://www.boxueio.com/series/python-101/ebook/288)

