### dict

```python
# 创建
user = {}
user = dict()
user = dict({"name": "Mars"})

# 元素个数
len(user)

# 添加或更新
user["age"] = 18
user.update({"age": 18})

# 访问
user['name']

# 获取 keys 或 values
user.keys()
user.values()

# 是否包含 key 或 value
"name" in user # 是否包含 key name
"Mars" in user # 是否包含 value Mars

# 清空元素
user.clear()

# 删除并返回元素
name = user.pop("name")

# 删除元素或字典
del(user['name']) # 删除元素
del(user)	# 删除字典
```