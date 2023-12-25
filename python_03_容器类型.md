[TOC]

#  1. 容器简介

* 容器概念

  代码里的容器泛指那些专门用来装其他对象的特殊数据类型。在 Python 中，最常见的内置容器类型有四种：列表、元组、字典、集合。 

* 列表(list)：通常用来存放多个同类对象 

  ~~~
  num_list = [1, 2, 3, 4, 5]
  ~~~

* 元组(tuple)：与列表非常类似，但跟列表不同，它不能被修改 

  ~~~
  >>> t1 = ('brx', 'xy')
  >>> t1[0] = 'hello' 
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: 'tuple' object does not support item assignment
  ~~~

* 字典(dict)：存放的是一个个键值对（key: value） 

  ~~~
  di = {"aa": 1, "bb": 2, "cc": 3}
  ~~~

* 集合(set): 它最大的特点是成员不能重复，所以经常用来去重（剔除重复元素） 

  ~~~
  >>> num = [1, 2, 3, 4, 5, 2, 3, 4]  
  >>> set(num) 
  {1, 2, 3, 4, 5}
  ~~~

#  1. 列表

##  1.1 列表常用操作

* 列表创建

  常用的列表创建方式有两种：字面量语法与 list() 内置函数 

  使用 [] 符号来创建一个列表字面量 

  ~~~
  num_list = [1, 2, 3, 4, 5]
  ~~~

  内置函数 list(iterable) 则可以把任何一个可迭代对象转换为列表，比如字符串 

  ~~~
  >>> list("brx") 
  ['b', 'r', 'x']
  ~~~

* 索引访问元素

  ~~~
  # 通过索引获取内容，如果索引越界，会抛出 IndexError 异常
  >>> num_list[2]               
  3
  >>> num_list[6] 
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  IndexError: list index out of range
  
  # 使用切片获取一段内容
  >>> num_list[3:]  
  [4, 5]
  
  # 删除列表中的一段内容
  >>> del num_list[1:] 
  >>> num_list
  [1]
  ~~~

* 遍历列表时获取下标：内置函数 enumerate() 

  ~~~
  >>> res_list = ["aa", "bb", "cc", "dd"]
  >>> for i, s in enumerate(res_list):    
  ...   print(i, s)
  ... 
  0 aa
  1 bb
  2 cc
  3 dd
  ~~~

  enumerate() 接收一个可选的 start 参数，用于指定循环下标的初始值（默认为 0）： 

  ~~~
  >>> res_list = ["aa", "bb", "cc", "dd"]
  >>> for i, s in enumerate(res_list, start=2): 
  ...   print(i, s)                             
  ... 
  2 aa
  3 bb
  4 cc
  5 dd
  ~~~

  enumerate() 适用于任何“可迭代对象”，因此它不光可以用于列表，还可以用于元组、字典、字符串等其他对象。 

* 列表推导式

  ~~~
  >>> num_list = [1, 2, 3, 4, 5]
  >>> res = [i for i in num_list if i % 2 == 0] 
  >>> res
  [2, 4]
  ~~~

##  1.2 理解列表的可变性

* Python 里的内置数据类型，大致上可分为可变与不可变两种 
* 可变（mutable）：列表、字典、集合 
* 不可变（immutable）：整数、浮点数、字符串、字节串、元组 
* 列表是可变的。当我们初始化一个列表后，仍然可以调用 .append()、.extend() 等方法来修改它的内容。而字符串和整数等都是不可变的——我们没法修改一个已经存在的字符串对象。 

* 为字符串追加内容

  * "+" 操作符

    ~~~
    >>> str1 = "Hello,"
    >>> str2 = "world!"
    >>> str3 = str1 + " " + str2
    >>> str3
    'Hello, world!'
    ~~~

    "+"操作符被用来将前两个字符串 str1 和 str2 进行拼接，并将结果赋值给一个新的变量 str3

  * str1 追加

    ~~~
    >>> str1           
    'Hello,'
    >>> str2
    'world!'
    
    >>> id(str1) 
    2164995160112
    
    >>> str1 += str2
    >>> str1
    'Hello,world!'
    >>> id(str1) 
    2164995161072
    ~~~

    "+"操作符被用来将前两个字符串 str1 和 str2 进行拼接，并将结果赋值变量 str1, str1 内存地址变化。

* 使用join函数追加多个字符

* 使用format格式化字符串追加

* 使用f-strings追加格式化字符串

* 为列表追加内容

  ~~~
  >>> num_list = [1, 2, 3, 4, 5]
  >>> num_list.append(12) 
  >>> num_list
  [1, 2, 3, 4, 5, 12]
  >>> num_list.extend([11, 23, 56])  
  >>> num_list
  [1, 2, 3, 4, 5, 12, 11, 23, 56]
  >>>
  >>> num_list.append([99, 88]) 
  >>> num_list
  [1, 2, 3, 4, 5, 12, 11, 23, 56, [99, 88]]
  ~~~

* 值传递与引用传递

  在`python`中，向函数传递参数的类型有两种，一种是值传递，还有一种是引用传递。

  值传递，我们可以理解为传递了一个副本过去，即变量的拷贝，修改副本值不会影响原先的值。

  ~~~
  def inedx(x):
      x = 123
      print("函数修改后的值：", x)
  
  if __name__ == '__main__':
      x = 12
      inedx(x)
      print("执行完函数之后的值：", x)
  ~~~

  函数修改后的值： 123
  执行完函数之后的值： 12

  传入的是形参，在函数中修改形参是不会改变原先的值。以上这个就是值传递。

  引用传递：传递的类型换一下，从数值类型更换为字典类型

  ~~~
  def inedx(di):
      di["x"] = 99
      print("函数修改后的值：", di)
  
  if __name__ == '__main__':
      di = {"x": 12}
      inedx(di)
      print("执行完函数之后的值：", di)
  ~~~

  函数修改后的值： {'x': 99}
  执行完函数之后的值： {'x': 99}

  在`python`中，参数传递是由解释器实现的，所以说，普通开发者，没办法直接干预参数传递方式，但是可以曲线救国，善用`return`就是其中一条，例如我们将最开始的代码修改一下，不直接修改值，而是返回一个新的值。

  ~~~
  def inedx(x):
      x = 123
      print("函数修改后的值：", x)
      return x
  
  if __name__ == '__main__':
      x = 12
      x = inedx(x)
      print("执行完函数之后的值：", x)
  ~~~

  函数修改后的值： 123
  执行完函数之后的值： 123

  即：引用传递分别有 列表、字典、集合、自定义类实例等。

  值传递分别有 字符串类型、元组、布尔类型、数值类型等。

##  1.3 常用元组操作

* 简介

  元组是一种有序的不可变容器类型。它看起来和列表非常像，只是标识符从中括号 [] 变成了圆括号 ()。由于元组不可变，所以它也没有列表那一堆内置方法，比如 .append()、.extend() 等。 

* 和列表一样，元组也有两种常用的定义方式——字面量表达式和tuple() 内置函数： 

  ~~~
  # 使用字面量语法定义元组
  >>> t = (0, 1, 2)
  # 真相：“括号”其实不是定义元组的关键标志——直接删掉两侧括号
  # 同样也能完成定义，“逗号”才是让解释器判定为元组的关键
  >>> t1 = 1, 2, 3
  >>> t1
  (1, 2, 3)
  
  >>> # 使用 tuple(iterable) 内置函数
  >>> t2 = tuple('brx')     
  >>> t2
  ('b', 'r', 'x')
  ~~~

  因为元组是一种不可变类型，所以下面这些操作都不会成功 

  ~~~
  >>> t = (0, 1, 2)
  >>> t1.append(4) 
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  AttributeError: 'tuple' object has no attribute 'append'
  
  >>> #元组成员不允许被删除
  >>> del t1[1] 
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: 'tuple' object doesn't support item deletion
  
  >>> # 可以删除整个tuple
  >>> del t1
  >>> t1
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  NameError: name 't1' is not defined. Did you mean: 't2'?
  ~~~

* 返回多个结果，其实就是返回元组 

  ~~~
  def inedx():
      x = 111
      y = 222
      z = 333
      return x, y, z
  
  if __name__ == '__main__':
      res = inedx()
      print(res)
      print(type(res))
  ~~~

  (111, 222, 333)
  <class 'tuple'>

* 没有元组推导式 

  而是返回了一个生成器（generator）对象。因此它是生成器推导式，而非元组推导式。 

  ~~~
  >>> t1 = (1, 2, 3, 4, 5)
  >>> res = (i for i in t1 if i % 2 == 0) 
  >>> res
  <generator object <genexpr> at 0x000001609C729A10>
  >>> tuple(res) 
  (2, 4)
  ~~~

* 存放结构化数据

  和列表不同，在同一个元组里出现不同类型的值是很常见的事情, 因此元组经常用来存放结构化数据。 

  ~~~
  >>> t1 = (1, 'brx', 'hh') 
  >>> t1
  (1, 'brx', 'hh')
  ~~~

  正因为元组有这个特点，所以 Python 为我们提供了一个特殊的元组类型：具名元组。 

##  1.4 具名元组

* 访问元组成员时，需要用数字索引来定位

  ~~~
  >>> t1 = 'a', 'b', 'c' 
  >>> t1
  ('a', 'b', 'c')
  >>> t1[0]
  'a'
  >>> t1[1] 
  'b'
  >>> t1[-1] 
  'c'
  ~~~

  元组经常用来存放结构化数据，但只能通过数字来访问元组成员其实特别不方便 

  为了解决这个问题，我们可以使用一种特殊的元组：具名元组（namedtuple）。具名元组在保留普通元组功能的基础上，允许为元组的每个成员命名，这样你便能通过名称而不止是数字索引访问成员。 

* 创建具名元组需要用到 namedtuple() 函数 

  ~~~
  >>> from collections import namedtuple
  >>> t1 = namedtuple("t1", "a, b, c")
  >>> t1
  <class '__main__.t1'>
  >>> t1.a
  <property object at 0x0000017AA5D712C8>
  >>> p = t1(1, 2, 3)
  >>> p
  t1(a=1, b=2, c=3)
  >>> p.a
  1
  >>> p.b
  2
  >>> p.c
  3
  ~~~

##  1.5 字典常用操作

* 简介

  跟列表和元组比起来，字典是一种更为复杂的容器结构。它所存储的内容不再是单一维度的线性序列，而是多维度的 key: value 键值对。 

* 通过key来获取值

  ~~~
  >>> d1 = {"aa": 1, "bb": 2, "cc": 3}
  >>> d1["aa"]
  1
  ~~~

* 可变类型，可增加新的key

  ~~~
  >>> d1["dd"] = 4
  >>> d1
  {'aa': 1, 'bb': 2, 'cc': 3, 'dd': 4}
  ~~~

* key不可重复，可覆盖

  ~~~
  >>> d1["dd"] = 44
  >>> d1
  {'aa': 1, 'bb': 2, 'cc': 3, 'dd': 44}
  ~~~

* 遍历字典

  当我们直接遍历一个字典对象时，会逐个拿到字典所有的 key 

  ~~~
  >>> for k in d1:
  >>>    print(k)
  aa
  bb
  cc
  dd
  ~~~

  遍历字典时同时获取 key 和 value，需要使用字典的.items() 方法 

  ~~~
  >>> for k, v in d1.items():
  >>>    print(k, v)
      
  aa 1
  bb 2
  cc 3
  dd 44
  ~~~

* 获取所有的key或者value

  ~~~
  >>> for k in d1.keys():
  >>>    print(k)
      
  aa
  bb
  cc
  dd
  >>> for v in d1.values():
  >>>    print(v)
      
  1
  2
  3
  44
  ~~~

* 访问不存在的字典键

  会抛出异常：KeyError

  ~~~
  >>> d1["e"]
  Traceback (most recent call last):
    File "<input>", line 1, in <module>
  KeyError: 'e'
  ~~~

  处理方式

  ~~~
  try:
      e = d1["ee"]
  except KeyError:
      e = 0
  ~~~

  或者

  ~~~
  >>> d1
  {'aa': 1, 'bb': 2, 'cc': 3, 'dd': 44}
  >>> 1 in d1
  False
  >>> "aa" in d1
  True
  >>> "ee" in d1
  False
  ~~~

* get()方法

  ~~~
  >>> d1.get("aa")
  1
  ~~~

  dict.get(key, default) 方法接收一个 default 参数，当访问的键不存在时，方法会返回 default 作为默认值 ，不会报错。

* del

  ~~~
  >>> d1
  {'aa': 1, 'bb': 2, 'cc': 3, 'dd': 44, 'ee': 5}
  >>> d1["ee"] = [1, 2]
  >>> d1
  {'aa': 1, 'bb': 2, 'cc': 3, 'dd': 44, 'ee': [1, 2]}
  >>> del d1["ee"]
  >>> d1
  {'aa': 1, 'bb': 2, 'cc': 3, 'dd': 44}
  ~~~

* 删除的键不存在就会抛出异常

  ~~~
  >>> del d1["ee"]
  Traceback (most recent call last):
    File "<input>", line 1, in <module>
  KeyError: 'ee'
  ~~~

  所以想要安全的删除某个键，需要加上一段异常捕获

* pop

  调用pop方法，传入默认值None，键不存在也不会抛异常

  ~~~
  >>> d1
  {'aa': 1, 'bb': 2, 'cc': 3}
  >>> d1.pop("cc", None)
  3
  >>> d1
  {'aa': 1, 'bb': 2}
  >>> d1.pop("qq", None)
  >>> d1
  {'aa': 1, 'bb': 2}
  ~~~

* setdefault 

  它用于在字典中查找指定键 如果键存在， 则返回对应的值； 如果键不存在，则在字典中添加该键，并将其值设置为指定的默认值

  ~~~
  >>> d = {"a": 1, "b": 2} 
  >>> d                   
  {'a': 1, 'b': 2}
  >>> d.setdefault("c", 3)
  3
  >>> d
  {'a': 1, 'b': 2, 'c': 3}
  
  >>> d.setdefault("c", 3)
  3
  >>> d.setdefault("c")    
  3
  ~~~

* 字典推导式

  ~~~
  >>> {k:v*2 for k, v in d.items()} 
  {'a': 2, 'b': 4, 'c': 6}
  ~~~

##  1.6 字典的有序性和无序性

- Python 3.6版本前字典是无序的

  按照某种顺序将内容存进字典后，无法按照原有顺序将其取出来。

  由字典的底层所实现，底层是哈希表数据结构。

- Python 3.6为字典引进了一个改进

  优化底层实现，同样的字典比之前版本可节约多达25%的内存。

  副作用：字典变的有序了

- collections 模块

  有序字典对象：OrderedDict

  ```
  >>> from collections import OrderedDict
  >>> d = OrderedDict()
  >>> d
  OrderedDict()
  ```

  Python 3.7以前版本里保证字典有序。

* 细微区别：键的顺序也会作为比对条件

  OrderedDict比普通字典更加清晰表达了字典的有序性。

  ~~~
  >>> d1
  {'bb': 2, 'aa': 1}
  >>> d2
  {'bb': 2, 'aa': 1}
  >>> d1 == d2
  True
  
  >>> d3 = OrderedDict(aa=1, bb=2)
  >>> d4 = OrderedDict(bb=2, aa=1)
  >>> d3
  OrderedDict([('aa', 1), ('bb', 2)])
  >>> d4
  OrderedDict([('bb', 2), ('aa', 1)])
  >>> d3 == d4
  False
  ~~~

##  1.7 集合常用操作

* 简介

  集合是一种无序的可变容器类型，它最大的特点就是成员不能重复。集合字面量的语法和字典很像，都是使用大括号包裹，但集合里装的是一维的值 {value, ...}，而不是键值对 {key: value,...}。 

* 可变重复类型，元素不能重复，语法和字典很像，大括号

  ~~~
  >>> s1 = {"aa", "bb", "cc", "aa"}
  >>> s1
  {'bb', 'aa', 'cc'}
  >>> type(s1)
  <class 'set'>
  ~~~

* 空集合

  ~~~
  >>> s2 = set()
  >>> s2
  set()
  >>> type(s2)
  <class 'set'>
  ~~~

* 集合推导式

  ~~~
  >>> l1 = [1, 2, 3, 4]
  >>> s1 = {i*2 for i in l1}
  >>> s1
  {8, 2, 4, 6}
  >>> type(s1)
  <class 'set'>
  ~~~

* 集合是一种可变类型，使用add可追加新元素

  ~~~
  >>> s1
  {1, 2, 3}
  >>> s1.add(4)
  >>> s1
  {1, 2, 3, 4}
  ~~~

* 不可变集合：内置类型 frozenset

  ~~~
  >>> s1 = frozenset([1, 2, 3])
  >>> s1
  frozenset({1, 2, 3})
  >>> s1.add(4)
  Traceback (most recent call last):
    File "<input>", line 1, in <module>
  AttributeError: 'frozenset' object has no attribute 'add'
  ~~~

* 集合运算

* 交集

  ~~~
  >>> s1 = {1, 2, 3, 4, 5}
  >>> s2 = {5, 6, 7, 8}
  >>> s1 & s2
  {5}
  ~~~

* 并集

  ~~~
  >>> s1 | s2
  {1, 2, 3, 4, 5, 6, 7, 8}
  ~~~

* 差集

  ~~~
  >>> s1 - s2
  {1, 2, 3, 4}
  ~~~

* 集合只能放可哈希对象

  ~~~
  >>> s1 = {1, 2, 3, [4, 5, 6]}
  Traceback (most recent call last):
    File "<input>", line 1, in <module>
  TypeError: unhashable type: 'list'
  ~~~

##  1.8 了解对象的可哈希性

* 简介

  字典底层使用了哈希表数据结构，其实集合也一样。当我们把某个对象放进集合或者作为字典的键使用
  时，解释器都需要对该对象进行一次哈希运算，得到哈希值，然后再进行后面的操作。 

* 可哈希对象

  ~~~
  hash(obj)
  ~~~

  如果对象是可哈希的，hash 函数会返回一个整型结果，否则将会报 TypeError 错误。 

- 不可变内置类型都是可哈希的

  整型的hash值就是自身值

  ```
  >>> hash(1000)
  1000
  ```

- 可变类型都无法正常计算hash值

- 不可变类型添加可变类型值也不可hash

  ```
  >>> hash((1, 2, 3))
  2528502973977326415
  >>> hash((1, 2, 3, [4, 7]))
  Traceback (most recent call last):
    File "<input>", line 1, in <module>
  TypeError: unhashable type: 'list'
  ```

##  1.9 深拷贝与浅拷贝

###  1.9.1 浅拷贝

* 浅拷贝：copy

  ~~~
  >>> import copy
  >>> l1 = [1, 2, 3]
  >>> l2 = copy.copy(l1)
  >>> l1
  [1, 2, 3]
  >>> l2
  [1, 2, 3]
  >>> id(l1) 
  1514458663872
  >>> id(l2) 
  1514458195584
  
  >>> l1[0] = 11
  >>> l1
  [11, 2, 3]
  >>> l2
  [1, 2, 3]
  ~~~

* 推导式产生浅拷贝对象

  ~~~
  >>> d1 = {"aa": 1}
  >>> d1
  {'aa': 1}
  >>> d2 = {key:value for key, value in d1.items()}
  >>> d2
  {'aa': 1}
  
  >>> d1["aa"] = 11
  >>> d1
  {'aa': 11}
  >>> d2
  {'aa': 1}
  ~~~

* 内置构造函数

  ```
  >>> d3 = dict(d1.items())
  >>> d3
  {'aa': 11}
  ```

* 切片

  ```
  >>> ll = [1, 2, 3]
  >>> ll
  [1, 2, 3]
  >>> l3 = ll[:]
  >>> l3
  [1, 2, 3]
  ```

* 类型自身提供的方法

  ```
  >>> l1 = [1, 2, 3]
  >>> l1
  [1, 2, 3]
  >>> l1.copy()
  [1, 2, 3]
  >>> d1 = {"aa": 1}
  >>> d1
  {'aa': 1}
  >>> d1.copy()
  {'aa': 1}
  ```

###  1.9.2 深拷贝

* 浅拷贝无法解决嵌套对象修改问题

  ```
  >>> import copy
  >>> l1 = [1, 2, 3, ["a", "b"]]
  >>> l1
  [1, 2, 3, ['a', 'b']]
  >>> l2 = copy.copy(l1)
  >>> l2
  [1, 2, 3, ['a', 'b']]
  
  >>> l1.append(4)
  >>> l1
  [1, 2, 3, ['a', 'b'], 4]
  >>> l2
  [1, 2, 3, ['a', 'b']]
  ```

  ```
  >>> l1[3].append("c")
  >>> l1
  [1, 2, 3, ['a', 'b', 'c'], 4]
  >>> l2
  [1, 2, 3, ['a', 'b', 'c']]
  ```

* deepcopy

  ```
  >>> l1
  [1, 2, 3, ['a', 'b', 'c'], 4]
  >>> l2 = copy.deepcopy(l1)
  >>> l2
  [1, 2, 3, ['a', 'b', 'c'], 4]
  
  >>> l1[3].append("d")
  >>> l1
  [1, 2, 3, ['a', 'b', 'c', 'd'], 4]
  >>> l2
  [1, 2, 3, ['a', 'b', 'c'], 4]
  ```

#  2. 编程建议

##  2.1 按需返回替代容器

* 按需生成

  ```
  >>> res = range(100)
  >>> res
  range(0, 100)
  ```

  返回一个range对象，而不是一次性返回所有结果，不会耗费大量内存和时间

* 生成器

  简介

  生成器（generator）是 Python 里的一种特殊的数据类型。顾名思义，它是一个不断给调用方“生成”内容的类型。定义一个生成器，需要用到生成器函数与 yield 关键字 

  yied与return，return一次性返回所有结果，yied可以逐步调用。next方法。

  ~~~
  def generate_even(max_number):
      for i in range(0, max_number):
          if i % 2 == 0:
              yield i
  
  for i in generate_even(10):
      print(i)
  ~~~

  0
  2
  4
  6
  8

##  2.2 容器的底层实现

* 避开列表的性能缺陷

  往列表中添加数据：append、insert

  ```
  l1 = []
  a = time.time()
  for i in range(100000000):
      l1.append(i)
  b = time.time()
  
  print(b - a)  # 10.833044290542603
  ```

  ```
  l1 = []
  a = time.time()
  for i in range(100000000):
      l1.insert(i, 0)
  b = time.time()
  
  print(b - a)  # 19.096739530563354
  ```

  append添加数据时间更快。性能更好。

* 使用集合判断成员是否存在

  集合之所以搜索更快，是其底层使用哈希表数据结构，判断集合中是否存在某个对象obj，只需要算出它的哈希值，直接去哈希表对应位置检查obj是否存在即可。

  ```
  >>> l1
  [1, 2, 3, 4]
  >>> 1 in set(l1)
  True
  ```

  除了集合，字典进行 k in ......查询同样很快。

##  2.3 快速合并字典

* 动态解包表达式

  ```
  >>> d1 = {"aa": 1}
  >>> d2 = {"bb": 2}
  >>> d3 = {**d1, **d2}
  >>> d3
  {'aa': 1, 'bb': 2}
  
  >>> d2
  {'bb': 2}
  >>> d2 = {**d1, **d2}
  >>> d2
  {'aa': 1, 'bb': 2}
  ```

* python3.9支持|运算符

  ```
  >>> d1 = {"aa": 1} 
  >>> d2 = {"bb": 2} 
  >>> d1
  {'aa': 1}
  >>> d2   
  {'bb': 2}
  >>> d1 | d2
  {'aa': 1, 'bb': 2}
  ```

##  2.4 使用有序字典去重

* 集合去重，得到的结果会丢失集合内元素原有顺序。

* 有序字典 OrderdDict

  * 键是有序的

  * 键不会重复

  * 字典的值默认为None

* 别在遍历列表的时候同步修改(remove会导致原列表索引位置改变)

* 让函数返回namedtuple

  对未来可能会变动的多返回值函数来说，一开始使用namedtuple类型会对结果进行建模，后期减少改动。