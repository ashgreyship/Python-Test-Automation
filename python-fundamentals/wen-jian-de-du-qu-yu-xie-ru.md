# 文件的读取与写入

#### `open()` 打开文件

w表示写入 r表示读取 a追加写入 必须填入的是文件的路径

#### `tell()`返回目前指针所在的位置

## 文件的读取

```python
file1=open('input.txt')  
#后面如果没有额外的参数，就表示模式是读取模式
#当r模式时，如果找不到文件，会报错
print('文件指针的位置：'+str(file1.tell())) 
#tell()返回目前指针所在的位置

#read() 方法
print(file1.read()) 
#读取整个文件
print(file1.read().splitlines()) #去掉换行符，返回值是列表类型


#seek() 方法
file1.seek(10) 
#第一个参数光标偏移几位，第二个参数可以不用写，缺省为0，表示从文件头开始计算
#如果seek()的第二个为1或2，
#1表示从指针的当前位置开始偏移，2表示从文件末尾开始偏移
#seek()的第二个参数如果是1和2，那么只有在rb模式才能用,rb是指以二进制方式读取文件

print(file1.readline()) 
#读取文件的一行

print(file1.readlines())  
#读取文件中的多行，返回值为列表形式
```

##  文件的写入

```python
file1=open('d:/note1.txt','r+')  
#w方法表示写入，并且会清除文件之前的内容 a也是表示写入，它会接着文件后面写
file1.write('Here is the input string')
file1.close()
```

## 同时读取和写入

```python
file1=open('aab.txt','w+')  #./表示当前目录下,或者直接写文件名，都代表当前目录
file1.write('asfsafsfsfsf')
file1.seek(0)
print(file1.read())
file1.close()
```

Open 和 With open:

Open: 如果出现异常，如读取过程中文件不存在或异常，则直接出现错误，close方法无法执行，文件无法关闭

 With open: 用with语句的好处，就是到达语句末尾时，会自动关闭文件，即便出现异常。

## 读取和写入图片

```python
with open('./pic.png', 'rb') as f:
    data = f.read()

with open('./pic2.png', 'wb') as f:
    f.write(data)
```

 

