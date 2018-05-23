# 3.字符串编码

#### 1.字符编码

ASCII码：英文编码，无法表示中文。只占用一个字节。

但是要处理中文显然一个字节是不够的，至少需要两个字节，而且还不能和ASCII编码冲突，所以，中国制定了GB2312编码，用来把中文编进去。各国都制定了自己的编码。

Unicode编码把所有语言都统一到一套编码里，这样就不会再有乱码问题了。Unicode标准也在不断发展，但最常用的是用两个字节表示一个字符（如果要用到非常偏僻的字符，就需要4个字节）。现代操作系统和大多数编程语言都直接支持Unicode。

如果把ASCII编码的A用Unicode编码，**只需要在前面补0就可以**，因此，A的Unicode编码是00000000 01000001。

如果统一成Unicode编码，乱码问题从此消失了。但是，如果你写的文本基本上全部是英文的话，用Unicode编码比ASCII编码需要多一倍的存储空间，在存储和传输上就十分不划算。所以，本着节约的精神，又出现了把Unicode编码转化为“**可变长编码**”的UTF-8编码。**UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，**常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间。UTF-8编码有一个额外的好处，就是ASCII编码实际上可以被看成是UTF-8编码的一部分，所以，大量只支持ASCII编码的历史遗留软件可以在UTF-8编码下继续工作。

**现在计算机系统通用的字符编码工作方式：**

（1）在计算机**内存**中，统一使用**Unicode编码**，当需要**保存**到硬盘或者需要**传输**的时候，就转换为**UTF-8编码**。

（2）用记事本**编辑**的时候，从文件读取的UTF-8字符被**转换为Unicode字符**到内存里，编辑**完成**后，**保存**的时候再把Unicode**转换为UTF-8**保存到文件。

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001387245992536e2ba28125cf04f5c8985dbc94a02245e000/0)![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001387245979827634fd6204f9346a1ae6358d9ed051666000/0)

#### 二、python字符串编码

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

（1）类似C语言的格式化：

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



