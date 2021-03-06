# Socket

## Socket 流程：

![](../../.gitbook/assets/image%20%2824%29.png)

## 例子1

### client

```python
import socket

# 创建 socket 对象
sk = socket.socket()

# 发起链接
sk.connect(("127.0.0.1", 13000))

# 发送数据
sk.sendall(input("请输入>>>>").encode("utf-8"))

server_data = sk.recv(1024)
print("服务端的数据：", server_data.decode('utf-8'))

# 释放资源
sk.close()
```

### server

```python
import socket

# 创建 socket 对象

sk = socket.socket()

# 绑定IP 地址和端口号
# 元祖形式传参！
sk.bind(("127.0.0.1", 13000))

# 监听
sk.listen()
print("服务端已经启动了")

# 等待传入连接
# 有人连接前，处于阻塞状态
# 连接成功之后，会返回一个新的套接字和客户端的ip地址
conn, addr = sk.accept()
print("客户端IP地址：", addr)

# 接收数据
client_data = conn.recv(1024)
print("客户端的数据：", client_data.decode("utf-8"))

# 发送数据
conn.sendall(input("请输入>>>").encode("utf-8"))

# 释放资源
conn.close()
sk.close()
```

##  例子2 文件上传

### client

```python
import socket
import os

# 创建 socket 对象
sk = socket.socket()
sk.connect(('127.0.0.1', 36000))


def post_file(sk_obj, file_path):
    """
    上传文件
    :param sk_obj:  socket 对象
    :param file_path: 文件路径
    :return:
    """
    # 发送文件名
    file_name = os.path.split(file_path)[1]
    print(file_name)
    sk_obj.sendall(file_name.encode('utf-8'))

    # 避免粘包
    sk_obj.recv(1024)
    # 发送文件大小
    file_size = os.stat(file_path).st_size
    print(file_size)
    sk_obj.sendall(str(file_size).encode('utf-8'))
    # 避免粘包
    sk_obj.recv(1024)

    # 发送文件内容
    with open(file_path, 'rb') as f:
        while file_size > 0:
            sk_obj.sendall(f.read(1024))
            file_size -= 1024


file_path = './Snipaste_2020-08-23_09-53-42.png'
post_file(sk, file_path)
```

###  server

```python
import socket

# 创建 socket 对象
sk = socket.socket()
sk.bind(("127.0.0.1", 36000))
sk.listen()
print("服务端已经启动了")


def get_file(sk_obj):
    """
    接收文件
    :param sk_obj: socket 对象
    :return:
    """
    # 接收文件名
    file_name = sk_obj.recv(1024).decode('utf-8')
    print("接收文件的名字：", file_name)
    sk_obj.sendall(b"ok")

    # 接收文件大小
    file_size = int(sk_obj.recv(1024).decode('utf-8'))
    sk_obj.sendall(b"ok")

    # 接收文件正文
    with open('./%s' % file_name, 'wb') as f:
        while file_size > 0:
            f.write(sk_obj.recv(1024))
            file_size -= 1024


# 等待客户端连接
conn, addr = sk.accept()

# 开始接收文件
get_file(conn)
conn.close()
sk.close()
```

##  例子3 多客户端

### client1

```python
import socket

sk = socket.socket()
sk.connect(('127.0.0.1', 13000))
print('客户端上线了')

while True:
    # 向女神发送数据
    input_data = input('请输入>>>')
    sk.sendall(input_data.encode('utf-8'))

    if input_data == 'exit':
        break

    server_data = sk.recv(1024)
    print("服务端的数据：", server_data.decode('utf-8'))
```

### client2

```python
import socket

sk = socket.socket()
sk.connect(('127.0.0.1', 13000))
sk.listen()
print('客户端上线了')

while True:
    # 向女神发送数据
    input_data = input('请输入>>>')
    sk.sendall(input_data.encode('utf-8'))

    if input_data == 'exit':
        break

    server_data = sk.recv(1024)
    print("服务端的数据：", server_data.decode('utf-8'))
```

### server

```python
import socket

sk = socket.socket()
sk.bind(('127.0.0.1', 13000))
sk.listen()
print('服务端上线了')

while True:
    print('客户端空闲。。。。')
    conn, addr = sk.accept()
    print('有人来找客户端了', addr)

    while True:
        client_data = conn.recv(1024).decode('utf8')
        if client_data == 'exit':
            break
        print(client_data)

        # 回复消息
        conn.sendall(input('请输入>>>').encode('utf8'))

    conn.close()
```

## 例子4 持续发送消息 （SocketServer）

Python考虑得很周到，为了满足我们对多线程网络服务器的需求，提供了`socketserver`模块。socketserver在内部使用IO多路复用以及多线程/进程机制，实现了并发处理多个客户端请求的socket服务端。每个客户端请求连接到服务器时，socketserver服务端都会创建一个“线程”或者“进程” 专门负责处理当前客户端的所有请求。

### client1

```python
import socket

sk = socket.socket()
sk.connect(('127.0.0.1', 13000))
print('客户端上线了')

while True:
    # 向服务器发送数据
    input_data = input('请输入>>>')
    sk.sendall(input_data.encode('utf-8'))

    if input_data == 'exit':
        break

    server_data = sk.recv(1024)
    print("服务端的数据：", server_data.decode('utf-8')) 
```

### client2

```python
import socket

sk = socket.socket()
sk.connect(('127.0.0.1', 13000))
print('客户端上线了')

while True:
    # 向服务端发送数据
    input_data = input('请输入>>>')
    sk.sendall(input_data.encode('utf-8'))

    if input_data == 'exit':
        break

    server_data = sk.recv(1024)
    print("服务端的数据：", server_data.decode('utf-8'))
```

### server 

```python
import socketserver


class MyServer(socketserver.BaseRequestHandler):
    def handle(self):
        print("有人尝试连接服务器。。")

        while True:
            # 接收数据
            client_data = self.request.recv(1024).decode('utf-8')  # 相当于 conn.recv
            if client_data == 'exit':
                break
            print(client_data)
            # 发送数据
            self.request.sendall(input('请输入>>>').encode('utf-8'))
        self.request.close()


server = socketserver.ThreadingTCPServer(('127.0.0.1', 13000), MyServer)
print('服务器上线了')
server.serve_forever()
```

 



 

