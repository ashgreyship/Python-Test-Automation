# 递归

```python
def multiply(n):
    if n == 1:
        return 1
    else:
        return n * multiply(n - 1)


value = multiply(101)
print(value)
```

 

```python
# 斐波那契数列
list1 = []
for i in range(20):
    if len(list1)< 2:
        list1.append(1)
    else:
        list1.append(list1[-1] + list1[-2])
print(list1)
```

 

