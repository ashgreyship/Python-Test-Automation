# 装饰器

## 一种写法

```python
import time


def foo():
    print("执行测试用例")


# 这就是装饰器，本质上是一个函数，用来处理其他的函数。
# 让其他函数在不需要修改代码的前提下，增加额外的功能
def show_time(func):
    def inner():
        begin_time = time.time()
        func()
        end_time = time.time()
        print('总计执行时间 = ', end_time - begin_time)

    return inner


foo = show_time(foo)  # 相当于 foo = inner
foo()

```

## 另外一种写法\(语法糖\)

```python
import time


# 这就是装饰器，本质上是一个函数，用来处理其他的函数。
# 让其他函数在不需要修改代码的前提下，增加额外的功能
def show_time(func):
    def inner():
        begin_time = time.time()
        func()
        end_time = time.time()
        print('总计执行时间 = ', end_time - begin_time)

    return inner


@show_time
def foo():
    print("执行测试用例")


foo()
```

## 被装饰的函数带参数

```python
def bar(func):
    """
    函数接受一个参数
    参数里边定义一个内部函数
    将定义的内部函数作为返回值返回
    :param func: 接受的参数就是被装饰的函数，也就是需要增加功能的函数
    :return: 装饰完后的函数
    """

    def inner(name):
        func(name)  # 需要保留被装饰的函数的原有功能，所以通过传参的形式调用被装饰的函数
        print('这是新加的功能')

    return inner


@bar
def foo(name):
    print("执行测试用例:", name)


foo("张三")
```

## 被装饰的函数不定参数

```python
# 对于不定长参数的函数的处理
def funcPro(funcName):
    print("---不定长参数的传递---")

    def func_in(*args, **kwargs):  # 前面传递的是元组，后面是字典
        print("----args,kwargs---")
        funcName(*args, **kwargs)
        print("-----end args ,kwargs----")

    print("---不定长参数传递结束--")
    return func_in


@funcPro
def testThree(a, b, c):
    print("----test  a=%d,b=%d,c=%d----" % (a, b, c))


testThree(1, 2, 3)  # 这里参数传递几个值，都可以，跟test对应起来就可以
'''
----funPro 1---
----funcPro2-----
---不定长参数的传递---
---不定长参数传递结束--
----args,kwargs---
----test  a=1,b=2,c=3----
-----end args ,kwargs----
'''
```

## 被装饰的函数带返回值

```python
def funcReturn(functionName):
    print("---有返回值的函数---")

    def func_in():
        print("--先进行装饰，将有返回值的函数作为参数传递进来")
        res = functionName()
        print("--调用有返回值的函数后--")
        return res

    print("---有返回值的函数装饰结束---")


@funcReturn
def testRe():
    print("---这个函数有返回值")
    return "测试有返回结果"


ret = testRe()
print("the end value is %s" % ret)
```

##  装饰器带参数

```python
def wraper(age):
    def bar(func):
        """
        函数接受一个参数
        参数里边定义一个内部函数
        将定义的内部函数作为返回值返回
        :param age:
        :param func: 接受的参数就是被装饰的函数，也就是需要增加功能的函数
        :return: 装饰完后的函数
        """
        print("新功能：")

        def inner(name):
            func(name)  # 需要保留被装饰的函数的原有功能，所以通过传参的形式调用被装饰的函数
            print('年龄:', age)

        return inner

    return bar


# 第一层 foo = bar
# 第二层 foo = inner
@wraper(18)
def foo(name):
    print("执行测试用例:", name)


foo("张三")
```



 

