# 循环

## For Loop:

```python
sumdata = 0
for i in range(1,101):
    # i从1开始，到101结束，实际只计算到100
    sumdata = sumdata+i
    #break
else:
    print(sumdata)
```

`for` 后面也可以带 `else`, 如果循环正常执行结束而没有意外中断，则执行一次`else`中的语句。

`Break` 属于中断循环。`continue`不是中断循环。

### for Loop 的普通方式

```python
beforesal = [10000, 15000, 8000, 4000, 5000]
aftersal = []
for i in beforesal:
    aftersal.append(i * 0.9)
print(aftersal)
```

### for loop 的简洁方式

```python
beforesal = [10000, 15000, 8000, 4000, 5000]
aftersal = [i * 0.9 for i in beforesal]  # 列表生成器
print(aftersal)
```

### `for loop`搭配 `if statement`

```python
#写一个列表，列表里的元素是100以内的所有奇数，不是奇数的位置写'偶数'
b=[i for i in range (1,100) if i%2==1] 
#for循环后面的if是筛选用的，不可以跟else语句
print(b)
```

### `for loop`搭配 `if, else`

```python
b=[i if i%2==1 else '偶数' for i in range (1,10)] 
#如果需要写else语句，应该将if写到for的前面
print(b)
```

 



 

 

