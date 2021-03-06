# XPath 语法

```python
from selenium import webdriver

driver1 = webdriver.Chrome('/usr/local/bin/chromedriver')
driver1.get("https://m.weibo.cn/search?containerid=231583")

driver1.implicitly_wait(2)
# driver1.find_element_by_xpath('//*[@id="app"]/div[1]/div[1]/div[2]/div/div/div[8]/div/div/h4')

# 定位微博热搜榜
element1 = driver1.find_element_by_xpath('/html/body/div[1]/div[1]')
# 注意相对路径的点
result = element1.find_element_by_xpath('./div[1]/div[2]/div/div/div[8]/div/div/h4')
# 两个点则是上层目录
# result = element1.find_element_by_xpath('../div[1]/div[2]/div/div/div[8]/div/div/h4')

# / 一条斜杆是直接子元素
# // 两条斜杆则是从任意层级开始寻找

result.click()
```

## 属性定位：

```python
# 从任意层级定位指定标签，然后根据属性来定位具体的元素

# 从任意层级定位任意标签，根据属性id ="app" 进行匹配
driver1.find_element_by_xpath('//*[@id="app"]/div[1]/div[1]/div[2]/div/div/div[8]/div/div/h4')


# 从任意层级定位标签为p，然后根据id属性来定位具体的元素
driver1.find_element_by_xpath('//p[@id ="abd"]')
```

### 模糊匹配：

```python
# 模糊匹配
//*[starts-with(@tag,"string")]
//*[ends-with(@tag,"string")]
//*[contains(@tag,"string")]
```

## 选取当前节点的父节点

```text
../div[1]/div[2]/div/div/div[8]/div/div/h4

//*[@id="content_views"]/p[51]/span[1]/parent::tag
```

##  选取当前节点的先辈节点

```text
# 选中当前节点的先辈节点,只有直系父节点(父亲，祖父)才可以
/html/body/div[1]/div[1]/ancestor::tag
/html/body/div[1]/div[1]/ancestor-or-self::tag
```

## 选取当前节点的子孙节点

```text
# 匹配后代和自己 descendant 相当于 //
/html/body/div[1]/div[1]/descendant::tag
/html/body/div[1]/div[1]/descendant-or-self::tag
```

## 选取当前节点之前的所有节点

```text
/html/body/div[1]/div[1]/preceding::tag
```

## 选取当前节点之前的同级节点

```text
/html/body/div[1]/div[1]/preceding-sibiling::tag
```

## 节点的结束标签,之后的的所有节点

包括自己及自己的后代元素

```text
/html/body/div[1]/div[1]/following::tag
```



