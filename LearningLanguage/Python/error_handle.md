### 异常处理

```python
try:
    3/0
except AttributeError:
    print("访问了不存在的属性。")
except IndexError:
    print('访问 list 或 tuple 越界。')
except KeyError:
    print('访问了 dict 中不存在的 key。')
except NameError:
    print('访问了一个不存在的名称。')
except SyntaxError:
    print('语法错误。')
except TypeError:
    print('传入参数类型错误。')
except ValueError:
    print('值错误。')
except ZeroDivisionError:
    print("除数为0")
except Exception:
    print("所有异常的基类。")
else:
    print('No error.')
finally:
    print('Clean up actions.')
```