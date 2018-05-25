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

##### 2.



