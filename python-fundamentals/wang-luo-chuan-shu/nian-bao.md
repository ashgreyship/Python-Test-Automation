# 粘包

### 粘包问题详情

只有TCP有粘包现象，UDP永远不会粘包

## **粘包的原因**

###  **直接原因**

所谓粘包问题主要还是因为接收方不知道消息之间的界限，不知道一次性提取多少字节的数据所造成的。

### 根本原因

发送方引起的粘包是由TCP协议本身造成的，TCP为提高传输效率，发送方往往要收集到足够多的数据后才发送一个TCP段。若连续几次需要send的数据都很少，通常TCP会根据**优化算法**把这些数据合成一个TCP段后一次发送出去，这样接收方就收到了粘包数据。

## 两种情况下会发生粘包

#### 1. 发送端需要等到本机的缓冲区满了以后才发出去，造成粘包（发送数据时间间隔很短，数据很小，python使用了优化算法，合在一起，产生粘包）

#### 客户端

```python
import socket

BUFSIZE = 1024
ip_port = ('127.0.0.1', 8080)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
res = s.connect_ex(ip_port)

s.send('hello'.encode('utf-8'))
s.send('feng'.encode('utf-8'))
```

#### 服务端

```python
from socket import *

ip_port = ('127.0.0.1', 8080)

tcp_socket_server = socket(AF_INET, SOCK_STREAM)
tcp_socket_server.bind(ip_port)
tcp_socket_server.listen(5)

conn, addr = tcp_socket_server.accept()

data1 = conn.recv(10)
data2 = conn.recv(10)

print('----->', data1.decode('utf-8'))
print('----->', data2.decode('utf-8'))

conn.close()
```

#### 2，接收端不及时接受缓冲区的包，造成多个包接受（客户端发送一段数据，服务端只收了一小部分，服务端下次再收的时候还是从缓冲区拿上次遗留的数据，就产生粘包）

#### 客户端

```python
import socket

BUFSIZE = 1024
ip_port = ('127.0.0.1', 8080)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
res = s.connect_ex(ip_port)

s.send('hello feng'.encode('utf-8'))
```

####  服务端

```python
from socket import *

ip_port = ('127.0.0.1', 8080)

tcp_socket_server = socket(AF_INET, SOCK_STREAM)
tcp_socket_server.bind(ip_port)
tcp_socket_server.listen(5)

conn, addr = tcp_socket_server.accept()

data1 = conn.recv(2)  # 一次没有收完整
data2 = conn.recv(10)  # 下次收的时候,会先取旧的数据,然后取新的

print('----->', data1.decode('utf-8'))
print('----->', data2.decode('utf-8'))

conn.close()
```

##  粘包问题如何解决？

**问题的根源在于，接收端不知道发送端将要传送的字节流的长度，所以解决粘包的方法就是围绕，如何让发送端在发送数据前，把自己将要发送的字节流总大小让接收端知晓，然后接收端来一个死循环接收完所有数据。**

### 简单的解决方法（从表面解决）：

在客户端发送下边添加一个时间睡眠，就可以避免粘包现象。在服务端接收的时候也要进行时间睡眠，才能有效的避免粘包情况。

#### 客户端：

```python
import socket
import time
import subprocess
din=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
ip_port=('127.0.0.1',8080)
din.connect(ip_port)
din.send('helloworld'.encode('utf-8'))
time.sleep(3)
din.send('sb'.encode('utf-8'))
```

#### 服务端:

```python
import socket
import time
import subprocess

din = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
ip_port = ('127.0.0.1', 8080)
din.bind(ip_port)
din.listen(5)
conn, deer = din.accept()
data1 = conn.recv(1024)
time.sleep(4)
data2 = conn.recv(1024)
print(data1)
print(data2)
```

### 普通的解决方法（从根本看问题）：

问题的根源在于，接收端不知道发送端将要传送的字节流的长度，所以解决粘包的方法就是围绕，如何让发送端在发送数据前，把自己将要发送的字节流总大小让接收端知晓，然后接收端来一个死循环接收完所有数据

为字节流加上自定义固定长度报头，报头中包含字节流长度，然后依次send到对端，对端在接受时，先从缓存中取出定长的报头，然后再取真是数据。

使用struct模块对打包的长度为固定4个字节或者八个字节，struct.pack.format参数是“i”时，只能打包长度为10的数字，那么还可以先将长度转化为json字符串，再打包。

#### 客户端

```python
import socket
import struct

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
phone.connect(('127.0.0.1', 8880))  # 连接服
while True:
    # 发收消息
    cmd = input('请你输入命令>>：').strip()
    if not cmd: continue
    phone.send(cmd.encode('utf-8'))  # 发送
    # 先收报头
    header_struct = phone.recv(4)  # 收四个
    unpack_res = struct.unpack('i', header_struct)
    total_size = unpack_res[0]  # 总长度
    # 后收数据
    recv_size = 0
    total_data = b''
    while recv_size < total_size:  # 循环的收
        recv_data = phone.recv(1024)  # 1024只是一个最大的限制
        recv_size += len(recv_data)  #
        total_data += recv_data  #
    print('返回的消息：%s' % total_data.decode('gbk'))
phone.close()
```

#### 服务端

```python
import socket
import subprocess
import struct

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # 买手机
phone.bind(('127.0.0.1', 8880))  # 绑定手机卡
phone.listen(5)  # 阻塞的最大数
print('start runing.....')
while True:  # 链接循环
    coon, addr = phone.accept()  # 等待接电话
    print(coon, addr)
    while True:  # 通信循环
        # 收发消息
        cmd = coon.recv(1024)  # 接收的最大数
        print('接收的是：%s' % cmd.decode('utf-8'))
        # 处理过程
        res = subprocess.Popen(cmd.decode('utf-8'), shell=True,
                               stdout=subprocess.PIPE,  # 标准输出
                               stderr=subprocess.PIPE  # 标准错误
                               )
        stdout = res.stdout.read()
        stderr = res.stderr.read()
        # 先发报头(转成固定长度的bytes类型，那么怎么转呢？就用到了struct模块)
        # len(stdout) + len(stderr)#统计数据的长度
        header = struct.pack('i', len(stdout) + len(stderr))  # 制作报头
        coon.send(header)
        # 再发命令的结果
        coon.send(stdout)
        coon.send(stderr)
    coon.close()
phone.close()
```

### 优化版的解决方法（从根本解决问题）

优化的解决粘包问题的思路就是服务端将报头信息进行优化，对要发送的内容用字典进行描述，首先字典不能直接进行网络传输，需要进行序列化转成json格式化字符串，然后转成bytes格式服务端进行发送，因为bytes格式的json字符串长度不是固定的，所以要用struct模块将bytes格式的json字符串长度压缩成固定长度，发送给客户端，客户端进行接受，反解就会得到完整的数据包。

#### 客户端：

```python
import socket
import struct
import json

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
phone.connect(('127.0.0.1', 8080))  # 连接服务器
while True:
    # 发收消息
    cmd = input('请你输入命令>>：').strip()
    if not cmd: continue
    phone.send(cmd.encode('utf-8'))  # 发送
    # 先收报头的长度
    header_len = struct.unpack('i', phone.recv(4))[0]  # 吧bytes类型的反解
    # 在收报头
    header_bytes = phone.recv(header_len)  # 收过来的也是bytes类型
    header_json = header_bytes.decode('utf-8')  # 拿到json格式的字典
    header_dic = json.loads(header_json)  # 反序列化拿到字典了
    total_size = header_dic['total_size']  # 就拿到数据的总长度了
    # 最后收数据
    recv_size = 0
    total_data = b''
    while recv_size < total_size:  # 循环的收
        recv_data = phone.recv(1024)  # 1024只是一个最大的限制
        recv_size += len(recv_data)  # 有可能接收的不是1024个字节，或许比1024多呢，
        # 那么接收的时候就接收不全，所以还要加上接收的那个长度
        total_data += recv_data  # 最终的结果
    print('返回的消息：%s' % total_data.decode('gbk'))
phone.close()
```

#### 服务端：

```python
import socket
import subprocess
import struct
import json

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # 买手机
phone.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
phone.bind(('127.0.0.1', 8080))  # 绑定手机卡
phone.listen(5)  # 阻塞的最大数
print('start runing.....')
while True:  # 链接循环
    coon, addr = phone.accept()  # 等待接电话
    print(coon, addr)
    while True:  # 通信循环
        # 收发消息
        cmd = coon.recv(1024)  # 接收的最大数
        print('接收的是：%s' % cmd.decode('utf-8'))
        # 处理过程
        res = subprocess.Popen(cmd.decode('utf-8'), shell=True,
                               stdout=subprocess.PIPE,  # 标准输出
                               stderr=subprocess.PIPE  # 标准错误
                               )
        stdout = res.stdout.read()
        stderr = res.stderr.read()
        # 制作报头
        header_dic = {
            'total_size': len(stdout) + len(stderr),  # 总共的大小
            'filename': None,
            'md5': None
        }
        header_json = json.dumps(header_dic)  # 字符串类型
        header_bytes = header_json.encode('utf-8')  # 转成bytes类型(但是长度是可变的)
        # 先发报头的长度
        coon.send(struct.pack('i', len(header_bytes)))  # 发送固定长度的报头
        # 再发报头
        coon.send(header_bytes)
        # 最后发命令的结果
        coon.send(stdout)
        coon.send(stderr)
    coon.close()
phone.close()
```

 

 

 

 

 

 

 

 



