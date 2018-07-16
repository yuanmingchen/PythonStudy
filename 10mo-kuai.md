# **10.模块**

## 一、使用模块

### 1.介绍

Python本身就内置了很多非常有用的模块，只要安装完毕，这些模块就可以立刻使用。

### 2.举例

我们以内建的`sys`模块为例，编写一个`hello`的模块：

```py
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

' a test module '

__author__ = 'Michael Liao'

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```



