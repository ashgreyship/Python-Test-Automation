# cookie 获取&设置

## 获取 cookie

```python
from selenium import webdriver
import pprint

# 屏蔽 windows.navigator.webdriver
options = webdriver.ChromeOptions()
# 设置为开发者模式，防止被各大网站识别出来使用了Selenium
options.add_experimental_option('excludeSwitches', ['enable-automation'])
# # 不加载图片,加快访问速度
# options.add_experimental_option(
#     "prefs", {"profile.managed_default_content_settings.images": 2})

driver = webdriver.Chrome('/usr/local/bin/chromedriver',options=options)

driver.implicitly_wait(10)
driver.get("https://www.baidu.com")
input("手动登录百度后继续>>>")

# 获取 cookies 信息，这里至少要等待一下，等百度网站计算 cookie
pprint.pprint(driver.get_cookies())

driver.quit()
```

## 设置 cookie

```python
from selenium import webdriver

# driver1.get('https://baidu.com')

# 这是之前 cookieGet.py 获取的cookie，把每个字典的expiry 注释
cookies = [{'domain': '.baidu.com',
            'httpOnly': False,
            'name': 'H_PS_PSSID',
            'path': '/',
            'secure': False,
            'value': '1465_31670_32139_31253_32045_32230_32092_32257'},
           {'domain': '.baidu.com',
            # 'expiry': 1854017865,
            'httpOnly': True,
            'name': 'BDUSS',
            'path': '/',
            'secure': False,
            'value': 'MxcVQzNkFMfk9UVS1WYm9oUTU2cWlJVU1NLUFmazdnWjVsb3VKfmpmRklpalpmSVFBQUFBJCQAAAAAAAAAAAEAAADrZKExeW91cmxpZWhlYXZlbgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEj9Dl9I~Q5fRD'},
           {'domain': '.baidu.com',
            # 'expiry': 1626353851,
            'httpOnly': False,
            'name': 'BAIDUID',
            'path': '/',
            'secure': False,
            'value': '6F8C821CDE5B8517B7170421EAB2E165:FG=1'},
           {'domain': '.baidu.com',
            # 'expiry': 3742301498,
            'httpOnly': False,
            'name': 'BIDUPSID',
            'path': '/',
            'secure': False,
            'value': '6F8C821CDE5B851762F4B07FF13C506B'},
           {'domain': '.baidu.com',
            # 'expiry': 3742301498,
            'httpOnly': False,
            'name': 'PSTM',
            'path': '/',
            'secure': False,
            'value': '1594817851'},
           {'domain': 'www.baidu.com',
            # 'expiry': 1595681866,
            'httpOnly': False,
            'name': 'BD_UPN',
            'path': '/',
            'secure': False,
            'value': '123253'},
           {'domain': 'www.baidu.com',
            'httpOnly': False,
            'name': 'BD_HOME',
            'path': '/',
            'secure': False,
            'value': '1'}]  # 是个列表,列表里是一个个的字典

options = webdriver.ChromeOptions()
options.add_experimental_option('excludeSwitches', ['enable-automation'])
driver = webdriver.Chrome('/usr/local/bin/chromedriver', options=options)
driver.implicitly_wait(10)
driver.get("https://www.baidu.com")

# 获得所有cookie信息
driver.delete_all_cookies()
for cookie in cookies:
    # 添加 cookie
    driver.add_cookie(cookie)
driver.refresh()
```

 

