#### 3.2.python字符串编码

##### 1.python的字符串是用Unicode编码的。

##### 2.单个字符的编码和查看编码

对于单个字符的编码，Python提供了ord\(\)函数获取字符的整数表示，chr\(\)函数把编码转换为对应的字符。

```py
ord('A')    #输出：65
ord('中')    #输出：20013
chr(66)    #输出：'B'
chr(25991)    #输出：'文'
```

##### 3.python字符串的编码与解码

由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。Python对bytes类型的数据用带b前缀的单引号或双引号表示：x = b'ABC'。要注意区分'ABC'和b'ABC'，前者是str，后者虽然内容显示得和前者一样，但bytes的每个字符都只占用一个字节。

（1）**编码：**以Unicode表示的str通过encode\(\)方法可以编码为指定的bytes（在bytes中，无法显示为ASCII字符的字节，用\x\#\#显示），encode\(\)方法是str类型的方法，输出的是bytes类型。

```py
'ABC'.encode('ascii') #转换为了b'ABC'
'中文'.encode('utf-8') #转换为了b'\xe4\xb8\xad\xe6\x96\x87'
'中文'.encode('ascii')
'''
报错：转换为了Traceback (most recent call last):
File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
'''
```

（2）**解码：**反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是bytes。要把bytes变为str，就需要用decode\(\)方法，decode\(\)方法是bytes类型的方法，输出的是str类型。

```py
b'ABC'.decode('ascii')#解码为'ABC'
b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')#解码为'中文'
b'\xe4\xb8\xad\xff'.decode('utf-8')
'''
当前编码无法解码时报错：
Traceback (most recent call last):
  ...
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
'''
```

##### 4.str和bytes在len\(\)方法方面的区别

str的len\(\)方法输出的是字符的个数，而bytes类型的len方法输出的是字节的个数。

举例：

```py
>>> len('ABC')
3
>>> len('中文')
2
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```

可见，1个中文字符经过UTF-8编码后通常会占用3个字节，而1个英文字符只占用1个字节。

##### 5.python源码文件的编码

当python源码中有中文的时候，也可能会出现乱码问题，需要两个步骤避免乱码：

（1）源码文件开头指定编码格式，注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

```py
# -*- coding: utf-8 -*-
#源码内容......
```

（2）文本编辑器也要使用UTF-8 without BOM编码来进行文件编辑工作。

##### 6.格式化

格式化的两种方法：

（1）类似C语言的格式化：

%运算符就是用来格式化字符串的。在字符串内部，%s表示用字符串替换，%d表示用整数替换，有几个%?占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个%?，括号可以省略。

常见的占位符有：

占位符    替换内容

%d            整数

%f           浮点数

%s          字符串

%x       十六进制整数

```py
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
>>> print('%2d-%02d' % (3, 1))#输出： 3-01，注意3的前面有个空格占位，%02d表示整数表示为两位，
#若不足两位，前面补0
>>> print('%.2f' % 3.1415926)#输出：3.14，保留两位小数
>>> 'Age: %s. Gender: %s' % (25, True)
'Age: 25. Gender: True'
>>> 'growth rate: %d %%' % 7
'growth rate: 7 %'
```

注意：

* 如果你不太确定应该用什么，%s永远起作用，全都用%s，它会把任何数据类型转换为字符串。
* 有些时候，字符串里面的%是一个普通字符怎么办？这个时候就需要转义，用%%来表示一个%：

```py
>>> 'Age: %s. Gender: %s' % (25, True) #所有的输出都用%s
'Age: 25. Gender: True'
>>> 'growth rate: %d %%' % 7 #需要输出%的情况
'growth rate: 7 %'
```

（2）format\(\)方法：另一种格式化字符串的方法是使用字符串的format\(\)方法，它会用传入的参数依次替换字符串内的占位符{0}、{1}……，不过这种方式写起来比%要麻烦得多：

```py
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```

##### 7.总结

Python 3的字符串使用**Unicode**，直接支持多语言。

当str和bytes互相转换时，需要指定编码。最常用的编码是**UTF-8**。Python当然也支持其他编码方式，比如把Unicode编码成GB2312：

```py
>>> '中文'.encode('gb2312')
b'\xd6\xd0\xce\xc4'
```

但这种方式纯属**自找麻烦**，如果没有特殊业务要求，请牢记仅使用UTF-8编码。

格式化字符串的时候，可以用Python的交互式环境测试，方便快捷。

