# select 下拉框处理

针对带有 `select` 标签的下拉框

```python
from selenium import webdriver
from selenium.webdriver.support.select import Select

driver1 = webdriver.Chrome('/usr/local/bin/chromedriver')
driver1.get("file:///Users/gabriel/PycharmProjects/trail1/selenium_note/submenu.html")

# 带有 select 标签的下拉框

# 先定位到下拉框元素
element1 = driver1.find_element_by_id('abc')
# 用 value 选择
Select(element1).select_by_value('p2')
# 用 下标选择
Select(element1).select_by_index(0)
# 根据文本做选择
Select(element1).select_by_visible_text("月薪三十万")
```

 

