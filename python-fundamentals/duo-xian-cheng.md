# 多线程

## 并发

单个 CPU 逻辑上实现同一时间处理多个任务

N个 CPU 逻辑上实现同一时间处理 N +1 个任务

## 线程不安全和锁

```python
import time
import threading

balance = 500

r = threading.Lock()


def foo(num):
    # 将变量存到自己的系统
    global balance

    r.acquire()  # 上锁
    account_balance = balance

    time.sleep(2)  # 防止代码太少, CPU 执行速度太快，t1和 t2 变成

    account_balance = account_balance + num

    balance = account_balance

    r.release()  # 解锁


# 消费300 元
t1 = threading.Thread(target=foo, args=[-300])

t2 = threading.Thread(target=foo, args=[10000])

t1.start()
t2.start()

# 阻塞线程
t1.join()
t2.join()
print("所用线程运行结束后，余额为：", balance)
```

##  守护线程

```python
import time
import threading

a = []


def foo():
    while True:
        a.append("1")
        print("生产了一个数据")
        time.sleep(1)


t1 = threading.Thread(target=foo)
# 设置守护线程, 必须在 start 之前设置
#  其作用就是在主线程想要退出进程的时候，不需要等自己运行结束，直接退出就行了
# 因此主线程结束时候，不会等待t1结束。
t1.setDaemon(True)
t1.start()

for i in range(10):
    if a:
        a.remove("1")
        print("消费一个数据")
    time.sleep(1)

print("不再需要消费数据了")
```

 

## 死锁和同步锁

```python
import threading
import time

LockA = threading.Lock()
LockB = threading.Lock()


# 面试官
def foo1():
    LockA.acquire()
    print("请回答问题")
    time.sleep(1)

    LockB.acquire()
    print("发offer")
    time.sleep(1)

    LockA.release()
    LockB.release()


def foo2():
    LockB.acquire()
    print("请给我offer")
    time.sleep(1)

    LockA.acquire()
    print("回答问题")
    time.sleep(1)

    LockA.release()
    LockB.release()


t1 = threading.Thread(target=foo1)
t2 = threading.Thread(target=foo2)
t1.start()
t2.start()

# 阻塞线程
t1.join()
t2.join()
```

##  递归锁

```python
import threading
import time

LockR = threading.RLock()
"""
递归锁
"""


# 面试官
def foo1():
    LockR.acquire()
    print("请回答问题")
    time.sleep(1)

    LockR.acquire()
    print("发offer")
    time.sleep(1)

    LockR.release()
    LockR.release()


def foo2():
    LockR.acquire()
    print("请给我offer")
    time.sleep(1)

    LockR.acquire()
    print("回答问题")
    time.sleep(1)

    LockR.release()
    LockR.release()


t1 = threading.Thread(target=foo1)
t2 = threading.Thread(target=foo2)
t1.start()
t2.start()

# 阻塞线程
t1.join()
t2.join()
```

 

