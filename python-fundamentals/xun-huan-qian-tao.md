# 排序算法

### Python 自带的排序算法：

```python
# Python可以自带排序方法
list1 = [3, 9, 6, 28, 0, -1, 16]
list1.sort()  # sort() 列表自带的方法,这种方式是对列表进行永久排序
print(list1)

list1.sort(reverse=True)  # 倒序
print(list1)

list1.reverse()  # 翻转
print(list1)

print(sorted(list1))  # sorted() 这种方式属于临时排序，不会影响到原列表

list2 = [3, 9, 6, 28, 0, -1, 16]
reversed_list2 = list2[::-1]  # 以切片的方式进行翻转
print(reversed_list2)
```

###  冒泡排序：

```python
# 冒泡排序
# 思路：两两之间进行比较,将大的往后放
# 第一轮比试，将最大的值放到最后
# [5,3,2]→[3,5,2]→[3,2,5]  n个数需要比较n-1次
# 第二轮比试，比较除了最后一个数之外的其他数
# [3,2,5]→[2,3,5]
list1 = [3, 9, 6, 28, 0, -1, 16]
for i in range(len(list1) - 1):
    for j in range(len(list1) - 1 - i):
        if list1[j] > list1[j + 1]:  # 相邻两数进行比较，如果前者比后者大，则交换位置
            #print('第{}位和第{}位的顺序不正确，需要交换顺序，交换前{}'.format(j, j + 1, list1))
            list1[j], list1[j + 1] = list1[j + 1], list1[j]
            #print('交换后{}'.format(list1))
        else:
            pass
            #print('第{}位和第{}位的顺序正确，不需要交换顺序，如下{}'.format(j, j + 1, list1))
print(list1)
```

 

 

