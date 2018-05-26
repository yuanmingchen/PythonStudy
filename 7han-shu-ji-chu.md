# 7.函数基础

### 一、python内置函数

python有许多内置函数，具体的使用文档请参考：

[https://yiyibooks.cn/xx/python\_352/library/functions.html](https://yiyibooks.cn/xx/python_352/library/functions.html)

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

hex：将整数转换为以“0x”为前缀的小写十六进制字符串

bin：将整数转换为二进制字符串。结果是一个有效的Python表达式。如果 x 不是 Python int 类型的对象，那么就定义一个 \_\_index\_\_\(\) 方法，返回是一个整数。

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

##### 4.函数重命名

函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”：

```py
>>> a = abs # 变量a指向abs函数
>>> a(-1) # 所以也可以通过a调用abs函数
1
```

### 二、



