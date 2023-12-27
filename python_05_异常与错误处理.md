[TOC]

#  1. 基础知识

##  1.1 优先使用异常捕获

* LBYL编程风格（三思而后行）

  就是在执行一个可能会出错的操作时，先做一些关键的条件判断，仅当条件满足时才进行操作。 

  ~~~
  # 接收一个整型参数，返回加1结果
  def fun(x) -> int:
      if isinstance(x, int):
          return x + 1
  
      elif isinstance(x, str) and x.isdigit():
          return int(x) + 1
  
      else:
          print("error")
  ~~~

  当 x 为整数时，执行 第一个 if 语句

  当 x 为整数字符串 时，执行第二个 elif 语句

  当 x 为其他无法转换为整数的参数时，执行 else 语句

* EAFP（获取原谅比许可简单）

  EAFP“获取原谅比许可简单”是一种和 LBYL“三思而后行”截然不同的编程风格 

  在 Python 世界里，EAFP 指不做任何事前检查，直接执行操作，但在外层用 try 来捕获可能发生的异常。

  ~~~
  def fun(x) -> int:
      try:
          return int(x) + 1
      except Exception as e:
          print(e)
  ~~~

##  1.2 try 语句常用知识 

* 实践 EAFP 编程风格时，需要大量用到异常处理语句：try/except 结构 

  ~~~
  def fun(x) -> int:
      try:
          res = int(x) + 1
          print("*****")
      except Exception as e:
          print(e)
      finally:
          print("11111111")
  
  fun("ad")
  ~~~

  ~~~
  invalid literal for int() with base 10: 'ad'
  11111111
  ~~~

  finally 里的语句，无论如何都会被执行，哪怕已经执行了 return 

* 把更精确的 except 语句放在前面 

  Python 的内置异常类之间存在许多继承关系 ，BaseException 是一切异常类的父类。

  issubclass() 方法用于判断参数 class 是否是类型参数 classinfo 的子类。

  语法

  ~~~
  issubclass(class, classinfo)
  ~~~

  ~~~
  >>> issubclass(Exception, BaseException)
  True
  >>> issubclass(LookupError, Exception)
  True
  >>> issubclass(KeyError, LookupError)
  True
  ~~~

  即

  ~~~
  一条异常类派生关系：BaseException → Exception → LookupError → KeyError
  ~~~

  如果一个 try 代码块里包含多条 except，异常匹配会按照从上而下的顺序进行。这时，假如你不小心把一个比较模糊的父类异常放在前面，就会导致在下面的 except 永远不会被触发。 

* 使用 else 分支 

  ~~~
  # 同步用户资料到外部系统，仅当同步成功时发送通知消息
  sync_succeeded = False
  try:
      sync_profile()
      sync_succeeded = True
  except Exception as e:
      print("Error while syncing user profile")
  if sync_succeeded:
      send_notification('profile sync succeeded')
  ~~~

  我期望只有当 sync_profile() 执行成功时，才继续调用 send_notification() 发送通知消息。为此，我定义了一个额外变量 sync_succeeded 来作为标记。 

  如果使用 try 语句块里的 else 分支，代码可以变得更简单 

  ~~~
  try:
      sync_profile()
  except Exception as e:
      print("Error while syncing user profile")
  else:
      send_notification('profile sync succeeded')
  ~~~

  上面的 else 和条件分支语句里的 else 虽然是同一个词，但含义不太一样。
  异常捕获语句里的 else 表示：仅当 try 语句块里没抛出任何异常时，才执行 else 分支下的内容，效果就像在 try 最后增加一个标记变量一样。 

  ~~~
  else 这个词，字面意义是“否则”，但当它紧随着 try和 except 出现时，你其实很难分辨它到底代表哪一种“否
  则”——到底是有异常时的“否则”，还是没异常时的“否则”。因此，有些开发者认为，异常捕获里的 else 关键字，应当调整为 then：表示“没有异常后，接着做某件事”的意思。
  ~~~

* 使用空 raise 语句 

  在处理异常时，有时我们可能仅仅想记录下某个异常，然后把它重新抛出，交由上层处理。 

  ~~~
  def fun(x) -> int:
      try:
         res = 12/x
      except Exception as e:
          raise
  
      return res
  
  fun(0)
  ~~~

  ~~~
  ZeroDivisionError: division by zero
  ~~~

  当一个空 raise 语句出现在 except 块里时，它会原封不动地重新抛出当前异常。 

##  1.3 抛出异常，而不是返回错误 

* Python 里的函数可以一次返回多个值（通过返回一个元组实现）。所以，当我们要表明函数执行出错时，可以让它同时返回结果与错误信息。 

  try-except 语句

  ~~~
  try:
      # 可能抛出异常的代码
  except SomeException:
      # 处理异常的代码
  ~~~

  try-except-else 语句

  ~~~
  try:
      # 可能抛出异常的代码
  except SomeException:
      # 处理异常的代码
  else:
      # 没有异常时执行的代码
  ~~~

  try-except-finally 语句

  ~~~
  try:
      # 可能抛出异常的代码
  except SomeException:
      # 处理异常的代码
  finally:
      # 总是会执行的代码
  ~~~

  多个异常类型的捕获

  ~~~
  try:
      # 可能会引发异常的代码
  except ExceptionType1:
      # 处理异常类型 ExceptionType1 的代码
  except ExceptionType2:
      # 处理异常类型 ExceptionType2 的代码
  ...
  except ExceptionTypeN:
      # 处理异常类型 ExceptionTypeN 的代码
  else:
      # 没有发生异常时执行的代码
  finally:
      # 无论是否发生异常，都会执行的代码
  ~~~

* 自定义异常

  ~~~
  class MyException(Exception):
      pass
   
  if some_condition:
      raise MyException("自定义异常信息")
  ~~~

* 在 Python 中，常见的异常类型有：

  TypeError（类型错误）：当一个操作或函数应用于不支持该操作或函数的对象类型时引发。

  ValueError（值错误）：当一个操作或函数应用于正确类型的对象，但对象的值不适合该操作或函数时引发。

  IndexError（索引错误）：当尝试访问一个不存在的索引或切片时引发。

  KeyError（键错误）：当尝试访问一个不存在的字典键时引发。

  NameError（名称错误）：当尝试访问一个未定义的变量或函数时引发。

  IOError（输入输出错误）：当一个输入输出操作（例如打开文件或读写文件）失败时引发。

  ZeroDivisionError（除零错误）：当试图除以零时引发。

##  1.4 使用上下文管理器 

* with

  ~~~
  # 使用 with 打开文件，文件描述符会在作用域结束后自动被释放
  
  with open('foo.txt') as fp:
      content = fp.read()
  ~~~

  with 是一个神奇的关键字，它可以在代码中开辟一段由它管理的上下文，并控制程序在进入和退出这段上下文时的行为。 此处就是：进入时打开某个文件并返回文件对象，退出时关闭该文件对象。 

#  2. 编程建议

* 不要随意忽略异常 

  * 在 except 语句里捕获并处理它，继续执行后面的代码；
  * 在 except 语句里捕获它，将错误通知给终端用户，中断执行；
  * 不捕获异常，让异常继续往堆栈上层走，最终可能导致程序崩溃。 

* 精确的使用try

  容易出错的步骤添加try，比如网络请求，本地文件操作。

* 不要随意忽略异常

  ```
  try:
      res = requests.get("xxx")
  except Exception as e:
      pass
  ```

  即使可以忽略异常，最好使用log进行记录。

* 抛出可区分的异常 