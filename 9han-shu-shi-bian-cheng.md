# 9.函数式编程

## 一、高阶函数

### 1.map函数

##### （1）作用

##### `map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回。

##### （2）使用方法

`map()`函数接收两个参数，第一个参数是一个函数，第二个参数是一个`Iterable`，即`map（函数名，Iterable）`

##### （3）举例

```py
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]


>>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

map\(\)传入的第一个参数是f，即函数对象本身。由于结果r是一个Iterator，Iterator是惰性序列，因此通过list\(\)函数让它把整个序列都计算出来并返回一个list。

### 2.reduce函数

##### （1）作用

`reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的**下一个元素**做累积计算，其效果就是：

```
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

##### （2）使用方法

`reduce()`函数接受2个参数，与map类似，第一个参数是一个函数，第二个参数是一个`Iterable`，即`reduce（函数名，Iterable）。`

##### （3）举例

```py
>>> from functools import reduce
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9]) //实际上用sum函数就可以求和
25


>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> reduce(fn, [1, 3, 5, 7, 9])
13579
```

```py
#字符串转int的实现，即int()方法
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> def char2num(s):
...     digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
...     return digits[s]
...
>>> reduce(fn, map(char2num, '13579'))
13579


#上面的的程序归纳为一个函数str2int
from functools import reduce

DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def str2int(s):
    def fn(x, y):
        return x * 10 + y
    def char2num(s):
        return DIGITS[s]
    return reduce(fn, map(char2num, s))
```

### 3.filter函数

##### （1）作用

`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。



