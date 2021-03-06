### 5.2循环

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

Python提供一个range\(\)函数，可以生成一个整数序列，再通过list\(\)函数可以转换为list。Python3 中 range\(\) 函数返回的结果是一个整数序列的对象，而不是列表。比如range\(5\)生成的序列是从0开始**小于5**的整数，range\(101\)就可以生成0-100的整数序列，计算如下：

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

```py
# -*- coding: utf-8 -*-
sum = 0
for x in range(101):    
    sum = sum + x
print("for计算的0-100之和是："+str(sum))

sum = 0
i = 0
ls = [1,2,3,5,8,9,11,25,36,78,125,652]
while i<len(ls):
    sum = sum + ls[i]
    i = i+1

print("while计算的数组之和是："+str(sum))

sum = 0
i = 0
while i<len(ls):
    if ls[i]>10:
        break
    sum = sum + ls[i]
    i = i+1
print("while+break计算的数组小于10的元素之和是："+str(sum))

sum = 0
i = 0
while i<len(ls):
    if ls[i]<10:
        i = i + 1
        continue
    sum = sum + ls[i]
    i = i+1
print("while+continue计算的数组大于10的元素之和是："+str(sum))
```



