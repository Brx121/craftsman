[TOC]

#  1. 基础知识

* 简介：最常用的函数定义方式是使用 def 语句 

  ~~~
  # 定义函数
  def add(x, y):
      return x + y
  # 调用函数
  add(3, 4)
  ~~~

* lamda

  ~~~
  # 效果与 add 函数一样
  add = lambda x, y: x + y
  ~~~

* 函数在 Python 中是一等对象，这意味着我们可以把函数自身作为函数参数来使用。最常用的内置排序函数 sorted() 就利用了这个特性 

  ~~~
  >>> l = [13, 16, 21, 3]
  # key 参数接收匿名函数作为参数
  >>> sorted(l, key=lambda i: i % 3)
  [21, 3, 13, 16]
  ~~~

##  1.1 函数参数的常用技巧 

* 参数

  它是函数最主要的输入源，决定了调用方使用函数时的体验。 

* 别将可变类型作为参数默认值 

  在编写函数时，我们经常需要为参数设置默认值。这些默认值可以是任何类型，比如字符串、数值、列表，等等。而当它是可变类型时 ：

  ~~~
  >>> def append_value(value, items=[]):        
  ...   """向 items 列表中追加内容，并返回列表""" 
  ...   items.append(value)
  ...   return items
  ...
  >>> append_value('foo')
  ['foo']
  >>> append_value('brx')
  ['foo', 'brx']
  ~~~

  多次调用它以后，就会发现函数的行为和预想的不太一样。

  在第二次调用时，函数并没有返回正确结果['brx']，而是返回了 ['foo', 'brx']，这意味着参数
  items 的值不再是函数定义的空列表 []，而是变成了第一次执行后的结果 ['brx']  

  ~~~
  之所以出现这个问题，是因为 Python 函数的参数默认值只会在函数定义阶段被创建一次，之后不论再调用多少次，函数内拿到的默认值都是同一个对象。
  ~~~

* 定义特殊对象来区分是否提供了默认参数 

  当我们为函数参数设置了默认值，不强制要求调用方提供这些参数以后，会引入另一件麻烦事儿：无法严格区分调用方是不是真的提供了这个默认参数。 

  ~~~
  def dump_value(value, extra=None):
      if extra is None:
      # 无法区分是否提供 None 是不是主动传入
      ...
  # 两种调用方式
  dump_value(value)
  dump_value(value, extra=None)
  ~~~

  对于 dump_value() 函数来说，当调用方使用上面两种方式来调用它时，它其实无法分辨。因为在这两种情况下，函数内拿到的 extra 参数的值都是 None。 

* 解决

  定义一个特殊对象（标记变量）作为参数默认值： 

  ~~~
  # 定义标记变量
  # object 通常不会单独使用，但是拿来做这种标记变量刚刚好
  _not_set = object()
  def dump_value(value, extra=_not_set):
      if extra is _not_set:
          # 调用方没有传递 extra 参数
      ...
  ~~~

  相比 None，_not_set 是一个独一无二、无法随意获取的标记值。假如函数在执行时判断 extra 的值等于 _not_set，那我们基本可以认定：调用方没有提供 extra 参数。 

* 定义仅限关键字参数 

  * 位置参数

  * 关键字参数

  当你要调用参数较多（超过 3 个）的函数时，使用关键字参数模式可以大大提高代码的可读性。 

##  1.2 函数返回的常见模式 

* 尽量只返回一种类型 

* 谨慎返回 None 值 

  在编程语言的世界里，“空值”随处可见，它通常用来表示某个应该存在但是缺失的东西。“空值”在不同编程语言里有不同的名字，比如 Go 把它叫作 nil，Java 把它叫作 null，Python 则称它为 None。 数据库为Null

  * 操作类函数的默认返回值 

    当某个操作类函数不需要任何返回值时，通常会返回None。 

    ~~~
    def close_ignore_errors(fp):
        # 操作类函数，默认返回 None
        try:
            fp.close()
        except IOError:
            logger.warning('error closing file')
    ~~~

  * 意料之中的缺失值 

    还有一类函数，它们所做的事情天生就是在尝试，比如从数据库里查找一个用户、在目录中查找一个文件。视条件不同，函数执行后可能有结果，也可能没有结果。而重点在于，对于函数的调用方来说，“没有结果”是意料之中的事情。
    针对这类函数，使用 None 作为“没有结果”时的返回值通常也是合理的。
    在标准库中，正则表达式模块 re 下的 re.search()、re.match() 函数均属于此类。 

  * 在执行失败时代表“错误” 

    有时候，None 也会用作执行失败时的默认返回值 

    ~~~
    def create_user_from_name(username):
        """通过用户名创建一个 User 实例"""
        if validate_username(username):
            return User.from_username(username)
        else:
            return None
    ~~~

    当 username 通过校验时，函数会返回正常的用户对象，否则返回 None。 

* 早返回，多返回 

  在编写函数时，请不要纠结函数是不是应该只有一个return，只要尽早返回结果可以提升代码可读性，那就多多返回 

##  1.3 常用函数模块：functools 

* 与函数关系紧密的模块 

  functools 模块提供的高阶函数 

* functools.partial()

  减少调用函数时的参数个数。允许给一个或多个参数设置固定的值，减少被调用是的参数个数。

  ~~~
  def test(a, b, c, d):
      print(a, b, c, d)
  
  from functools import partial
  s1 = partial(test, 1)
  s1(2, 3, 4)
  
  s2 = partial(test, d=4)
  s2(1, 2, 3)
  
  s3 = partial(test, 1, 2, d=4)
  s3(3)
  ~~~

  1 2 3 4
  1 2 3 4
  1 2 3 4

* functools.lru_cache() 

  可以方便地给函数加上缓存功能，同时不用修改任何函数内部代码 

#  2. 编程建议

* 别写太复杂的函数
* 优先使用列表推导式 
* 尽量不要lambda 