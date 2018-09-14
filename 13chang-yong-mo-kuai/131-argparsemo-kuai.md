# 13.1 **argparse模块** {#一argparse模块}

## 1、文档

首先必须要给出它的**文档地址**：[https://docs.python.org/3/howto/argparse.html](https://docs.python.org/3/howto/argparse.html)

**参考博客**：[https://blog.csdn.net/guoyajie1990/article/details/76739977](https://blog.csdn.net/guoyajie1990/article/details/76739977)（比文档写的清楚易懂）

## 2、作用

然后来看看它是用来做什么的：

#### 官方的描述：

python标准库模块argparse用于**解析命令行参数**，编写用户友好的命令行界面，该模块还会自动生成帮助信息，并在所给参数无效时报错。

#### 我的描述：

这个库很简单，作用比较单一，就是用于在命令行执行python程序时**处理命令行传入的参数**的，我们写了一个程序，想要在命令行执行，除了直接使用 python +文件名 执行，还可以像执行脚本一样，在执行python命令时传入各种参数，让python程序执行更加灵活，很多东西不必再程序里面写死了，可以作为命令参数传入。

举例：

```
$ python prog.py a b c
```

如上所示，a、b、c就是命令行的参数，使用argparse模块我们就可以在程序中获取这些参数，还可以像真正的脚本一样给用户帮助信息，帮助用户理解每个参数的作用。

## 3、使用方法

```
#arg_parse.py
#coding:utf-8
import argparse

parser = argparse.ArgumentParser(description='Process some integers.')
parser.add_argument('integers', metavar='N', type=int, nargs='+',help='an integer for the accumulator')
parser.add_argument('--sum', dest='accumulate', action='store_const',const=sum, default=max,
                      help='sum the integers (default: find the max)')
args = parser.parse_args()
print(args.accumulate(args.integers))
```



