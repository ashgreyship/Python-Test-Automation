# 对话框切换

1. 对话框
2. 确认框
3. 提示框

```python
from selenium import webdriver
import time

driver = webdriver.Chrome('/usr/local/bin/chromedriver')
driver.get('file:///Users/gabriel/PycharmProjects/trail1/selenium_note/alert_test.html')


# 触发对话框
driver.find_element_by_id("bu1").click()
# 获取对话框
al = driver.switch_to.alert
# 确认对话框
al.accept()


# 触发确认框
driver.find_element_by_id('bu2').click()
# 获取确认框
al = driver.switch_to.alert
# 点击确认
al.accept()
driver.find_element_by_id('bu2').click()
# 点击取消
al.dismiss()

# 触发提示框
driver.find_element_by_id("bu3").click()

al = driver.switch_to.alert
al.send_keys('今天温度很高')
al.accept()
time.sleep(5)
driver.close()
```

 

