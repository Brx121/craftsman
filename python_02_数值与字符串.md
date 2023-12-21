[TOC]

#  1. 基础知识

* Python整型

  动态类型。自动管理整型的内存分配和释放，而不需要程序员手动的关心内存管理。这也是Python相对C/C++的一个优势，Python的整型可以表示任意大小的整数。

##  1.1 数值基础

* 三种数值类型：整型(int)、浮点型(float)、复数类型(complex)

  创建数值

  ~~~
  >>> score = 12  # int
  >>> score = 1.2  # float
  >>> com = 1 + 2j  # complex
  ~~~

  int 类型与 float 类型相互转换

  ~~~
  >>> score = 1.2
  >>> score = int(1.2)  # float -> int
  >>> score
  1
  >>> brx = 123
  >>> float(brx)  # int -> float
  123.0
  ~~~

  数字若是很长，可添加 _ 分割，使其更加易读

  ~~~
  >>> 1_000_000 + 1 
  1000001
  ~~~

* 浮点数精度问题

  问题

  ~~~
  >>> 0.1 + 0.2
  0.30000000000000004
  ~~~

  原因：计算机是一个二进制的世界，它能够表示所有的数字，都是通过0和1两个数模拟而来的。比如二进制的110代表十进制的6，这套机制适用于整数类型，当需要小于1的浮点数时，计算机就做不到绝对的精准了。

* 解决：内置模块decimal

  ~~~
  >>> from decimal import Decimal
  >>> Decimal('0.1') + Decimal('0.2')
  Decimal('0.3')
  ~~~

  注意：必须使用字符串来表示数字进行运算

  ~~~
  >>> print(Decimal(1.2))
  1.1999999999999999555910790149937383830547332763671875
  >>> 
  >>> print(Decimal('1.2'))
  1.2
  ~~~

##  1.2 布尔值也是数字

* bool值: True、False

  int 类型的子类型，可以直接当做1 0 来使用

  ~~~
  >>> True + 1
  2
  >>> False + 2
  2
  ~~~

##  1.3 字符串常用操作

* 将字符串当做序列操作

  * 遍历

    ~~~
    >>> s1 = 'hello'
    >>> for i in s1:
    ...   print(i)
    ... 
    h
    e
    l
    l
    o
    ~~~

  * 切片

    ~~~
    >>> s1
    'hello'
    >>> s1[::-1]
    'olleh'
    >>> 
    >>> s1[:2]
    'he'
    
    # 反转还可以使用reversed reversed会返回一个可迭代对象
    >>> reversed(s1)
    <reversed object at 0x7f6002b41080>
    >>> ''.join(reversed(s1))
    'olleh'
    ~~~

* 字符串格式化

  * 基于 % 格式

    ~~~
    >>> 'hello %s' %'world'
    'hello world'
    
    >>> 'hello %d' %1    
    'hello 1'
    ~~~

  * str.format() 格式

    ~~~
    >>> "hello, {}".format("world") 
    'hello, world'
    ~~~

  * f-string 格式

    ~~~
    >>> f"hello {'world'}" 
    'hello world'
    ~~~

* 拼接多个字符串

  * join

    ~~~
    >>> l1 = ['a', 'b', 'c', 'd']
    >>> ''.join(l1) 
    'abcd'
    >>>
    >>> '-'.join(l1) 
    'a-b-c-d'
    ~~~

  * +=

    ~~~
    >>> s = ''
    >>> for i in l1:
    ...   s += i
    ... 
    >>> s
    'abcd'
    ~~~

##  1.4 常用字符串方法

###  1.4.1 字符串拼接 切割 替换

* join()

  `join(iterable)`方法用于将字符串列表中的元素连接为一个字符串。

  - 方法作用：将字符串列表中的元素连接为一个字符串。
  - 方法参数：`iterable`为可迭代对象，如字符串列表、元组等。
  - 方法返回值：返回连接后的新字符串。

  ~~~
  >>> l1 = ['a', 'b', 'c', 'd']
  >>> ''.join(l1) 
  'abcd'
  >>>
  >>> '-'.join(l1) 
  'a-b-c-d'
  ~~~

* format()

  `format()`方法用于格式化字符串，将占位符替换为指定的值。`format()`方法所包含的内容非常丰富，具体已经在另一篇文章中详细介绍。

  - 方法作用：格式化字符串，将占位符替换为指定的值。
  - 方法参数：占位符可以使用位置参数或关键字参数，并使用大括号 `{}`表示。
  - 方法返回值：返回格式化后的新字符串。

  ~~~
  >>> "hello, {}".format("world") 
  'hello, world'
  ~~~

* split()

  `split(separator, maxsplit)`方法用于将字符串分割为子字符串。

  - 方法作用：将字符串分割为子字符串。
  - 方法参数：`separator`为分隔符，`maxsplit`指定分割的次数，默认为全部分割。
  - 方法返回值：返回分割后的子字符串列表。

  ~~~
  >>> str1 = "Hello World"
  >>> parts = str1.split(" ")
  >>> parts                   
  ['Hello', 'World']
  ~~~

* splitlines()

  `splitlines(keepends)`方法用于将字符串按行分割为子字符串。

  - 方法作用：将字符串按行分割为子字符串。
  - 方法参数：`keepends`指定是否保留换行符，默认为 `False`。
  - 方法返回值：返回分割后的子字符串列表。

  ~~~
  >>> str1 = "Hello\nWorld"
  >>> lines = str1.splitlines()
  >>> lines                    
  ['Hello', 'World']
  ~~~

* rsplit()

  `rsplit(separator, maxsplit)`方法用于从右侧开始将字符串分割为子字符串。

  - 方法作用：从右侧开始将字符串分割为子字符串。
  - 方法参数：`separator`为分隔符，`maxsplit`指定分割的次数，默认为全部分割。
  - 方法返回值：返回分割后的子字符串列表。

  ~~~
  >>> str1 = "Hello World"
  >>> parts = str1.rsplit(" ", 1)
  >>> parts                       
  ['Hello', 'World']
  ~~~

* rpartition()

  `rpartition(separator)`方法用于将字符串从最后一个匹配的分隔符处分割为三部分。

  - 方法作用：将字符串从最后一个匹配的分隔符处分割为三部分。
  - 方法参数：`separator`为分隔符。
  - 方法返回值：返回一个包含三个元素的元组，第一个元素是分隔符之前的部分，第二个元素是分隔符本身，第三个元素是分隔符之后的部分。

  ~~~
  >>> str1 = "Hello-World"
  >>> parts = str1.rpartition("-")
  >>> parts                       
  ('Hello', '-', 'World')
  ~~~

* replace()

  `replace(old, new, count)`方法用于将字符串中的旧子字符串替换为新的子字符串。

  - 方法作用：将字符串中的旧子字符串替换为新的子字符串。
  - 方法参数：`old`为要替换的旧子字符串，`new`为新的子字符串，`count`指定替换的次数，默认为全部替换。
  - 方法返回值：返回替换后的新字符串。

  ~~~
  >>> str1 = "Hello World"
  >>> new_str = str1.replace("World", "Universe")
  >>> new_str
  'Hello Universe'
  ~~~

* expandtabs()

  `expandtabs(tabsize)`方法用于将字符串中的制表符 `\t`转换为空格，根据指定的制表位数进行对齐。

  - 方法作用：将字符串中的制表符 `\t`转换为空格，根据指定的制表位数进行对齐。
  - 方法参数：`tabsize`为制表位数，默认为 `8`。
  - 方法返回值：返回转换后的新字符串。

  ~~~
  >>> str1 = "Hello\tWorld"
  >>> new_str = str1.expandtabs(4)
  >>> new_str                      
  'Hello   World'
  ~~~

* partition()

  `partition(separator)`方法用于将字符串从第一个匹配的分隔符处分割为三部分。

  - 方法作用：将字符串从第一个匹配的分隔符处分割为三部分。
  - 方法参数：`separator`为分隔符。
  - 方法返回值：返回一个包含三个元素的元组，第一个元素是分隔符之前的部分，第二个元素是分隔符本身，第三个元素是分隔符之后的部分。

  ~~~
  >>> str1 = "Hello-World"
  >>> parts = str1.partition("-")
  >>> parts                      
  ('Hello', '-', 'World')
  ~~~

###  1.4.2 字符串去除字符

* strip()方法

  `strip(chars)`方法用于去除字符串两侧指定字符，默认为去除空格。

  - 方法作用：去除字符串两侧指定字符。
  - 方法参数：`chars`为要去除的字符，默认为去除空格。
  - 方法返回值：返回去除指定字符后的新字符串。

  ~~~
  >>> str1 = "   Hello   "
  >>> new_str = str1.strip()
  >>> new_str                
  'Hello'
  ~~~

* lstrip()

  `lstrip(chars)`方法用于去除字符串左侧指定字符，默认为去除空格。

  - 方法作用：去除字符串左侧指定字符。
  - 方法参数：`chars`为要去除的字符，默认为去除空格。
  - 方法返回值：返回去除指定字符后的新字符串。

  ~~~
  >>> str1 = "   Hello   "
  >>> new_str = str1.lstrip()
  >>> new_str                  
  'Hello   '
  ~~~

* rstrip()

  `rstrip(chars)`方法用于去除字符串右侧指定字符，默认为去除空格。

  - 方法作用：去除字符串右侧指定字符。
  - 方法参数：`chars`为要去除的字符，默认为去除空格。
  - 方法返回值：返回去除指定字符后的新字符串

  ~~~
  >>> str1 = "   Hello   "
  >>> new_str = str1.rstrip()
  >>> new_str
  '   Hello'
  ~~~

###  1.4.3 字符串索引 填充对齐

* index()

  `index(substring, start, end)`方法用于查找字符串中子字符串的第一个匹配项的索引。

  - 方法作用：查找字符串中子字符串的第一个匹配项的索引。
  - 方法参数：`substring`为要搜索的子字符串，`start`和 `end`指定要搜索的字符串范围，默认为整个字符串。
  - 方法返回值：如果找到子字符串，则返回第一个匹配项的索引，否则引发 `ValueError`异常。

  ~~~
  >>> str1 = "Hello World"
  >>> index = str1.index("Wor")
  >>> index
  6
  ~~~

* rjust()

  `rjust(width, fillchar)`方法用于将字符串右对齐，并使用指定的字符进行填充。

  - 方法作用：将字符串右对齐，并使用指定的字符进行填充。
  - 方法参数：`width`为最终字符串的总宽度，`fillchar`为填充字符，默认为空格。
  - 方法返回值：返回右对齐填充后的新字符串。

  ~~~
  >>> str1 = "Hello"
  >>> new_str = str1.rjust(10, "*")
  >>> new_str                      
  '*****Hello'
  ~~~

* rindex()

  `rindex(substring, start, end)`方法用于查找字符串中子字符串的最后一个匹配项的索引。

  - 方法作用：查找字符串中子字符串的最后一个匹配项的索引。
  - 方法参数：`substring`为要搜索的子字符串，`start`和 `end`指定要搜索的字符串范围，默认为整个字符串。
  - 方法返回值：如果找到子字符串，则返回最后一个匹配项的索引，否则引发 `ValueError`异常。

  ~~~
  >>> str1 = "Hello World"
  >>> index = str1.rindex("o")
  >>> index
  7
  ~~~

* find()

  `find(substring, start, end)`方法用于查找字符串中子字符串的第一个匹配项的索引。

  - 方法作用：查找字符串中子字符串的第一个匹配项的索引。
  - 方法参数：`substring`为要搜索的子字符串，`start`和 `end`指定要搜索的字符串范围，默认为整个字符串。
  - 方法返回值：如果找到子字符串，则返回第一个匹配项的索引，否则返回 `-1`。

  ~~~
  >>> str1 = "Hello World"
  >>> index = str1.find("o")
  >>> index                  
  4
  ~~~

* rfind()

  `rfind(substring, start, end)`方法用于查找字符串中子字符串的最后一个匹配项的索引。

  - 方法作用：查找字符串中子字符串的最后一个匹配项的索引。
  - 方法参数：`substring`为要搜索的子字符串，`start`和 `end`指定要搜索的字符串范围，默认为整个字符串。
  - 方法返回值：如果找到子字符串，则返回最后一个匹配项的索引，否则返回 `-1`。

  ~~~
  >>> str1 = "Hello World"
  >>> index = str1.rfind("o")
  >>> index                   
  7
  ~~~

* center()方法

  - 用于将字符串居中，并使用指定的字符进行填充

  - 方法参数：`width`为最终字符串的总宽度，`fillchar`为填充字符，默认为空格。
  - 方法返回值：返回居中填充后的新字符串。

  ~~~
  >>> str1 = "Hello"
  >>> new_str = str1.center(10, "*")
  >>> new_str
  '**Hello***'
  ~~~

* zfill()

  `zfill()`方法用于在字符串左边填充0使其长度变为width，z代表zero 

  * 方法作用：在左边填充0使其长度变为width
  * 方法参数：width 为填充后字符串的总长度。

   - 方法返回值：返回填充后的新字符串。

  ~~~
  >>> str1 = "12".zfill(5) 
  >>> str1
  '00012'
  ~~~

* ljust

  `ljust(width, fillchar)`方法用于将字符串左对齐，并使用指定的字符进行填充

  * 方法作用：将字符串左对齐，并使用指定的字符进行填充。
  * 方法参数：`width`为最终字符串的总宽度，`fillchar`为填充字符，默认为空格
  * 方法返回值：返回左对齐填充后的新字符串。

  ~~~
  >>> str1 = "Hello"
  >>> new_str = str1.ljust(10, "*")
  >>> new_str        
  'Hello*****'
  ~~~

###  1.4.4 字符串大小写操作

* lower()

  `lower()`方法用于将字符串转换为小写

  * 方法作用：将字符串转换为小写
  * 方法参数：无
  * 方法返回值：返回转换为小写后的新字符串

  ~~~
  >>> str1 = "Hello World"
  >>> new_str = str1.lower()
  >>> new_str                
  'hello world'
  ~~~

* upper()

  `upper()`方法用于将字符串转换为大写。

  - 方法作用：将字符串转换为大写。
  - 方法参数：无。
  - 方法返回值：返回转换为大写后的新字符串。

  ~~~
  >>> str1 = "Hello World"
  >>> new_str = str1.upper()
  >>> new_str                
  'HELLO WORLD'
  ~~~

* capitalize()

  * 用于将字符串的首字母大写。

  - 方法参数：无。

  - 方法返回值：返回首字母大写后的新字符串。

    ~~~
    >>> str1 = "hello world"
    >>> new_str = str1.capitalize()
    >>>
    >>> new_str                          
    'Hello world'
    ~~~

* casefold()

  `casefold()`方法用于将字符串转换为小写，并进行 Unicode 兼容的字符串比较。

  英语中upper case = 大写， lower case = 小写，用case表示大小写情况，fold有折叠的意思，所以casefold就表示把大小写统统折叠为小写。

  - 方法作用：将字符串转换为小写，并进行 Unicode 兼容的字符串比较。
  - 方法参数：无。
  - 方法返回值：返回转换为小写后的新字符串。

  ~~~
  >>> str1 = "Hello World"
  >>> new_str = str1.casefold()
  >>> new_str                   
  'hello world'
  ~~~

* swapcase()方法

  `swapcase()`方法用于将字符串中的大小写进行转换。

  - 方法作用：将字符串中的大小写进行转换。
  - 方法参数：无。
  - 方法返回值：返回大小写转换后的新字符串。

  ~~~
  >>> str1 = "Hello World"
  >>> new_str = str1.swapcase()
  >>> new_str                   
  'hELLO wORLD'
  ~~~

* title()方法

  `title()`方法用于将字符串转换为标题形式，即每个单词的首字母大写，其他字母小写。

  - 方法作用：将字符串转换为标题形式。
  - 方法参数：无。
  - 方法返回值：返回转换为标题形式后的新字符串。

  ~~~
  >>> str1 = "hello world"
  >>> new_str = str1.title()
  >>> new_str                
  'Hello World'
  ~~~

### 1.4.5 字符串计数及编码

* count()

  `count(substring, start, end)`方法用于计算字符串中子字符串的出现次数。

  - 方法作用：计算字符串中子字符串的出现次数。
  - 方法参数：`substring`为要搜索的子字符串，`start`和 `end`指定要搜索的字符串范围，默认为整个字符串。
  - 方法返回值：返回子字符串的出现次数。

  ~~~
  >>> str1 = "abracadabra"
  >>> count = str1.count("a")
  >>> count
  5
  ~~~

* encode()

  `encode(encoding, errors)`方法用于将字符串编码为指定的编码格式。

  - 方法作用：将字符串编码为指定的编码格式。
  - 方法参数：`encoding`为编码格式，如 `"utf-8"`，`errors`指定错误处理方式，默认为 `"strict"`。
  - 方法返回值：返回编码后的字节对象。

  ~~~
  >>> str1 = "你好"
  >>> encoded_str = str1.encode("utf-8")
  >>> encoded_str                       
  b'\xe4\xbd\xa0\xe5\xa5\xbd'
  ~~~

###  1.4.6 字符串判断 

* startswith()

  `startswith(prefix, start, end)`方法用于检查字符串是否以指定的前缀开头

  * 方法作用：检查字符串是否以指定的前缀开头
  * 方法参数：`prefix`为要检查的前缀，`start`和 `end`指定要检查的字符串范围，默认为整个字符串
  * 方法返回值：如果字符串以指定的前缀开头，则返回 `True`，否则返回 `False`

  ~~~
  >>> str1 = "Hello World"
  >>> is_startswith = str1.startswith("Hello")
  >>> is_startswith                           
  True
  ~~~

* endswith()

  `endswith(suffix, start, end)`方法用于检查字符串是否以指定的后缀结尾

  * 方法参数：`suffix`为要检查的后缀，`start`和 `end`指定要检查的字符串范围，默认为整个字符串
  * 方法返回值：如果字符串以指定的后缀结尾，则返回 `True`，否则返回 `False`

  ~~~
  >>> str1 = "Hello World"
  >>> ends_with = str1.endswith("World")
  >>> ends_with                         
  True
  ~~~

* isalnum()

  `isalnum()`方法用于检查字符串是否只包含字母和数字字符，名字是is、alpha、number 三个单词合并而来。

  * 方法作用：检查字符串是否只包含字母和数字字符
  * 方法参数：无
  * 方法返回值：如果字符串只包含字母和数字字符，则返回 `True`，否则返回 `False`

  ~~~
  >>> str1 = "Hello123"
  >>> is_alnum = str1.isalnum()
  >>> is_alnum                 
  True
  ~~~

* isalpha()

  `isalpha()`方法用于检查字符串是否只包含字母字符。

  - 方法作用：检查字符串是否只包含字母字符。
  - 方法参数：无。
  - 方法返回值：如果字符串只包含字母字符，则返回 `True`，否则返回 `False`。

  ~~~
  >>> str1 = "Hello"
  >>> is_alpha = str1.isalpha()
  >>> is_alpha                 
  True
  ~~~

* isdigit()

  `isdigit()`方法用于检查字符串是否只包含数字字符。

  - 方法作用：检查字符串是否只包含数字字符。
  - 方法参数：无。
  - 方法返回值：如果字符串只包含数字字符，则返回 `True`，否则返回 `False`。

  ~~~
  >>> str1 = "12345"
  >>> is_digit = str1.isdigit()
  >>> is_digit                 
  True
  ~~~

* isnumeric()

  `isnumeric()`方法用于检查字符串是否只包含数字字符。

  - 方法作用：检查字符串是否只包含数字字符。
  - 方法参数：无。
  - 方法返回值：如果字符串只包含数字字符，则返回 `True`，否则返回 `False`。

  ~~~
  >>> str1 = "12345"
  >>> is_numeric = str1.isnumeric()
  >>> is_numeric                   
  True
  ~~~

*  islower()

   `islower()`方法用于检查字符串是否只包含小写字母

   * 方法作用：检查字符串是否只包含小写字母
   * 方法参数：无
   * 方法返回值：如果字符串只包含小写字母，则返回 `True`，否则返回 `False`

   ~~~
   >>> str1 = "hello"
   >>> is_lower = str1.islower()
   >>> is_lower                 
   True
   ~~~

* isupper()方法

  `isupper()`方法用于检查字符串是否只包含大写字母。

  - 方法作用：检查字符串是否只包含大写字母。
  - 方法参数：无。
  - 方法返回值：如果字符串只包含大写字母，则返回 `True`，否则返回 `False`。

  ~~~
  >>> str1 = "HELLO"
  >>> is_upper = str1.isupper()
  >>> is_upper                 
  True
  ~~~

* istitle()

  `istitle()`方法用于检查字符串是否符合标题化规则，即每个单词的首字母大写，其他字母小写。

  - 方法作用：检查字符串是否符合标题化规则。
  - 方法参数：无。
  - 方法返回值：如果字符串符合标题化规则，则返回 `True`，否则返回 `False`。

  ~~~
  >>> str1 = "Hello World"
  >>> is_title = str1.istitle()
  >>> is_title                 
  True
  ~~~

* isspace()

  `isspace()`方法用于检查字符串是否只包含空白字符。

  - 方法作用：检查字符串是否只包含空白字符。
  - 方法参数：无。
  - 方法返回值：如果字符串只包含空白字符，则返回 `True`，否则返回 `False`。

  ~~~
  >>> str1 = "   "
  >>> is_space = str1.isspace()
  >>> is_space                 
  True
  ~~~

*  isprintable()

   isprintable()`方法用于检查字符串是否只包含可打印字符

   * 方法作用：检查字符串是否只包含可打印字符
   * 方法参数：无
   * 方法返回值：如果字符串只包含可打印字符，则返回 `True`，否则返回 `False`

   ~~~
   >>> str1 = "Hello\nWorld"
   >>> is_printable = str1.isprintable()
   >>> is_printable                     
   False
   ~~~

* isidentifier()

  `isidentifier()`方法用于检查字符串是否是一个合法的Python标识符。

  - 方法作用：检查字符串是否是一个合法的Python标识符。
  - 方法参数：无。
  - 方法返回值：如果字符串是一个合法的Python标识符，则返回 `True`，否则返回 `False`。

  ~~~
  >>> str1 = "hello"
  >>> is_identifier = str1.isidentifier()
  >>>
  >>> is_identifier                      
  True
  ~~~

* isdecimal()

  `isdecimal()`方法用于检查字符串是否只包含十进制字符。

  - 方法作用：检查字符串是否只包含十进制字符。
  - 方法参数：无。
  - 方法返回值：如果字符串只包含十进制字符，则返回 `True`，否则返回 `False`。

  ~~~
  >>> is_decimal = str1.isdecimal()
  >>> is_decimal                   
  True
  ~~~

##  1.5 字符串与字节串

* 字符串

  字符串或串(String)是由数字、字母、下划线组成的一串字符。Python 里面最常见的类型。 可以简单地通过在引号间(单引号,双引号和三引号)包含字符的方式创建它。

  可通过.encode()方法编码为字节串

* 字节串

  二进制字符串，给计算机看的，对应Python中的字节串 bytes 类型。默认utf-8，可通过.decode()解码为字串。要创建一个字节串字面量，可以在字符串前面加一个 b 作为前缀。

* str -> bytes

  ~~~
  >>> str1 = 'hello' 
  >>> type(str1) 
  <class 'str'>
  >>>
  >>> byte = str1.encode()
  >>> byte
  b'hello'
  >>> type(byte) 
  <class 'bytes'>
  >>>
  ~~~

* bytes -> str

  ~~~
  >>> byte
  b'hello'
  >>> byte.decode()
  'hello'
  >>> type(byte.decode()) 
  <class 'str'>
  ~~~


#  2. 编程建议

##  2.1 不必预计算

* 以下写法性能一致

  预计算

  ~~~
  if x_time > 1036800:
      pass
  ~~~

  直接写成

  ~~~
  if x_time > 12*24*60*60：
      pass
  ~~~

##  2.2 超长字符串

* 斜杠连接

  ~~~
  str1 = "sssssssssssaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" \
         "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" \
         "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" \
         "aaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
  ~~~

* 括号连接

  ~~~
  str2 = ("sssssssssssaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
          "ddddddddddddddddddddddd"
          "ddddddddddddddddd"
          "sddddddddddddddds")
  ~~~

##  2.3 拼接字符串

- 性能基本差别不大

  Python2.2前的版本，字符串拼接操作很慢，字符串是不可变对象，每拼接异常字符串都会生成一个新的对象，触发新的内存分配，效率低。

  如今做了性能优化。大大提升了执行效率。

  += 与 "".join(str_list) 效率基本一样。

