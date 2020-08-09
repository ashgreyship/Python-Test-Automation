# 常见问题

### selenium 不可以想页面发送鼠标滚轮操作？

正确, 可以通过js进行操作。

### 有id 属性一定用id属性进行定位？

错误，动态id不能用来进行元素定位

### **如何提高selenium脚本的执行速度？**

* 使用更高配置的电脑和选择更快的网络环境
* 不要盲目的加sleep，尽量使用显式等待

### **关于 ui 自动化验证码处理正确的是**

* 测试环境关闭此功能
* 万能验证码
* 权限允许的情况下，登录服务器直接读取验证码

### **如何去定位属性动态变化的元素**

xpath或者css通过同级、父级、子级进行定位

### **找不到元素有哪些原因**

* 代码执行速度比元素加载速度快
* 元素在内嵌网页中，而我没有操作切换
* 元素在另一个标签页
* ~~元素被遮挡 （错误）~~


