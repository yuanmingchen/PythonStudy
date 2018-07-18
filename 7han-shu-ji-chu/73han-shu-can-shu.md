### 7.3函数参数

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
yuanmc 18 (1, 2, 3) Beijing SSS
yuanmc 18 (1, 2, 3) Shanghai SSS
```

和关键字参数\*\*kw不同，命名关键字参数需要一个特殊分隔符\*，\*后面的参数被视为命名关键字参数。如果函数定义中已经有了一个**可变参数**，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了。

**命名关键字参数若没有指定默认值，则是必须参数，必须传值！！！**

命名关键字参数**必须传入参数名**，这和位置参数不同。如果没有传入参数名，调用将报错。调用时缺少参数名city和job，Python解释器把这4个参数均视为位置参数，但person\(\)函数仅接受2个位置参数。

命名关键字参数也可以有默认值，当有默认值时，可以不传入该参数。

##### 6.参数组合

参数定义的顺序必须是：**必选参数、默认参数、可变参数、命名关键字参数和关键字参数。**

```py
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)

>>> f1(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}
>>> f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
>>> f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
>>> f1(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
>>> f2(1, 2, d=99, ext=None)
a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}
```

虽然可以组合多达5种参数，但不要同时使用太多的组合，否则函数接口的可理解性很差。

##### 7.小结

* Python的函数具有非常灵活的参数形态，既可以实现简单的调用，又可以传入非常复杂的参数。
* 默认参数一定要用不可变对象，如果是可变对象，程序运行时会有逻辑错误！
* 要注意定义可变参数和关键字参数的语法：
* `*args`是可变参数，args接收的是一个tuple；
* `**kw`是关键字参数，kw接收的是一个dict。
* 以及调用函数时如何传入可变参数和关键字参数的语法：
* 可变参数既可以直接传入：`func(1, 2, 3)`，又可以先组装list或tuple，再通过`*args`传入：`func(*(1, 2, 3))`；
* 关键字参数既可以直接传入：`func(a=1, b=2)`，又可以先组装dict，再通过`**kw`传入：`func(**{'a': 1, 'b': 2})`。
* 使用`*args`和`**kw`是Python的习惯写法，当然也可以用其他参数名，但最好使用习惯用法。
* 命名的关键字参数是为了限制调用者可以传入的参数名，同时可以提供默认值。
* 定义命名的关键字参数在没有可变参数的情况下不要忘了写分隔符`*`，否则定义的将是位置参数。



