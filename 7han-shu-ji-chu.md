# 7.函数基础

### 一、python内置函数

##### 1.查询函数的帮助信息：

help函数，参数为要查询信息的函数名。

```py
help(abs)
#输出：
Help on built-in function abs in module builtins: 

abs(x, /) 
    Return the absolute value of the argument.
```

##### 2.调用内置函数

例如abs函数，绝对值函数。

```py
>>> abs(100)
100
>>> abs(-20)
20
>>> abs(12.34)
12.34

>>> max(1, 2) #max可以接受任意多个参数（至少两个）
2
>>> max(2, 3, 1, -5)
3
```

##### 3.数据类型转换

与java的格式强转不同，python的格式强转是以函数的方式实现的。

```py
>>> int('123')
123
>>> int(12.34)
12
>>> float('12.34')
12.34
>>> str(1.23)
'1.23'
>>> str(100)
'100'
>>> bool(1)
True
>>> bool('')
False
```



