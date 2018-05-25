# 4.数组

### 一、list

list是一种有序的集合，可以随时添加和删除其中的元素。list的元素可以也是一个list。

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

##### 1.



