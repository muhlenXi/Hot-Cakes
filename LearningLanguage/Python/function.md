## 函数

*Python 中的所有函数，都是有返回值的，不指定 return 语句则会返回 None。*

## syntax


```python
# 1、无参
def a_stub_function():
	pass


# 2、参数个数固定
def sum(a, b):
	return a + b
	
sum(2, 3)		# 调用方式一
sum(b=2, a=3)		# 调用方式二


# 3、参数个数固定，带有默认值
def sum(m=1, n=2):
    return m + n

print(sum())    # 使用了默认值 m，n
print(sum(3))   # 使用了默认值 n
print(sum(3, 3))    # 未使用默认值
print(sum(n=2))     # 使用了默认值 m 

# 函数的默认参数定义，只能是从右往左依次定义的。我们不能指定 m 的默认值，而不指定 n 的默认值。


# 4、可变参数列表
def add(*args):
	tmp = 0
	for i in args:
		tmp += i
	
	return tmp

add(1, 2, 3, 4) # 调用


# 5、可变参数带默认值
def add(**args):
	tmp = 0
	for i in args:
		tmp += args[i]
	
	return tmp
	
	
add(n1=1, n2=2, n3=3) # 调用


# 6、混合使用
def add(*args, **k_args):
	tmp = 0
	for i in args:
		tmp += i
		
	for i in k_args:
		tmp += k_args[i]
	
	return tmp
	
	
add(4, 5, n1=1, n2=2, n3=3) # 调用
```

