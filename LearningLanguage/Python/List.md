### list

```python
# 创建
numberList = [] 
numberList = list()

# 获取元素个数
len(numberList)

# 添加元素
numberList.append(9)
numberList.append([7, 8])

# 合并
numberList2 = [numberList + numberList1]

# 排序
numberList.sort()

# 访问
numberList[0] # 单索引
numberList[0:3] # range 索引

# 插入
numberList.insert(0, "Mars")

# 删除
numberList.pop(0) # 删除并返回指定元素
numberList.remove(0)
del(numberList[0:5]) # 参数是list的切片

# 查询
numberList.index(2)
```
