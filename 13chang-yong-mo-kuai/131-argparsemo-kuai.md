# 13.1 **argparse模块** {#一argparse模块}

## 1、文档

首先必须要给出它的**文档地址**：[https://docs.python.org/3/howto/argparse.html](https://docs.python.org/3/howto/argparse.html)

**参考博客**：[https://blog.csdn.net/guoyajie1990/article/details/76739977](https://blog.csdn.net/guoyajie1990/article/details/76739977)（比文档写的清楚易懂）

## 2、作用

然后来看看它是用来做什么的：

#### 官方的描述：

python标准库模块argparse用于**解析命令行参数**，编写用户友好的命令行界面，该模块还会自动生成帮助信息，并在所给参数无效时报错。

#### 我的描述：

这个库很简单，作用比较单一，就是用于在命令行执行python程序时**处理命令行传入的参数**的，我们写了一个程序，想要在命令行执行，除了直接使用 python +文件名 执行，还可以像执行普通脚本一样，在执行python命令时传入各种参数，让python程序执行更加灵活，很多东西不必再程序里面写死了，可以作为命令参数传入。

举例：

```
$ python prog.py a b c
```

如上所示，a、b、c就是命令行的参数，使用argparse模块我们就可以在程序中获取这些参数，还可以像真正的脚本一样给用户帮助信息，帮助用户理解每个参数的作用。

## 3、使用示例

```py
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

上面就是使用argparse模块的一个例子，这个例子很简单（这里只有解析参数的过程，真正的脚本计算结果的程序很简单，并且与这个模块无关，所以没有给出），在执行脚本时需要传入一些int类型的数，然后回车，脚本会返回这些数中的最大值，还要一个可选参数--sum，使用该参数会改变脚本功能，不再是找出最大值，而是返回所有数的和。

首先来看一下效果吧，将上述代码保存为arg\_parse.py，在命令行运行该脚本。使用-h选项可以查看帮助信息：

```
$ python prog.py -h  #查看脚本帮助信息
usage: prog.py [-h] [--sum] N [N ...]
Process some integers.
positional arguments:
 N           an integer for the accumulator
optional arguments:
 -h, --help  show this help message and exit
 --sum       sum the integers (default: find the max)
```

再来看一下执行结果和报错功能：

```py
$ python prog.py 1 2 3 4
4
$ python prog.py 1 2 3 4 --sum
10

$ python prog.py a b c
usage: prog.py [-h] [--sum] N [N ...]
prog.py: error: argument N: invalid int value: 'a'
```

## 4、类和参数介绍

现在详细讲解一下上面例子中的方法和参数：

**注意：**为了区分我们使用的方法的参数和要解析的参数，下面将方法的参数称作**方法参数**，要解析的参数称作**解析参数**。

其实很简单：

* 首先创建一个ArgumentParser类的实例，这个类恰如其名，参数解析器嘛，用来解析参数的，我们可以在创建参数解析器时传入一些**方法参数**，比如示例中的description**方法参数**，就是用于介绍脚本的功能的，当用户用-h查看脚本帮助时会看到这个介绍。

```py
argparse.ArgumentParser(description='Process some integers.')
```

当然除了description，还有一些别的**方法参数**，不过用的不多：

```py
argparse.ArgumentParser(prog=None,#传入一个字符串，自定义程序名
                        usage=None, #不适用自动生成的用法信息，自己定义用法说明
                        epilog=None, #与description，程序额外描述信息，位于参数说明之后
                        parents=[],  #继承别的参数解析器
                        formatter_class=argparse.HelpFormatter, #自己定制帮助信息
                        prefix_chars='-', #自定义参数选项的前缀，默认为'-'           
                        fromfile_prefix_chars=None,              
                        argument_default=None,
                        conflict_handler='error', 
                        add_help=True)  #设为False将禁用-h,--help帮助选项
```

* 第二步是重点，为参数解析器添加我们想解析的参数：

```py
parser.add_argument('integers', metavar='N', type=int, nargs='+',
                      help='an integer for the accumulator')
parser.add_argument('--sum', dest='accumulate', action='store_const',const=sum, default=max,
                      help='sum the integers (default: find the max)')
```

重点内容是add\_argument的**方法参数**，也就是**解析参数**的属性，常用的**方法参数**如下：

```py
argumentParser.add_argument(name or flags...[,action][,nargs][,const][,default]
                           [,type][,choices][,required][,help][,metavar][,dest])
```

##### （1）**name 或 flags**

指定一个可选参数或位置参数，可选参数是以’-‘为前缀的参数，剩下的就是位置参数

```py
>>> parser = argparse.ArgumentParser(prog='PROG')
>>> parser.add_argument('-f', '--foo')
>>> parser.add_argument('bar')
>>> parser.parse_args(['BAR'])
Namespace(bar='BAR', foo=None)
>>> parser.parse_args(['BAR', '--foo', 'FOO'])
Namespace(bar='BAR', foo='FOO')
>>> parser.parse_args(['--foo', 'FOO'])
usage: PROG [-h] [-f FOO] bar
PROG: error: too few arguments
```

##### （2）**action参数**

指定应该如何处理命令行参数，预置的操作有以下几种:

* action=’store’ 仅仅保存参数值，为action默认值
* action=’store\_const’ 与store基本一致，但store\_const只保存const关键字指定的值
* action=’store\_true’或’store\_false’ 与store\_const一致，只保存True和False
* action=’append’ 将相同参数的不同值保存在一个list中
* action=’count’ 统计该参数出现的次数
* action=’help’ 输出程序的帮助信息
* action=’version’ 输出程序版本信息

除了上述几种预置action，还可以自定义action，需要继承Action并覆盖**call**和**init**方法，这是高级用法，需要的话请自行查阅文档。

##### （3）**default**

如果参数可以缺省，default指定命令行参数不存在时的参数值。

```py
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', default=42)
>>> parser.parse_args('--foo 2'.split())
Namespace(foo='2')
>>> parser.parse_args(''.split())
Namespace(foo=42)
```

##### （4）**type**

默认情况下，ArgumentParser对象将命令行参数保存为字符串。但通常命令行参数应该被解释为另一种类型，如 float或int。通过指定type,可以对命令行参数执行**类型检查和类型转换**。通用的内置类型和函数可以直接用作type参数的值：

```py
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('foo', type=int)
>>> parser.add_argument('bar', type=open)
>>> parser.parse_args('2 temp.txt'.split())
Namespace(bar=<_io.TextIOWrapper name='temp.txt' encoding='UTF-8'>, foo=2)
```

##### （5）**choices**

将命令行参数的值限定在一个范围内，超出范围则报错

```py
#例一
>>> parser = argparse.ArgumentParser(prog='game.py')
>>> parser.add_argument('move', choices=['rock', 'paper', 'scissors'])
>>> parser.parse_args(['rock'])
Namespace(move='rock')
>>> parser.parse_args(['fire'])
usage: game.py [-h] {rock,paper,scissors}
game.py: error: argument move: invalid choice: 'fire' (choose from 'rock',
'paper', 'scissors')

#例二
>>> parser = argparse.ArgumentParser(prog='doors.py')
>>> parser.add_argument('door', type=int, choices=range(1, 4))
>>> print(parser.parse_args(['3']))
Namespace(door=3)
>>> parser.parse_args(['4'])
usage: doors.py [-h] {1,2,3}
doors.py: error: argument door: invalid choice: 4 (choose from 1, 2, 3)
```

##### （6）**required**

指定命令行参数是否必需，默认通过-f –foo指定的参数为可选参数。

```py
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', required=True)
>>> parser.parse_args(['--foo', 'BAR'])
Namespace(foo='BAR')
>>> parser.parse_args([])
usage: argparse.py [-h] [--foo FOO]
argparse.py: error: option --foo is required
```

##### （7）**dest**

dest 允许自定义ArgumentParser的参数属性名称

```py
#例一
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('bar')
>>> parser.parse_args('XXX'.split())
Namespace(bar='XXX')

#例二
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('-f', '--foo-bar', '--foo')
>>> parser.add_argument('-x', '-y')
>>> parser.parse_args('-f 1 -x 2'.split())
Namespace(foo_bar='1', x='2')
>>> parser.parse_args('--foo 1 -y 2'.split())
Namespace(foo_bar='1', x='2')


#例三
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', dest='bar')
>>> parser.parse_args('--foo XXX'.split())
Namespace(bar='XXX')

```



