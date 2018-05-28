# 8.函数高级

函数高级用法包括切片、迭代列表生成式、生成器和迭代器这五方面。

### 一、切片

取一个list或tuple的部分元素是非常常见的操作。我们可以使用一下的普通方法：

```py
>>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
>>> [L[0], L[1], L[2]]
['Michael', 'Sarah', 'Tracy']

>>> n = 3
>>> for i in range(n):
...     r.append(L[i])
... 
>>> r
['Michael', 'Sarah', 'Tracy']
```

除了以上方法，我们还可以通过切片来实现，切片将会极大地简化我们的代码。

##### 1.普通语法

L\[0:3\]表示，从索引0开始取，直到索引3为止，但不包括索引3。即索引0，1，2，正好是3个元素。当然下表也可以是负数，代表倒数第几个元素。

```py
>>> L[0:3]   #取前三个元素
['Michael', 'Sarah', 'Tracy']

>>> L[-2:-1]  #取倒数第二个元素
['Bob']
```

所以0:3表示的是区间\[0,3\)左闭右开区间。

##### 2.数组边界切片

我们发现了，用上述语法无法取值到数组的最后一个元素，怎么办呢？当我们要取数组边界时，我们可以省略位置下表来代替一直取到边界元素。通过使用最后这种方法，我们可以取到最后一个元素。示例如下：

```py
L = list(range(100))
>>> L[:10]   #等价于L[0:10]，取前十个元素
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> L[-10:]   #取后十个元素，包括最后一个元素
[90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
>>> L[:]    #取所有元素，相当于复制一个当前数组
[0, 1, 2, 3, ..., 99]
```

##### 3.跳跃切片

跳跃切片就是每隔几个取一个元素，语法是L\[:10:2\],意思是前十个元素每两个取一个。

```py
>>> L[:10:2]
[0, 2, 4, 6, 8]
>>> L[::5]
```

##### 4.应用

切片可以用于list、tuple，不同之处在于list切片返回的还是list，tuple切片返回的还是tuple。

切片还可以用于字符串的处理，用法与上述所述是相同的，把字符串当数组就好了。

利用切片操作，实现一个trim\(\)函数，去除字符串首尾的空格，注意不要调用str的

`strip()`方法。

方法：

```py
# -*- coding: utf-8 -*-
def trim(s):
    qian=0
    hou=len(s)-1    
    while qian<len(s):
        if(s[qian]==' '):
            qian+=1;
        else:
            break;
    while hou>=0:
        if(s[hou]==' '):
            hou-=1;
        else:
            break;
    if hou==0:
        return '';
    elif hou==len(s):
        return s[qian:]
    else:
        return s[qian:hou+1]
```

### 二、迭代

##### 1.迭代的概念

如果给定一个list或tuple，我们可以通过`for`循环来遍历这个list或tuple，这种遍历我们称为迭代（Iteration）。在Python中，迭代是通过`for ... in`来完成的。

Python的`for`循环不仅可以用在list或tuple上，还可以作用在其他可迭代对象上。只要是可迭代对象，无论有无下标，都可以迭代。

##### 2.数组的迭代

`for ... in可以迭代数组list和tuple，这是我们早就知道的。`

```py
for x in list(range(101)):   #此处不强转为list也可以
    sum = sum + x
print(sum)
```

##### 3.字符串的迭代

for ... in还可以迭代字符串，与数组的迭代方式是一样的。

```py
>>> for ch in 'ABC':
...     print(ch)
...
A
B
C
```

##### 4.dict的迭代

for ... in还可以迭代dict字典，或者只是迭代字典的key值和value值。

**默认情况**下，dict迭代的是**key**。如果要迭代value，可以用`for value in d.values()`，如果要同时迭代key和value，可以用`for k, v in d.items()`。

```py
>>> d = {'a': 1, 'b': 2, 'c': 3}  #默认情况下，dict迭代的是key
>>> for key in d:
...     print(key)
...

for value in d.values():
    print(value)
for k, v in d.items():
    print(k,v)
```

##### 5.`enumerate`函数

如果要对list实现类似Java那样的下标循环怎么办？Python内置的`enumerate`函数可以把一个list变成索引-元素对，这样就可以在`for`循环中同时迭代索引和元素本身：

```py
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C
```

enumerate函数返回的是enumerate类型的对象，而不是dict类型。

##### 6.其他迭代

```py
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print(x, y)
...
1 1
2 4
3 9
```

##### 7.能否迭代的判断

上面列举了那么多可以迭代的对象，那么到底什么样的对象才可以迭代呢？如何判断呢？这就需要使用collections模块的Iterable类型判断。

```py
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```

### 三、列表生成式



