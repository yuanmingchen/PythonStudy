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

### 三、函数参数

现在我们单独讨论一下函数的参数问题。

##### 1.位置参数

之所以要特别说明位置参数的概念，是因为除了位置参数外，python还可以通过参数名指定传入的参数是什么参数，这是java所没有的。

位置参数就是函数调用这在调用函数的时候，传入的参数按照位置关系一一进行匹配。比如定义一个用于计算x的n次方的函数`power（x,n）`，当我们调用函数计算2的4次方的时候写作`power（2,4）`，2和4会按照位置关系依次对x和n进行赋值，即2是第一个参数赋值给x，4是第二个参数赋值给n。

```py
def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s

>> power(2, 4)
16
>>> power(5, 3)
125
```

java中的函数参数都是位置参数。

##### 2.默认参数

函数定义的时候可以为参数设置默认值，有默认值的参数是可选的，当没有传入该参数的时候，将使用函数定义的默认值。比如`power（x，n）`可以将n的默认值设为2，当只传入x的时候，默认计算x的平方。

```py
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
>>> power(5)
25


def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)

enroll('Sarah', 'F')
enroll('Bob', 'M', 7)
enroll('Adam', 'M', city='Tianjin')
```

有多个默认参数时，调用的时候，既可以按顺序提供默认参数，比如调用`enroll('Bob', 'M', 7)`，意思是，除了`name`，`gender`这两个参数外，最后1个参数应用在参数`age`上，`city`参数由于没有提供，仍然使用默认值。

也可以不按顺序提供部分默认参数。当不按顺序提供部分默认参数时，需要把参数名写上。比如调用`enroll('Adam', 'M', city='Tianjin')`，意思是，`city`参数用传进去的值，其他默认参数继续使用默认值。

设置默认参数时，有几点要注意：

* 一是**必选参数在前，默认参数在后**，否则Python的解释器会报错（思考一下为什么默认参数不能放在必选参数前面）；
* 二是如何设置默认参数。
* 当函数有多个参数时，把**变化大的参数放前面，变化小的参数放后面**。变化小的参数就可以作为默认参数。
* 定义默认参数要牢记一点：默认参数必须指向**不变对象**！（若不遵守会有坑，下有详述）

这么做是为了降低调用函数的难度。

* **默认函数的坑：**

```py
def add_end(L=[]):
    L.append('END')
    return L

#第一次调用：
>>> add_end() 
['END']  #ok，没有问题
#第二次调用：
>>> add_end()
['END', 'END']    #有问题
#第三次调用：
>>> add_end()
['END', 'END', 'END']   #更有问题了
```

默认参数是`[]`，但是函数似乎每次都“记住了”上次添加了`'END'`后的list。

原因解释如下：

Python函数**在定义的时候**，**默认参数**`L`**的值就被计算出来了**，即`[]`，因为默认参数`L`也是一个**变量**，它指向对象`[]`，每次调用该函数，如果改变了`L`的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的`[]`了。

L所指向的对象始终没有变化，所以每次操作都会改变同一个对象（这个对象就是初始定义的`[]`）的内容，从而出现问题。

如何解决问题？把默认值改为不可变对象None就可以了。

```py
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L

#无论调用多少次，都不会有问题
>>> add_end()
['END']
>>> add_end()
['END']
```

##### 3.可变参数

在Python函数中，还可以定义可变参数。顾名思义，可变参数就是传入的参数个数是可变的，可以是1个、2个到任意个，还可以是0个。亲测貌似一个函数只能有一个可变参数，不能有多个可变参数。

我们可以通过传入一个list或者tuple类型的数组参数来实现参数个数的改变，如下：

```py
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

>>> calc([1, 2, 3])
14
>>> calc((1, 3, 5, 7))
84
```

而我们使用可变参数的话，将会更加简单，不必去定义一个数组作为参数，定义可变参数的方式是在参数的前面加一个\*。此时我们依然可以把数组作为参数，方法是在数组名前面加一个\*作为参数，意思是把这个数组的所有元素作为作为可变参数传进去。

```py
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

>>> calc(1, 2)
5
>>> calc()
0
>>> nums = [1, 2, 3]
>>> calc(*nums)  #把nums的所有元素作为可变参数传进去
14
```

##### 4.关键字参数

关键字参数允许你传入0个或任意个**含参数名的参数**，这些关键字参数在函数内部自动组装为一个**dict**。请看示例：

```py
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

#只传入必须参数：
>>> person('Michael', 30)
name: Michael age: 30 other: {}

#传入必须参数和任意个关键字参数：
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}

#先组装出一个dict，把该dict转换为关键字参数传进去：
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, city=extra['city'], job=extra['job'])
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
#简单写法：
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

函数`person`除了必选参数`name`和`age`外，还接受关键字参数`kw`。在调用该函数时，可以只传入必选参数，也可以传入任意个数的关键字参数。也可以先组装出一个dict，然后把该dict转换为关键字参数传进去。\*\*extra表示把extra这个dict的所有key-value用关键字参数传入到函数的\*\*kw参数，kw将获得一个dict，注意kw获得的dict是extra的一份拷贝，对kw的改动不会影响到函数外的extra。

##### 5.命名关键字参数

如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收city和job作为关键字参数。这种方式定义的函数如下：

```py
def person(name, age, *, city, job):
    print(name, age, city, job)

>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer

def person(name, age, *args, city='Brijing', job):
    print(name, age, args, city, job)
person('yuanmc',18,1,2,3,job = 'SSS')
person('yuanmc',18,1,2,3,city = 'Shanghai',job = 'SSS')
```

和关键字参数\*\*kw不同，命名关键字参数需要一个特殊分隔符\*，\*后面的参数被视为命名关键字参数。如果函数定义中已经有了一个**可变参数**，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了。

**命名关键字参数若没有指定默认值，则是必须参数，必须传值！！！**

命名关键字参数**必须传入参数名**，这和位置参数不同。如果没有传入参数名，调用将报错。调用时缺少参数名city和job，Python解释器把这4个参数均视为位置参数，但person\(\)函数仅接受2个位置参数。

##### 6.参数组合



