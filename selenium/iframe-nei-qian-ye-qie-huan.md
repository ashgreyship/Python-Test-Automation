# iframe 内嵌页切换

 对于某些标签为 `iframe` 的元素，需要先切换到 `iframe`，才能对 `iframe` 内嵌页内的元素进行操作。

### 切换到 iframe 内嵌页

```python
driver.switch_to.frame(element_frame)
```

### 切换出 iframe 内嵌页

```python
driver.switch_to.default_content()
```

###  例子

```python

driver = webdriver.Chrome('/usr/local/bin/chromedriver')
driver.get('https://baidu.com')
driver.get('file:///Users/gabriel/PycharmProjects/trail1/selenium_note/iframe_test.html')

element_frame = driver.find_element_by_css_selector('iframe:nth-child(3)')
# 切换到内嵌网页
driver.switch_to.frame(element_frame)

driver.find_element_by_id('kw').send_keys('abc')

driver.switch_to.default_content()
```

