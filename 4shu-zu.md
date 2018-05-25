# 4.数组5

### 一、list

list是一种有序的集合，可以随时添加和删除其中的元素。list的元素可以也是一个list。结合了java中的数组和ArrayList两种的优点，既可以改变数组长度增加元素，还可以根据坐标位置读取元素。

```py
>>> p = ['asp', 'php']
>>> s = ['python', 'java', p, 'scheme']
```

##### 1.定义方式

用中括号括起来一些元素表示一个list，元素中间用逗号分隔。

```py
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
```

##### 2.读取指定位置元素

```py
>>> classmates[0]
'Michael'
>>> classmates[1]
'Bob'
>>> classmates[2]
'Tracy'
>>> classmates[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range

>>> classmates[-1]
'Tracy'
>>> classmates[-2]
'Bob'
>>> classmates[-3]
'Michael'
>>> classmates[-4]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

##### 3.修改指定位置的元素

```py
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
```

##### 4.增加元素

两种方式，append方法在数组最后增加一个元素，参数为要增加的元素；insert方法在指定位置插入一个元素，第一个参数为位置，第二个参数为要插入的元素。

```py
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']

>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```

##### 5.删除元素

pop方法移除数组的一个元素，参数为要移除的元素所在的位置坐标。若省略参数，默认移除数组最后一个元素。

```py
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']

>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```

##### 6.其他方法

len方法返回数组的元素个数。

```py
>>> s = ['python', 'java', ['asp', 'php'], 'scheme']
>>> len(s)
4

>>> L = []
>>> len(L)
0
```

### 二、tuple

tuple和list非常类似，也是有序列表，但是tuple一旦初始化就不能修改，相当于java中被加了的final关键字的数组。

##### 1.定义方式

用括号括起来的元素集合，也是用逗号分隔元素。**值得注意的是，当还有一个元素的时候，也需要在元素后面加上一个逗号用来表示这是一个tuple，若果不加逗号，python将不会识别为一个tuple。**

```py
>>> classmates = ('Michael', 'Bob', 'Tracy')

>>> t = (1) #不加逗号，将被识别为1，是一个整数变量
>>> t
1

>>> t = (1,) #加逗号，将被识别为一个tuple数组
>>> t
(1,)

>>> t = ()
>>> t
()
```

##### 2.tuple数组无法增加修改删除数组中的元素。但是也并非是绝对不可变，当元素是指针的时候就可以，其实数组的元素还是没有变化，因为它始终存储的是那一个指针，但是指针的指向的内容发生改变可以变相实现“可变的tuple”（其实tuple的元素还是没变）。

详细演示如下：

```
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```

这个tuple定义的时候有3个元素，分别是'a'，'b'和一个list。不是说tuple一旦定义后就不可变了吗？怎么后来又变了？

别急，我们先看看定义的时候tuple包含的3个元素：

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001387269705541ad608276b6f7426ca59b8c2b19947243000/0)

当我们把list的元素'A'和'B'修改为'X'和'Y'后，tuple变为：

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001387269768140c7d5ca167342402989dfc75343fe900b000/0)

表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向'a'，就不能改成指向'b'，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！

理解了“**指向不变**”后，要创建一个**内容也不变的tuple**怎么做？那就必须保证**tuple的每一个元素本身也不能变。**

##### 3.读取元素的值

```py
>>> classmates = ('Michael', 'Bob', 'Tracy')
>>> classmates[1] #不加逗号，将被识别为1，是一个整数变量
'Bob'
```



