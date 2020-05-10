# About Python

Python 是 Guido van Rossum 于 1989 年圣诞节期间，为了打发无聊的圣诞节而编写的一个编程语言。

### 用途

* 网络应用开发，包括网站后台服务等。
* 许多日常需要小工具的开发，如系统管理员需要的脚本任务等。
* 把其他语言开发的程序包装起来，方便使用。

### 优点

* 提供了非常完善的基础代码库，此外还有大量优秀的第三方库。
* 代码简单优雅。

### 缺点

* 运行速度慢，因为 Python 是解释性语言，代码在执行时会逐行的翻译成 CPU 能理解的机器码。
* 代码不能加密。

## Python 编程注意事项 
 
1、用 `#` 开头的语句是注释。 
 
2、Python 语法采用的是缩进的方式，约定俗成，一般为4个空格的缩进。 
 
3、Python 是大小写敏感的。 
 
4、`True`、`False`、`and`、`or`、`not`，用 `None` 表示空值。 
 
5、在 Python 中， 通常用全部大写的变量名来表示常量。 
 
6、`/` 精确除法，结果为浮点数; `//` 地板除，两个整数相除仍然是整数; `%` 为取模或取余数。 
 
7、文件开头代码表示： 
 
```python 
# !/usr/bin/env python3  
# _*_ coding: utf-8 _*_  
``` 

* 第一行注释，是告诉 Linux/OS X 系统，这是一个 python 可执行程序，windows 系统会忽略这个注释； 
* 第二行注释，是告诉 python 解释器，按照 UTF-8 编码读取源代码，否则你在源代码写的中文输出可能会有乱码。 
 
8、只有一个元素的 tuple 定义的时候，必须加一个 `,` 来消除歧义,如 `（1, ）`。 

## Python Interpreter

Python 解释器主要有以下几种：

* CPython，官方版本解释器，使用最广。
* IPython
* PyPy
* Jython
* IronPython

## 引入第三方库的三种方式

1、`import module_name`

```python
import math

print(math.gcd(4, 6)) # 2
```

2、给 module 起别名

```python
import math as m

m.gcd(4, 6)
```

3、使用 from 引入名称

```python
from math import gcd, sqrt
from math import * # 不推荐使用，避免被意外覆盖

sqrt(gcd(4, 6))
```