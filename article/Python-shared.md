---
title: 如何分享 Python 代码到 PyPI 社区
date: 2017-11-06 22:58:15
categories: blog
tags: [Python, PyPI]

---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: <http://muhlenxi.com/2017/11/06/Python-shared>*

### 导语：

> Python 是一个高级、通用、强大的开源编程语言，然而 Python 的流行和强大离不开开源社区的代码共享，所以，我们应该积极向 Python 社区开放你的代码 ... 共享总是一件好事，不是吗？
> 本文将教你如何向 PyPI 社区共享代码。

<!-- more -->

经过以下的 7 个步骤，即可成功的向 PyPI 社区提交代码模块。

### 准备发布

为了共享代码模块，需要准备一个发布（distribution），也就是一个允许你构建、打包、发布模块的一个文件集合。

**1、**首先为模块创建一个文件夹。复制 python 代码文件到该文件夹中。

**2、**在该文件夹中创建一个名为 `setup.py` 的文件，打开，并增加如下代码：

```python
from distutils.core import setup		# 从 Python 发布工具导入 setup 函数


setup(
    name="mlx_nester",		# 模块名
    version="1.3.0",		# 模块版本号
    py_modules=['mlx_nester'],		# 关联模块的元数据
    author="muhlenXi",		# 作者		
    author_email="muhlenxi@gmail.com",		# 作者邮箱
    url="http://www.muhlenxi.com",		# 作者的主页
    description="A simple printer of nested lists",		# 模块的简单功能描述
)
```

### 构建发布

**3、**打开 `终端` ，cd 到新建的文件夹目录下，输入如下命令：

```python
python3 setup.py sdist
```

**4、**将发布安装到你的 Python 本地副本中，继续输入以下命令：

```python
sudo python3 setup.py install
```

注：该过程需要输入电脑密码。


### 发布预览

**5、**使用 Python 关键字 `import` 导入已经安装的模块，然后测试模块代码能否正常运行，如果没问题的话，执行第六步。

### 向 PyPI 上传代码

如果第一次上传代码的话，需要先注册一个 PyPI 社区的账号，注册流程如下：

* 1、 浏览器中输入 <https://pypi.python.org/pypi> 打开 PyPI 网站。
* 2、点击最右侧的 [Register](https://pypi.python.org/pypi?%3Aaction=register_form) 按钮，然后在界面上一次输入用户名、密码、确认密码、Email，然后勾选 I agree，最后点击 register 选项，PyPI 会向你邮箱发一份确认邮件。
* 3、打开邮箱中的 PyPI 邮件，点击确认链接来完成 PyPI 注册就可以了。

**6、**给上传工具配置你的 PyPI 的账户名和密码。（本步骤只需操作一次）

打开 `终端`,cd 到新建的文件夹目录下，输入如下命令：

```python
python3 setup.py register
```

运行后，输入 `1` ,然后依次输入 用户名 和 密码。

正常情况下会返回 Server response (200): OK 等信息。

如果返回来的信息是：

```python
urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:748)
```

此时在终端输入： `/Applications/Python\ 3.6/Install\ Certificates.command` 执行后，再次执行步骤6，直到返回 OK 为止。

**7、**上传模块到 PyPI。

打开 `终端`,cd 到新建的文件夹目录下，输入如下命令：

```python
python3 setup.py sdist upload
```

如果上传成功的话，会返回 Server response (200): OK 等信息。