### 8.2迭代

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



