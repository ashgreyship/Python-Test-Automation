# 窗口截图

```python
from selenium import webdriver

driver = webdriver.Chrome('/usr/local/bin/chromedriver')
driver.get('https://www.baidu.com')

# 截取整个页面
driver.get_screenshot_as_file('./baidu_all.png')

# 截屏
element1 = driver.find_element_by_css_selector('form')
element1.screenshot('./baidu_searchbar.png')

driver.close()
```

 

