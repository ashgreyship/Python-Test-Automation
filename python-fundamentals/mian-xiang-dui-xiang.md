# 面向对象

## 例子

```python
class Rectangle:
    def __init__(self, chang, kuan):
        self.chang = chang
        self.kuan = kuan

    def length(self):
        return (self.chang + self.kuan) * 2
        
cfx = Rectangle(6,4)
print(cfx.length())
print(cfx.dict)
```

## 类方法&静态方法

```python
class Rectangle:
    def __init__(self, chang, kuan):
        self.chang = chang
        self.kuan = kuan

    def length(self):
        return (self.chang + self.kuan) * 2

    # 类方法,可以直接由类本身调用，也可以实例调用
    @classmethod
    def feature(cls):
        print('两边的长相等，两边的宽也相等')

    # 静态方法，可以由类调用，也可以由实例调用，它本身确实是一个函数，和类没有关系，只是放在类下面
    @staticmethod
    def feature2(a, b):
        print(a + b)


Rectangle.feature()

cfx = Rectangle(9, 6)
cfx.feature()

Rectangle.feature2(3, 5)

#加载inspect模块，这是一个Python的自检模块，可以判断是否是某一种对象
import inspect
#此处不要加（),如果加括号 feature（），则判断的是返回值
print(inspect.ismethod(Rectangle.feature))
print(inspect.ismethod(Rectangle.feature2))
print(inspect.isfunction(Rectangle.feature2))
```

 类方法是方法 \(`method`\) 静态方法是函数\(`function`\)

`self`，一般指的是类的实例。  
`cls`，一般指的是类。

```python
class A(object):
    def foo1(self): # 普通的方法，第一个参数需要是self，它表示一个具体的实例本身。
        print('Hellow', self)

    @staticmethod
    def foo2():  # 如果用了staticmethod，那么就可以无视这个self，而将这个方法当成一个普通的函数使用。
        print('hello')

    @classmethod
    def foo3(cls): # 而对于classmethod，它的第一个参数不是self，是cls，它表示这个类本身。
        print('hellow', cls)


if __name__ == "__main__":
    a = A()
    a.foo1()   # Hellow <__main__.A object at 0x102a08908> （表明是类实例）
    A.foo2(a)  # hello  （没有用）
    A.foo3()   # hellow <class '__main__.A'> (表明是类)
```

## 继承

```python
# 继承
class Square(Rectangle):
    # 覆盖重写父类的初始化方法（不适用父类的此方法，完全重写）
    def __init__(self, bianchang):
        self.chang = bianchang
        self.kuan = bianchang

    # 扩展父类的方法
    @classmethod
    def feature(cls):
        super().feature()  # 先把父类的方法继承过来
        # Rectangle.feature()
        print('长和宽相等')


square = Square(4)
print(square.length())
print(square.area())
square.feature()
```

## 私有方法

```python
class Rectangle:
    def __init__(self, chang, kuan):
        self.chang = chang
        self.kuan = kuan

    def length(self):
        return (self.chang + self.kuan) * 2

    def area(self):
        return self.chang * self.kuan

    # 类方法,可以直接由类本身调用，也可以实例调用
    @classmethod
    def feature(cls):
        print('两边的长相等，两边的宽也相等')

    # 静态方法，可以由类调用，也可以由实例调用，它本身确实是一个函数，和类没有关系，只是放在类下面
    @staticmethod
    def feature2(a, b):
        print(a + b)

    def __privatemethod(self):
        print('此为私有方法')
```

 在方法名字前面加上`__`，此方法就不可继承，变量也同理。

 所有类本质上都是object的子类

```python
class Class1:
    '''
    显示注释
    '''
    pass


class Class2(object):
    pass


cls1 = Class1()
cls2 = Class2()
print(dir(cls1))
print(dir(cls2))
# 显示注释
print(Class1.__doc__)
# 显示模块
print(Class1.__module__)
print(Class1.__base__)
```

## 多继承

```python
# 多继承
class Class1:
    def money(self):
        print('有钱100万')


class Class2:
    def money(self):
        print('有钱200万')


class Class3(Class1, Class2):
    pass


cls3 = Class3()
cls3.money()
# print 有钱一百万
```

 当遇到多个父类当中，有同名的方法时，从上向下寻找，使用最先找到的父类方法。

## 多态

```python

class Fanguan:
    pass


class Yunxiangrousi(Fanguan):
    def menu(self):
        print('鱼香肉丝')


class Gongbaojiding(Fanguan):
    def menu(self):
        print('公保鸡丁')


def fuwuyuan(obj):
    obj.menu()


guest1 = Yunxiangrousi()
guest2 = Gongbaojiding()
guest1.menu()
guest2.menu()
```

 

