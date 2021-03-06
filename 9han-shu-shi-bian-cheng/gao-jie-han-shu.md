## 9.1高阶函数

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

`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。filter函数恰如其名，就像一个过滤器一样，根据函数的返回值，把不符合条件的元素过滤掉。

##### （2）使用方法

和`map()`类似，`filter()`也接收一个函数和一个序列。不同之处在于，filter的函数参数应该返回一个布尔类型。具体使用为：`filter（函数名，Iterable）`

##### （3）举例

```py
#将奇数选择出来，把偶数过滤掉
def is_odd(n):
    return n % 2 == 1

list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
# 结果: [1, 5, 9, 15]
```

###### 经典示例：用filter求素数

计算[素数](http://baike.baidu.com/view/10626.htm)的一个方法是[埃氏筛法](http://baike.baidu.com/view/3784258.htm)，它的算法理解起来非常简单：

首先，列出从`2`开始的所有自然数，构造一个序列：

2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...

取序列的第一个数`2`，它一定是素数，然后用`2`把序列的`2`的倍数筛掉：

3, ~~4~~, 5, ~~6~~, 7, ~~8~~, 9, ~~10~~, 11, ~~12~~, 13, ~~14~~, 15, ~~16~~, 17, ~~18~~, 19, ~~20~~, ...

取新序列的第一个数`3`，它一定是素数，然后用`3`把序列的`3`的倍数筛掉：

5, ~~6~~, 7, ~~8~~, ~~9~~, ~~10~~, 11, ~~12~~, 13, ~~14~~, ~~15~~, ~~16~~, 17, ~~18~~, 19, ~~20~~, ...

取新序列的第一个数`5`，然后用`5`把序列的`5`的倍数筛掉：

7, ~~8~~, ~~9~~, ~~10~~, 11, ~~12~~, 13, ~~14~~, ~~15~~, ~~16~~, 17, ~~18~~, 19, ~~20~~, ...

不断筛下去，就可以得到所有的素数。

用Python来实现这个算法，可以先构造一个从`3`开始的奇数序列：

```py
#用filter求素数，首先构造一个从3开始的奇数序列作为初始序列
def _odd_iter():
    n = 1
    while True:
        n = n + 2
        yield n

def _not_divisible(n):
    return lambda x: x % n > 0

#这个生成器先返回第一个素数2，然后，利用filter()不断产生筛选后的新的序列。 
def primes():
    yield 2
    it = _odd_iter() # 初始序列
    while True:
        n = next(it) # 返回序列的第一个数
        yield n
        it = filter(_not_divisible(n), it) # 构造新序列

#使用方法：由于primes()也是一个无限序列，所以调用时需要设置一个退出循环的条件：
# 打印1000以内的素数:
for n in primes():
    if n < 1000:
        print(n)
    else:
        break
```

### 4.sorted函数

##### （1）作用

对传入的`Iterable`参数按照一定规则进行排序，返回排序后的`Iterable。`

##### （2）使用方法

可以只传入一个数组，按照从小到大排序。

它还可以接收一个key函数（作为第二个参数）来实现自定义的排序，即：`sorted（数组，key=函数名）`

上面的排序都是从小到大排序，如果想从大到小排序，传入参数`reverse=True`即可，即：

```py
sorted（数组，key=函数名，reverse=True）
```

**注意：**默认情况下，对字符串排序，是按照ASCII的大小比较的

##### （3）举例

```py
#直接对list进行排序
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]

#使用abs绝对值函数作为key进行绝对值排序
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]

#字符串排序，按照ASCII码大小排序
>>> sorted(['bob', 'about', 'Zoo', 'Credit'])
['Credit', 'Zoo', 'about', 'bob']

#想忽略大小写按照从A-Z排序
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
['about', 'bob', 'Credit', 'Zoo']

#想忽略大小写按照从Z-A排序
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```



