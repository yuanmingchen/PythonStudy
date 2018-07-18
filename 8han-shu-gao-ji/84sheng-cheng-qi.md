### 8.4生成器

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

首先来看一个打印斐波那契数列的函数。

```py
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        print(b)
        a, b = b, a + b
        n = n + 1
    return 'done'
```

值得一提的是，上面的赋值语句：

```py
a, b = b, a + b
```

相当于：

```py
t = (b, a + b) # t是一个tuple
a = t[0]
b = t[1]
```

上面的函数和generator仅一步之遥。要把`fib`函数变成generator，只需要把`print(b)`改为`yield b`就可以了：

```py
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'

>>> f = fib(6)
>>> f
<generator object fib at 0x104feaaa0>


>>> for n in fib(6):  #如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator。
...     print(n)
...
1
1
2
3
5
8
```

这就是定义generator的**另一种方法**。如果一个函数定义中包含`yield`关键字，那么这个函数就不再是一个普通函数，而是一个generator。

**如何获取return的返回值？**

但是用for循环调用generator时，发现拿不到generator的return语句的返回值。如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中：

```py
>>> g = fib(6)
>>> while True:
...     try:
...         x = next(g)
...         print('g:', x)
...     except StopIteration as e:
...         print('Generator return value:', e.value)
...         break
...
g: 1
g: 1
g: 2
g: 3
g: 5
g: 8
Generator return value: done
```

**需要注意的是generator和函数的执行流程不一样。**

函数是顺序执行，遇到return语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用next\(\)的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。

来看一个例子：

```
def odd():
    print('step 1')
    yield 1
    print('step 2')
    yield(3)
    print('step 3')
    yield(5)

>>> o = odd()
>>> next(o)
step 1
1
>>> next(o)
step 2
3
>>> next(o)
step 3
5
>>> next(o)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

可以看到，odd不是普通函数，而是generator，在执行过程中，遇到yield就中断，下次又继续执行。执行3次yield后，已经没有yield可以执行了，所以，第4次调用next\(o\)就报错。

##### 3.小结

* generator是非常强大的工具，在Python中，可以简单地把列表生成式改成generator，也可以通过函数实现复杂逻辑的generator。
* 要理解generator的工作原理，它是在`for`循环的过程中不断计算出下一个元素，并在适当的条件结束`for`循环。对于函数改成的generator来说，遇到`return`语句或者执行到函数体最后一行语句，就是结束generator的指令，`for`循环随之结束。
* 请注意区分普通函数和generator函数

```py
>>> r = abs(6)
>>> r
6

>>> g = fib(6)
>>> g
<generator object fib at 0x1022ef948>
```



