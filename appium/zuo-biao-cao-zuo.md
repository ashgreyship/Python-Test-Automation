# 坐标操作

## Tap 点击

### 使用场景：当某个元素定位不到

```python
driver.tap([(463, 155)], 5000)
# 第一个值：x 第二个值：y 第三个值：长按时间（毫秒）
```

### 获取屏幕的高度

```python
size = driver.get_window_size()
height = size['height']
pos_y = height / 16  # 屏幕高度的1/16
driver.tap([(463, pos_y)])
```

##  Swipe 滑动

```python
# 设置隐式等待时间
driver.implicitly_wait(0)

# 获取屏幕大小
size = driver.get_window_size()
pos_x = size['width'] / 2
pos_y = size['height'] / 2
swipe_distance = size['height'] / 2

while 1:
    # 寻找目标元素
    target = driver.find_element_by_xpath('//*[@text="Python后端工程师"]')
    if target:
        print("找到目标岗位")
        # 测试滑动
        break
    driver.swipe(pos_x, pos_y, pos_x, pos_y - swipe_distance)

driver.implicitly_wait(10)

driver.quit()
```

## 通过切换 activity 

```python
driver.start_activity('包名','activity')
# 不推荐使用， 可能会失败
```



