# Page object 设计模式



```python
from selenium import webdriver


class BaiDuPage:
    def __init__(self, path=None):
        # 创建 driver 对象
        self.driver = webdriver.Chrome('/usr/local/bin/chromedriver')
        # 最大化窗口
        self.driver.maximize_window()
        # 访问网址
        self.driver.get(path)

    # 抽离出浏览器元素控件
    def search_input_box(self):
        return self.driver.find_element_by_id("kw")


# 抽离出页面动作类, 继承对应的页面类
class BaiDuPagePageAction(BaiDuPage):

    def search(self):
        """
        访问百度界面,搜索 测试 两个字
        :return:
        """
        self.search_input_box().send_keys("测试\n")
        input("输入任意字符继续运行>>>")
        self.driver.quit()


if __name__ == '__main__':
    BaiDuPagePageAction("http://www.baidu.com").search()
```

注意需要实时定位元素。

由于页面重新加载会导致元素过期。 



