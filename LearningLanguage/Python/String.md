```python
welcome = "Hello, World!" # 方式1

welcome = 'Hello, World!' # 方式2

welcome = '''Hello, Python!
Hello, World!
Hello, Everybody!
'''

# 字符个数
len(welcome)

# 拼接
string2 = string + string1

# 大小写转换
welcome.upper()
welcome.lower()

# 去掉首尾空格
welcome.strip()

# 字符串分割
welcome[0]
hello = welcome[0:5]

print("%s World!" % "Hello")
print("%s %s" % ("Hello", "World!"))
print("{0} {1}".format("Hello", "World!"))
```

### 获取帮助

```python
dir(welcome) # 获取支持的所有方法
help(welcome.count) # 获取方法的具体帮助
```