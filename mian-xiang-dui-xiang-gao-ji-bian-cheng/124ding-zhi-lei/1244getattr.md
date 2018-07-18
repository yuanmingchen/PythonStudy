### 12.4.4\_\_getattr\_\_

正常情况下，当我们调用类的方法或属性时，如果不存在，就会报错。比如定义`Student`类：

```py
class Student(object):

    def __init__(self):
        self.name = 'Michael'
```

调用`name`属性，没问题，但是，调用不存在的`score`属性，就有问题了：

```py
>>> s = Student()
>>> print(s.name)
Michael
>>> print(s.score)
Traceback (most recent call last):
  ...
AttributeError: 'Student' object has no attribute 'score'
```

错误信息很清楚地告诉我们，没有找到`score`这个attribute。

要避免这个错误，除了可以加上一个`score`属性外，Python还有另一个机制，那就是写一个`__getattr__()`方法，动态返回一个属性。修改如下：

```py
class Student(object):

    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self, attr):
        if attr=='score':
            return 99
```

当调用不存在的属性时，比如score，Python解释器会试图调用\_\_getattr\_\_\(self, 'score'\)来尝试获得属性，这样，我们就有机会返回score的值：

```py
>>> s = Student()
>>> s.name
'Michael'
>>> s.score
99
```

返回函数也是完全可以的：

```py
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
```

只是调用方式要变为：

```py
>>> s.age()
25
```

**注意，只有在没有找到属性的情况下，才调用**`__getattr__`**，已有的属性，比如**`name`**，不会在**`__getattr__`**中查找。**

此外，注意到任意调用如`s.abc`都会返回`None`，这是因为我们定义的`__getattr__`**默认返回就是**`None`。要让class只响应特定的几个属性，我们就要按照约定，**抛出**`AttributeError`**的错误：**

```py
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
        raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)
```

这实际上可以把一个类的所有属性和方法调用全部**动态化处理**了，不需要任何特殊手段。

这种完全动态调用的特性有什么实际作用呢？作用就是，**可以针对完全动态的情况作调用。**

举个例子：

现在很多网站都搞REST API，比如新浪微博、豆瓣啥的，调用API的URL类似：

* [http://api.server/user/friends](http://api.server/user/friends)
* [http://api.server/user/timeline/list](http://api.server/user/timeline/list)

如果要写SDK，给每个URL对应的API都写一个方法，那得累死，而且，API一旦改动，SDK也要改。

利用完全动态的`__getattr__`，我们可以写出一个**链式调用**：

```py
class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__
```

试试：

```py
>>> Chain().status.user.timeline.list
'/status/user/timeline/list'
```

这样，无论API怎么变，SDK都可以根据URL实现完全动态的调用，而且，不随API的增加而改变！

还有些REST API会把参数放到URL中，比如GitHub的API：

```py
GET /users/:user/repos
```

调用时，需要把`:user`替换为实际用户名。如果我们能写出这样的链式调用：

```py
Chain().users('michael').repos
```

就可以非常方便地调用API了。有兴趣的童鞋可以试试写出来。

个人理解：

使用链式调用之前：sdk需要为每一个URLz指定实际的资源/方法位置，即类似于在某配置文件中配置所有URL与资源方法的对应关系，当URL请求时，首先在配置文件中查找资源或方法位置，再进行访问，比如配置文件如下：

* [http://api.server/user/friends](http://api.server/user/friends)：/server/user/friends

* [http://api.server/user/timeline/list](http://api.server/user/timeline/list):：/server/user/timeline/list

  …………

使用了链式调用之后，当增加新的api或者修改原来的api时时，不必在配置文件中添加新的内容，可以使用链式调用方法根据url自动生成资源位置，动态加载资源目录而非静态配置。

