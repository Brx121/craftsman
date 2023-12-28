[TOC]

#  1. 基础知识

##  1.1 迭代器与可迭代对象 

* 可迭代对象

  字符串、生成器等等

* iter() 与 next() 内置函数

  bool可以获取某个对象的布尔值真假值

  ```
  >>> bool("brx")
  True
  ```

  iter函数类似于bool，调用Iter()会尝试返回一个迭代器对象

  ~~~
  >>> iter([1, 2, 3])
  <list_iterator object at 0x00000297902EFD60>
  >>> iter("brx")
  <str_iterator object at 0x000002979029A4C0>
  ~~~

  不可迭代类型会抛异常

  ```
  >>> iter(1)
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: 'int' object is not iterable
  ```

  next()

  ```
  >>> iter_list = iter([1, 2, 3])
  >>> next(iter_list)
  1
  >>> next(iter_list)
  2
  >>> next(iter_list)
  3
  >>> next(iter_list)
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  StopIteration
  ```

* 循环与迭代

  ```
  for i in [1, 2, 3, 4, 5]:
      print(i)
  ```

  可翻译成：

  ```
  iterator = iter([1, 2, 3, 4, 5])
  
  while True:
      try:
          i = next(iterator)
          print(i)
      except StopIteration:
          break
  ```

* 迭代器与可迭代对象区别
  - 可迭代对象不一定是迭代器，迭代器一定是可迭代对象。
  - 可迭代对象使用iter()会返回迭代器，迭代器则会返回其自身。
  - 每个迭代器的迭代过程是一次性的，可迭代对象则不一定。
  - 可迭代对象只需要实现inter方法，迭代器需要额外实现next方法

## 1.2 实现一个和range()类似功能的迭代器对象range7()

- 可以被7整除或者包含7

  ```
  class Range7():
      def __init__(self, start, end):
          self.start = start
          self.end = end
  
      def __iter__(self):
          # 返回一个新的迭代器对象
          return Range7Iterator(self)
  
  class Range7Iterator():
      def __init__(self, range_obj):
          self.range_obj = range_obj
          self.current = range_obj.start
  
      def __iter__(self):
          return self
  
      def __next__(self):
          while True:
              if self.current >= self.range_obj.end:
                  raise StopIteration
  
              if self.num_is_valid(self.current):
                  ret = self.current
                  self.current += 1
                  return ret
              self.current += 1
  
      def num_is_valid(self, num):
          if num == 0:
              return False
          return num % 7 == 0 or "7" in str(num)
  
  r = Range7(0, 20)
  print(tuple(r))  # (7, 14, 17)
  ```

## 1.3 生成器

- 生成器是迭代器

  关键字：yield

- 修饰可迭代对象优化循环： 遍历一个列表的同时，获取当前索引位置 

  ~~~
  index = 0
  for name in names:
      print(index, name)
      index += 1
  ~~~

  或者

  ~~~
  for i, name in enumerate(names):
      print(i, name)
  ~~~

- 使用生成器函数修饰可迭代对象 

  ~~~
  def sum_even_only(numbers):
      """对 numbers 里面所有的偶数求和"""
      result = 0
      for num in numbers:
          if num % 2 == 0:
              result += num
      return result
  ~~~

  生成器函数 even_only()，它专门负责偶数过滤工作 

  ~~~
  def even_only(numbers):
      for num in numbers:
          if num % 2 == 0:
              yield num
  ~~~

  求和

  ~~~
  def sum_even_only_v2(numbers):
      """对 numbers 里面所有的偶数求和"""
      result = 0
      for num in even_only(numbers):
          result += num
      return result
  ~~~

##  1.4 使用 itertools 模块优化循环 

* 优化嵌套循环 product()

  ```
  l1 = [1, 2, 3]
  l2 = [4, 5, 6]
  l3 = [7, 8, 9]
  
  def fun(l1, l2, l3):
      for i in l1:
          for j in l2:
              for k in l3:
                  if i + j + k == 12:
                      return i, j, k
  
  res = fun(l1, l2, l3)
  print(res)  # (1, 4, 7)
  
  from itertools import product
  
  def test(l1, l2, l3):
      for i, j, k in product(l1, l2, l3):
          if i + j + k == 12:
              return (i, j, k)
  res2 = test(l1, l2, l3)
  print(res2)  # (1, 4, 7)
  
  print(list(product([1, 2], [3, 4])))  # [(1, 3), (1, 4), (2, 3), (2, 4)]
  ```

## 1.5 for else

* for else 语句： for 循环中的语句和普通的没有区别，else 中的语句会在循环正常执行完（即 for 不是通过 break 跳出而中断的）的情况下执行。

  for 循环正常执行的情况下

  ~~~
  for i in [1, 2, 3]:
      print(i)
  
  else:
      print("*************")
  ~~~

  ~~~
  1
  2
  3
  *************
  ~~~

  for 循环中断的情况下

  ~~~
  for i in [1, 2, 3]:
      print(i)
      if i == 2:
          break
  
  else:
      print("*************")
  ~~~

  1
  2

#  2. 数字统计

* 计算某个文件包含了多少个数字字符

* demo

  ```
  def count_number() -> int:
      with open("test.txt", "r", encoding="utf-8") as fp:
          count = 0
          for line in fp:
              for i in line:
                  if i.isdigit():
                      count += 1
      return count
  
  res = count_number()
  print(res)  
  ```

* 缺点：

  若test.txt文件里面没有换行符，则一次性生成一个巨大字符串对象，会消耗大量时间和内存

* while循环+read() 方法优化

  每次读取4K大小数据量

  ~~~
  def count_number() -> int:
      count = 0
      block_size = 1024 * 4
      with open("test.txt", "r", encoding="utf-8") as fp:
          while True:
              chunk = fp.read(block_size)
              if not chunk:
                  break
              for i in chunk:
                  if i.isdigit():
                      count += 1
      return count
  
  res = count_number()
  print(res)  
  ~~~

#  3.6 获取字典第一个键

- demo

  ```
  di = {"aa": 1, "bb": 2, "cc": 3}
  
  print(list(di.keys())[0])
  
  print(next(iter(di.keys())))
  ```