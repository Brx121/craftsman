[TOC]

#  1. 基础知识

##  1.1 分支惯用写法

* if/elif/else

  ~~~
  # 标准条件分支语句
  if condition:
  ...
  elif another_condition:
  ...
  else:
  ...
  ~~~

* 不要显式地和布尔值做比较 

  ~~~
  # 不推荐的写法
  # if user.is_active_member() == True:
  # 推荐写法
  if user.is_active_member():
  ~~~

  绝大多数情况下，在分支判断语句里写 == True 都没有必要，删掉它代码会更短也更易读。 

* 省略零值判断 

  当你编写 if 分支时，如果需要判断某个类型的对象是否是零值，可能会把代码写成下面这样： 

  ~~~
  if containers_count == 0:
  ...
  if fruits_list != []:
  ...
  ~~~

  这种判断语句其实可以变得更简单，因为当某个对象作为主角出现在 if 分支里时，解释器会主动对它进行“真值测试”，也就是调用 bool() 函数获取它的布尔值。 

  ~~~
  # 数字 0 的布尔值为 False，其他值为 True
  >>> bool(0) 
  False
  >>> bool(123) 
  True
  
  # 空列表的布尔值为 False，其他值为 True
  >>> bool([])  
  False
  >>> bool([1, 2, 3]) 
  True
  ~~~

  * 布尔值为假

    None、0、False、[]、{}、()、set() frozenset等

  * 布尔值为真

    非0的数值、非空的序列、元组、字典，用户定义的类和实例等

* 把否定逻辑移入表达式内 

  在构造布尔逻辑表达式时，你可以用 not 关键字来表达“否定”含义 

  ~~~
  >>> res_list = [1, 2, 3]
  >>> 1 in res_list
  True
  >>> 9 not in res_list
  True
  ~~~

* 尽可能让三元表达式保持简单

  ~~~
  >>> x = 11                            
  >>> y = 12 if x > 7 else 9  
  >>> y
  12
  ~~~

##  1.2 与 None 比较时使用 is 运算符 

* == 对比，行为可被重载，is判断两个对象是否是内存里的同一个东西，无法被重载

  ~~~
  >>> x = 12
  >>> y = 12
  >>> x == y
  True
  >>> x is y
  True
  >>>
  >>> a = 257
  >>> b = 257
  >>> a is b
  False
  >>>
  ~~~

  ~~~
  (1) == 对比两个对象的值是否相等，行为可被 __eq__ 方法重载；
  (2) is 判断两个对象是否是内存里的同一个东西，无法被重载。 
  
  换句话说，当你在执行 x is y 时，其实就是在判断 id(x) 和id(y) 的结果是否相等，二者是否是同一个对象。
  ~~~

  因此，当你想要判断某个对象是否为 None 时，应该使用 is 运算符 

  ~~~
  对于从 -5 到 256 的这些常用小整数，Python 会将它们缓存在内存里的一个数组中。当你的程序需要用到这些数字时，Python不会创建任何新的整型对象，而是会返回缓存中的对象。
  ~~~

#  2. bisect 优化分支

* demo

  ~~~
  def fun(x) -> str:
      if x >= 8.5:
          return "S"
  
      if x >= 8:
          return "A"
  
      if x >= 7:
          return "B"
  
      if x >= 6:
          return "C"
  
      else:
          return "D"
  
  res = fun(4)
  print(res)  # D
  ~~~

* 收集分界点

  有序列表做二分查找 二分查找的容器必须是已经排好序的.

  ```
  import bisect
  
  def fun(x):
      breakpoints = [6, 7, 8, 8.5]
      grades = ["D", "C", "B", "A", "S"]
      index = bisect.bisect(breakpoints, x)
      res = grades[index]
      return res
  
  res = fun(8)
  print(res)  # A
  ```

#  3. 编程建议

* 尽量避免多层分支嵌套 

* 别写太复杂的条件表达式 

* 尽量降低分支内代码的相似性 

* 使用“德摩根定律” 

  ~~~
  not A or not B 等价于 not (A and B)
  ~~~

* 使用 all()/any() 函数构建条件表达式 

  在 Python 的众多内置函数中，有两个特别适合在构建条件表达式时使用，它们就是 all() 和 any()。这两个函数接收一个可迭代对象作为参数，返回一个布尔值结果。

  ~~~
  all(iterable)：仅当 iterable 中所有成员的布尔值都为真时返回 True，否则返回 False。
  
  any(iterable)：只要 iterable 中任何一个成员的布尔值为真就返回 True，否则返回 False。
  ~~~

* 留意 and 和 or 的运算优先级 

  ~~~
  >>> (True or False) and False
  False
  >>> True or False and False
  True
  ~~~

  出现这个结果的原因是：and 运算符的优先级高于 or。因此在Python 看来，上面第二个表达式实际上等同于 True or (False and False)，所以最终结果是 True 而不是 False。 

* 避开 or 运算符的陷阱 

  or 运算符是构建逻辑表达式时的常客。or 最有趣的地方是它的“短路求值”特性。比如在下面的例子里，1 / 0 永远不会被执行，也就意味着不会抛出 ZeroDivisionError 异常

  ~~~
  >>> True or (1 / 0)
  True
  ~~~

* 所有的 0、空列表、空字符串等，都是布尔假值 

