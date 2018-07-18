### 7.2定义函数

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

调用函数时，如果参数个数错误，Python解释器会自动检查出来，并抛出`TypeError`。

```py
>>> my_abs(1, 2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: my_abs() takes 1 positional argument but 2 were given
```

但是如果参数类型错误，Python解释器无法检测出来，需要我们在定义函数的时候人为判断处理或者抛出错误。数据类型检查可以用内置函数`isinstance()`实现：

```py
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```

添加了参数检查后，如果传入错误的参数类型，函数就可以抛出一个错误：

```py
>>> my_abs('A')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in my_abs
TypeError: bad operand type
```

##### 5.返回多个值

不同于java函数只可以有一个返回值，python的函数可以有多个返回值，但其实就是一个tuple。

比如在游戏中经常需要从一个点移动到另一个点，给出坐标、位移和角度，就可以计算出新的新的坐标：

```py
import math  #导入math包

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny


>>> x, y = move(100, 100, 60, math.pi / 6)
>>> print(x, y)
151.96152422706632 70.0
```

但其实这只是一种假象，Python函数返回的仍然是单一值：

```py
>>> r = move(100, 100, 60, math.pi / 6)
>>> print(r)
(151.96152422706632, 70.0)
```

原来返回值是一个tuple！但是，在语法上，返回一个tuple可以省略括号，而多个变量可以同时接收一个tuple，按位置赋给对应的值，所以，Python的函数返回多值其实就是返回一个tuple，但写起来更方便。

再来一个求解一元二次方程的解的例子。

```py
# -*- coding: utf-8 -*-

import math

def quadratic(a, b, c):

    dd = b*b-4*a*c
    if dd<0:
        return '无解'
    elif dd==0:
        return -b/(2*a)
    else:
        return (-b+math.sqrt(dd))/(2*a),(-b-math.sqrt(dd))/(2*a)
# 测试:
print('quadratic(1, 1.5, 1) =', quadratic(1, 1.5, 1))
print('quadratic(1, 2, 1) =', quadratic(1, 2, 1))
print('quadratic(2, 3, 1) =', quadratic(2, 3, 1))
print('quadratic(1, 3, -4) =', quadratic(1, 3, -4))

if quadratic(2, 3, 1) != (-0.5, -1.0):
    print('测试失败')
elif quadratic(1, 3, -4) != (1.0, -4.0):
    print('测试失败')
else:
    print('测试成功')
```

##### 6.小结

* 定义函数时，需要确定函数名和参数个数；
* 如果有必要，可以先对参数的数据类型做检查；
* 函数体内部可以用`return`随时返回函数结果；
* 函数执行完毕也没有`return`语句时，自动`return None`。
* 函数可以同时返回多个值，但其实就是一个tuple。



