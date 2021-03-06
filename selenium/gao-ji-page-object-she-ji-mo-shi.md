# 高级Page object 设计模式

![](../.gitbook/assets/image%20%283%29.png)

### 配置文件

```python
from selenium import webdriver

# 抽离 网址
URL = "http://172.20.10.8:8088/login"

# 超时时间
TIMEOUT = 10

# 轮询时间
POLL_FREQUENCY = 0.5

# driver 路径
driverPath = {
    'Chrome': '/usr/local/bin/chromedriver',
    'Firefox': ''
}
```

###  driver 工具类

```python
# 创建浏览器驱动的对象
from selenium import webdriver
from selenium_note.po.version_2.mySetting import driverPath, URL


class Driver:
    """
        浏览器driver 驱动类
    """
    # 类成员，初始化 driver 为空
    # 名字前面加下划线 主要是表示给子类继承
    _driver = None

    # 类方法，不需要实例化，可以直接调用
    @classmethod
    def getDriver(cls, browserName='Chrome'):
        if cls._driver is None:
            if browserName == 'Chrome':
                cls._driver = webdriver.Chrome(driverPath['Chrome'])
            elif browserName == 'Firefox':
                cls._driver = webdriver.Chrome(driverPath['Firefox'])

            cls._driver.maximize_window()

            cls._driver.get(URL)
        return cls._driver


# __name__ 是当前模块名，当模块被直接运行时模块名为 __main__
if __name__ == '__main__':
    Driver.getDriver()
```

### basepage 页面类

从driver 工具类获取driver

```python
from selenium.webdriver.support.wait import WebDriverWait
from selenium_note.po.version_2.mySetting import TIMEOUT, POLL_FREQUENCY
from selenium_note.po.version_2.myDriver import Driver
from selenium.webdriver.support import expected_conditions


class BasePage:
    def __init__(self):
        self.driver = Driver.getDriver()

    def get_element(self, locator):
        """
        （显示等待）根据表达式匹配元素列表 并且返回元素列表
        :param locator:  元祖, 第0个元素表示定位方法 比如 By.CSS_SELECTOR
                            ，第1个元素表示元素定位表达式  div:nth-child(1)
        :return:
        """
        WebDriverWait(
            # 传入浏览器对象
            driver=self.driver,
            # 传入超时时间
            timeout=TIMEOUT,
            # 传入轮询时间
            poll_frequency=POLL_FREQUENCY).until(
            expected_conditions.visibility_of_element_located(locator))

        # 返回元素对象
        return self.driver.find_elements(*locator)
```

###  loginpage 页面类

```python
from selenium_note.po.version_2.basePage import BasePage
from selenium.webdriver.common.by import By


class LoginPage(BasePage):
    def __init__(self):
        """
        进一步抽离出元素定位方法
        这里只封装孕照元素的方法，不会真的找元素
        """
        # 执行父类的构造方法
        super().__init__()

        # 用户名输入框
        self.usernameInputLocator = (By.NAME, "username")
        # 密码输入框
        self.passwordInputLocator = (By.NAME, 'password')
        # 登录按钮
        self.loginButtonLocator = (By.CSS_SELECTOR, 'button')

    # 用户名输入框
    def usernameInputBox(self):
        return self.get_element(self.usernameInputLocator)

    # 密码输入框
    def passwordInputBox(self):
        return self.get_element(self.passwordInputLocator)

    # 登录按钮
    def loginButtonBox(self):
        return self.get_element(self.loginButtonLocator)


# 抽离出 页面动作类  继承相应的页面类。
class LoginPageAction(LoginPage):
    def login(self):
        # 输入用户名
        self.usernameInputBox().send_keys("libai")
        # 输入密码
        self.passwordInputBox().send_keys("opmsopms123")
        # 点击登录按钮
        self.loginButtonBox().click()


if __name__ == "__main__":
    loginPageActionObj = LoginPageAction()
    loginPageActionObj.login()
```

 

