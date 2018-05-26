# 7.函数基础

### 一、python内置函数

python有许多内置函数，具体的使用文档请参考：

[https://yiyibooks.cn/xx/python\_352/library/functions.html](https://yiyibooks.cn/xx/python_352/library/functions.html)

##### 1.查询函数的帮助信息：

help函数，参数为要查询信息的函数名。

```py
help(abs)
#输出：
Help on built-in function abs in module builtins: 

abs(x, /) 
    Return the absolute value of the argument.
```

##### 2.调用内置函数

例如abs函数，绝对值函数。

```py
>>> abs(100)
100
>>> abs(-20)
20
>>> abs(12.34)
12.34

>>> max(1, 2) #max可以接受任意多个参数（至少两个）
2
>>> max(2, 3, 1, -5)
3
```

##### 3.数据类型转换

与java的格式强转不同，python的格式强转是以函数的方式实现的。

hex：将整数转换为以“0x”为前缀的小写十六进制字符串

bin：将整数转换为二进制字符串。结果是一个有效的Python表达式。如果 x 不是 Python int 类型的对象，那么就定义一个 \_\_index\_\_\(\) 方法，返回是一个整数。

```py
>>> int('123')
123
>>> int(12.34)
12
>>> float('12.34')
12.34
>>> str(1.23)
'1.23'
>>> str(100)
'100'
>>> bool(1)
True
>>> bool('')
False
```

##### 4.函数重命名

##### 函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”：

```py
>>> a = abs # 变量a指向abs函数
>>> a(-1) # 所以也可以通过a调用abs函数
1
```

### 二、定义函数

##### 1.定义方式

在Python中，定义一个函数要使用def语句，依次写出函数名、括号、括号中的参数和冒号:，然后，在缩进块中编写函数体，函数的返回值用return语句返回。

```py
# -*- coding: utf-8 -*-
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
```

请注意，函数体内部的语句在执行时，一旦执行到`return`时，函数就执行完毕，并将结果返回。因此，函数内部通过条件判断和循环可以实现非常复杂的逻辑。

如果没有`return`语句，函数执行完毕后也会返回结果，只是结果为`None`。`return None`可以简写为`return`。

##### 2.函数的导入和使用

把my\_abs\(\)的函数定义保存为abstest.py文件，那么，可以在该文件的当前目录下启动Python解释器，用from abstest import my\_abs来导入my\_abs\(\)函数，注意abstest是文件名（不含.py扩展名）：

```py
from abstest import my_abs
>>> my_abs(-9)
9
```

##### 3.空函数和pass的使用

如果想定义一个什么事也不做的空函数，可以用pass语句：

```py
def nop():
    pass
```

pass语句什么都不做，那有什么用？实际上pass可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个pass，让代码能运行起来。

`pass`还可以用在其他语句里，比如：

```py
if age >= 18:
    pass
```

##### 4.参数检查

调用函数时，如果参数错误，Python解释器会自动检查出来，并抛出TypeError，这些错误包括参数的类型错误、参数的个数错误。

```py
>>> my_abs(1, 2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: my_abs() takes 1 positional argument but 2 were given
```

除此之外，还有一些类型和个数都正确但是却不恰当的参数，需要我们在定义函数的时候认为处理或者抛出错误。



