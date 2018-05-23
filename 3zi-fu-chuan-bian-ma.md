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

1.python的字符串是用Unicode编码的。

2.对于单个字符的编码，Python提供了ord\(\)函数获取字符的整数表示，chr\(\)函数把编码转换为对应的字符。

```py
ord('A')    #输出：65
ord('中')    #输出：20013
chr(66)    #输出：'B'
chr(25991)    #输出：'文'
```

3.由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。Python对bytes类型的数据用带b前缀的单引号或双引号表示：x = b'ABC'。要注意区分'ABC'和b'ABC'，前者是str，后者虽然内容显示得和前者一样，但bytes的每个字符都只占用一个字节。

以Unicode表示的str通过encode\(\)方法可以编码为指定的bytes（在bytes中，无法显示为ASCII字符的字节，用\x\#\#显示）：

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



