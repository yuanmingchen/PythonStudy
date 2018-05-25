# 5.条件判断和循环

### 一、条件判断

##### 1.非常简单，就不用说了。

值得注意的是，python会根据缩进来判断if和while的开始和结束的，而并非java和C语言的括号，python强制要求缩进。

if和elif与后面的判断条件用空格分开，判断条件不用括号，注意还要有冒号。

```py
age = 20
if age >= 6:
    print('teenager')
elif age >= 18:
    print('adult')
else:
    print('kid')

s = input('birth: ')
birth = int(s)
if birth < 2000:
    print('00前')
else:
    print('00后')
```

```py
# -*- coding: utf-8 -*-
print("请输入体重（kg）：")
weight = float(input())
print("请输入身高（cm）：")
height = float(input())
bmi = weight/(height*height)
if bmi<18.5:
    print("过轻")
elif bmi<23:
    print("正常")
elif bmi<28:
    print("过重")
elif bmi<32:
    print("肥胖")
else:
    print("过度肥胖")
```

### 二、循环

##### 1.for...in循环

for...in循环用于遍历数组中的元素较为方便

```py
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
输出：
Michael
Bob
Tracy
```

Python提供一个range\(\)函数，可以生成一个整数序列，再通过list\(\)函数可以转换为list。比如range\(5\)生成的序列是从0开始**小于5**的整数，range\(101\)就可以生成0-100的整数序列，计算如下：

```py
sum = 0
for x in range(101):
    sum = sum + x
print(sum)
```

##### 2.while循环

只要条件满足，就不断循环，条件不满足时退出循环。比如我们要计算100以内所有**奇数**之和，可以用while循环实现：

```py
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```

##### 3.break关键字和continue关键字

这两个关键字的用法与java和C语言是一样的。程序遇到break会跳出当前的循环，直接执行当前循环结束后的代码，而continue关键字则是终止本次循环，开始下一次循环。

break：提前结束整个循环。

continue：提前结束本轮循环。

```py
#break的用法示例
n = 1
while n <= 100:
    if n > 10: # 当n = 11时，条件满足，执行break语句
        break # break语句会结束当前循环
    print(n)
    n = n + 1
print('END')

#continue的用法示例
n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue
    # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)
```



