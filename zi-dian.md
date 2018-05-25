# 6.dict和set

### 一、dict

##### 1.定义

dict即字典，对应于java中的map，dict内部存放的顺序和key放入的顺序是没有关系的，具有极快的查找速度。

```py
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
```

##### 2.查找速度快的原因：

为什么dict查找速度这么快？因为dict的实现原理和查字典是一样的。假设字典包含了1万个汉字，我们要查某一个字，一个办法是把字典从第一页往后翻，直到找到我们想要的字为止，这种方法就是在list中查找元素的方法，list越大，查找越慢。

第二种方法是先在字典的索引表里（比如部首表）查这个字对应的页码，然后直接翻到该页，找到这个字。无论找哪个字，这种查找速度都非常快，不会随着字典大小的增加而变慢。

dict就是第二种实现方式，给定一个名字，比如`'Michael'`，dict在内部就可以直接计算出`Michael`对应的存放成绩的“页码”，也就是`95`这个数字存放的内存地址，直接取出来，所以速度非常快。

你可以猜到，这种key-value存储方式，在放进去的时候，必须根据key算出value的存放位置，这样，取的时候才能根据key直接拿到value。

##### 3.读取dict的值

两种方法：

###### （1类似数组的读取方式：

```py
>>> d['Jack'] = 90
>>> d['Jack']
90
```

如果key的值不存在，将会报错！可以用key in dict方法查询是否在该dict中含有指定的key值，这个方法返回一个布尔值。

```py
>>> 'Thomas' in d
False
```

注意这种方法也是为dict**添加元素**的方法，比如想要新加一个键值对，只需

```py
>>> d['hahaha'] = 86
>>> d['hahaha']
86
```

###### （2）使用get方法，get方法传一个参数（key）的时候与第一种方法相同，get方法可以有两个参数，第二个参数是当key不存在的时候的默认返回值。

```py
>>> d.get('Thomas') #报错
>>> d.get('Thomas', -1)
-1
```

##### 4.删除一个key-value

使用pop（key）的方法移除一个键值对。

```py
>>> d.pop('Bob')
75
>>> d
{'Michael': 95, 'Tracy': 85}
```

##### 5.和list比较

和list比较，dict有以下几个特点：

1. 查找和插入的速度极快，不会随着key的增加而变慢；
2. 需要占用大量的内存，内存浪费多。

而list相反：

1. 查找和插入的时间随着元素的增加而增加；
2. 占用空间小，浪费内存很少。

所以，dict是用空间来换取时间的一种方法。

##### 6.使用时注意的地方：

dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，正确使用dict非常重要，需要牢记的第一条就是dict的key必须是**不可变对象**。

这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（Hash）。

要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key：

```py
>>> key = [1, 2, 3]
>>> d[key] = 'a list'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

### 二、set

set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。set就是数学中的集合。集合的特性：无序（set也是无序的）、互异性（无重复）、确定性（对应于set每个元素都应该是不可变对象）的特性同样适用于set。

##### 1.定义一个set

a = set （list）注意，传入的参数`[1, 2, 3]`是一个list，而显示的`{1, 2, 3}`只是告诉你这个set内部有1，2，3这3个元素，显示的顺序也不表示set是有序的。。

重复元素在set中自动被过滤。

```py
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}

>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
{1, 2, 3}
```

##### 2.添加元素

通过add\(key\)方法可以添加元素到set中，可以重复添加，但不会有效果。

```py
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}
```

##### 3.删除元素

通过remove\(key\)方法可以删除元素。

```py
>>> s.remove(4)
>>> s
{1, 2, 3}
```

##### 4.集合之间的运算

set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作。

&：集合的交运算。    \|：集合的并运算。

```py
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
{2, 3}
>>> s1 | s2
{1, 2, 3, 4}
```

##### 5.set和dict对比

set和dict的唯一区别仅在于没有存储对应的value，但是，set的原理和dict一样，所以，**同样不可以放入可变对象**，因为无法判断两个可变对象是否相等，也就无法保证set内部“不会有重复元素”。

##### 6.不可变对象

str是不变对象，而list是可变对象。

对于可变对象，比如list，对list进行操作，list内部的内容是会变化的，比如：

```py
>>> a = ['c', 'b', 'a']
>>> a.sort()
>>> a
['a', 'b', 'c']
```

而对于不可变对象，比如str，对str进行操作呢：

```py
>>> a = 'abc'
>>> a.replace('a', 'A')
'Abc'
>>> a
'abc'
```

虽然字符串有个`replace()`方法，也确实变出了`'Abc'`，但变量`a`最后仍是`'abc'`，应该怎么理解呢？

我们先把代码改成下面这样：

```py
>>> a = 'abc'
>>> b = a.replace('a', 'A')
>>> b
'Abc'
>>> a
'abc'
```

要始终牢记的是，a是变量，而'abc'才是字符串对象！有些时候，我们经常说，对象a的内容是'abc'，但其实是指，a本身是一个变量，它指向的对象的内容才是'abc'。

当我们调用a.replace\('a', 'A'\)时，实际上调用方法replace是作用在字符串对象'abc'上的，而这个方法虽然名字叫replace，但却没有改变字符串'abc'的内容。相反，replace方法创建了一个新字符串'Abc'并返回，如果我们用变量b指向该新字符串，就容易理解了，变量a仍指向原有的字符串'abc'，但变量b却指向新字符串'Abc'了。

所以，对于不变对象来说，调用对象自身的任意方法，也不会改变该对象自身的内容。相反，这些方法会创建新的对象并返回，这样，就保证了不可变对象本身永远是不可变的。
