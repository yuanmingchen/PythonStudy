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

## 二、返回函数

### 1.介绍

高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回。

### 2.举例

首先看普通的求和函数：

```py
def calc_sum(*args):
    ax = 0
    for n in args:
        ax = ax + n
    return ax
```

但是，如果不需要立刻求和，而是在后面的代码中，根据需要再计算怎么办？可以不返回求和的结果，而是返回求和的函数：

```py
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```

当我们调用`lazy_sum()`时，返回的并不是求和结果，而是求和函数：

```py
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
```

调用函数`f`时，才真正计算求和的结果：

```py
>>> f()
25
```

在这个例子中，我们在函数`lazy_sum`中又定义了函数`sum`，并且，内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，**相关参数和变量都保存在返回的函数中，**这种称为“**闭包**（Closure）”的程序结构拥有极大的威力。

请再注意一点，当我们调用`lazy_sum()`时，每次调用都会返回一个新的函数，即使传入相同的参数：

```py
>>> f1 = lazy_sum(1, 3, 5, 7, 9)
>>> f2 = lazy_sum(1, 3, 5, 7, 9)
>>> f1==f2
False
#f1()和f2()的调用结果互不影响。
```

### 3.闭包

##### （1）定义

一个函数的返回值是一个函数，返回的函数在其定义内部引用了局部变量`args`，相关参数和变量都保存在返回的函数中**，**这种程序结构称为“**闭包**（Closure）”。

当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易。

另一个需要注意的问题是，返回的函数并**没有立刻执行**，而是直到调用了`f()`才执行。

##### （2）举例

```py
#函数返回一个函数数组，其实每个函数都是会返回i*i，并未算出结果，真正执行的时候才会计算
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
```

在上面的例子中，每次循环，都创建了一个新的函数，然后，把创建的3个函数都返回了。

你可能认为调用`f1()`，`f2()`和`f3()`结果应该是`1`，`4`，`9`，但实际结果是：

```py
>>> f1()
9
>>> f2()
9
>>> f3()
9
```

全部都是9！原因就在于返回的函数引用了变量i，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量i已经变成了3，因此最终结果为9。

* **注意：返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量。**

如果一定要引用循环变量怎么办？方法是再**创建一个函数**，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，**已绑定到函数参数的值不变**：

```py
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()，g中持有j是f（j）被调用时的j
    return fs
```

再看看结果：

```py
>>> f1, f2, f3 = count()
>>> f1()
1
>>> f2()
4
>>> f3()
9
```

##### （3）练习

利用闭包返回一个计数器函数，每次调用它返回递增整数：

```py
# -*- coding:utf-8 -*-
def ziranshu():
    n = 0
    while True:
        n = n+ 1
        yield n
def counter():
    it = ziranshu()
    def f():
        return next(it)
    return f
i = 0

counterA = counter()
print(counterA(), counterA(), counterA(), counterA(), counterA()) # 1 2 3 4 5
counterB = counter()
if [counterB(), counterB(), counterB(), counterB()] == [1, 2, 3, 4]:
    print('测试通过!')
else:
    print('测试失败!')
```

## 三、匿名函数

### 1.介绍

当我们在传入函数时，有些时候，不需要显式地定义函数，直接传入匿名函数更方便。

### 2.举例

在Python中，对匿名函数提供了有限支持。还是以`map()`函数为例，计算f\(x\)=x2时，除了定义一个`f(x)`的函数外，还可以直接传入匿名函数：

```py
>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

通过对比可以看出，匿名函数`lambda x: x * x`实际上就是：

```py
def f(x):
    return x * x
```

关键字`lambda`表示匿名函数，冒号前面的`x`表示函数参数。

匿名函数有个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。

用匿名函数有个好处，因为函数没有名字，不必担心函数名冲突。此外，匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数：

```py
>>> f = lambda x: x * x
>>> f
<function <lambda> at 0x101c6ef28>
>>> f(5)
25
```

同样，也可以把匿名函数作为返回值返回，比如：

```py
def build(x, y):
    return lambda: x * x + y * y
```

## 四、装饰器

### 1.什么是装饰器

由于函数也是一个对象，而且函数对象可以被赋值给变量，所以，通过变量也能调用该函数。函数对象有一个`__name__`属性，可以拿到函数的名字：

```py
>>> def now():
...     print('2015-3-25')
...
>>> f = now
>>> f()
2015-3-25

>>> now.__name__
'now'
>>> f.__name__
'now'
```

现在，假设我们要增强`now()`函数的功能，比如，在函数调用前后自动打印日志，但又不希望修改`now()`函数的定义，**这种在代码运行期间动态增加功能的方式**，称之为“装饰器”（Decorator）。

### 2.举例

本质上，decorator就是一个返回函数的高阶函数。所以，我们要定义一个能打印日志的decorator，可以定义如下：

```py
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

观察上面的`log`，因为它是一个decorator，所以接受一个函数作为参数，并返回一个函数。我们要借助Python的@语法，把decorator置于函数的定义处：

```py
@log
def now():
    print('2015-3-25')
```



