# 键盘操作

```python
from selenium import webdriver
import time
from selenium.webdriver.common.keys import Keys

driver = webdriver.Chrome('/usr/local/bin/chromedriver')
driver.get('https://baidu.com')

input1 = driver.find_element_by_id('kw')
input1.send_keys('selenium')
time.sleep(3)

# 全选操作
input1.send_keys(Keys.COMMAND, 'a')
time.sleep(3)

input1.send_keys(Keys.COMMAND, 'x')

input1.send_keys(Keys.COMMAND, 'c')

#  退格键
input1.send_keys(Keys.BACK_SPACE)

# 空格键
input1.send_keys(Keys.SPACE)

# Enter
input1.send_keys(Keys.ENTER)

# ESC
input1.send_keys(Keys.ESCAPE)

```

 

