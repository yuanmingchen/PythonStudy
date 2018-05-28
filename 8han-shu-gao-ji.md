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

列表生成式即List Comprehensions，是Python内置的非常简单却强大的可以用来创建list的生成式。

##### 1.生成普通列表

前面已经讲过，如果我们需要生成一个连续数字数组很简单：

```py
>>> list(range(1, 11))
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

##### 2.列表生成式

但是，如果我们想生成更加复杂的数组呢，比如\[1x1, 2x2, 3x3, ..., 10x10\]，难道用循环吗？循环肯定可以实行需求，但是通过使用列表生成式，我们可以只写一行代码来实现需求。

###### （1）列表生成式简单使用：

```py
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

###### （2）列表生成式还可以使用条件判断：

```py
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```

###### （3）列表生成式还可以使用多种循环：

```py
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

###### （4）列表生成式还可以迭代字典：

```py
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```

###### （5）列表生成式还可以使用函数：

```py
>>> L = ['Hello', 'World', 'IBM', 'Apple']
>>> [s.lower() for s in L]
['hello', 'world', 'ibm', 'apple']
```

### 四、生成器

通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

为了解决这个问题，我们可以使用生成器。

##### 1.什么是生成器

如果列表元素可以按照某种算法推算出来，那我们可以在循环的过程中不断推算出后续的元素。这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器：generator。

##### 2.怎样创建一个生成器

要创建一个generator，有很多种方法。

###### （1）第一种方法

第一种方法很简单，只要把一个列表生成式的`[]`改成`()`，就创建了一个generator：

```py
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>


>>> next(g)
0
>>> next(g)
1
>>> next(g)
4
>>> next(g)
9
>>> next(g)
16
>>> next(g)
25
>>> next(g)
36


#使用循环遍历生成器
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print(n)
... 

0
1
4
9
16
25
36
49
64
81
```

创建L和g的区别仅在于最外层的\[\]和\(\)，L是一个list，而g是一个generator。如果要一个一个打印出来，可以通过`next()`函数获得generator的下一个返回值。

generator保存的是算法，每次调用`next(g)`，就计算出`g`的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出`StopIteration`的错误。

###### （2）第二种方法





