# 实现HTTP服务器开发

## 网络套接字与通信

- 是进程之间的通信（浏览器的一个进程和服务器进程）
- 通过端口号来标记不同的网络进程，通过IP标记不同的计算机
- IP+端口号标记某台计算机的某个进程（IP:Port为一个套接字，表示TCP连接的一端）

### 套接字通信过程

**服务端：**

创建套接字->绑定套接字(bind)->监听套接字(listen)->处理信息

listen使主动连接套接口变为被动连接套接口，使得一个进程可以接受其他进程的请求，从而变成一个服务进程。



**客户端：**

创建套接字->连接套接字(connect)->处理信息



### 网络套接字与通信过程代码实现

#### 服务端编程

```python
#server.py

import socket

def server():
    # 1.创建套接字
    s = socket.socket()
    
    # 2.绑定
    HOST = 127.0.0.1
    PORT = 6666
    s.bind((HOST, PORT))
    
    # 3.监听
    # listen()函数内的数字表示可同时接受的请求数，但是只可以第一个链接的客户端进行通信
    s.listen(5)
    
    # 4.处理信息
    while True:
        # accept()接受的外来请求
        c, addr = s.accept()
        print("Connect client:",addr)
        cmsg = c.recv(1024)
        # 因为是面向字节流，必须转换为字节然后发包
        msg = b'I am the server,I have got it:\n'+cmsg
        c.send(msg)
    pass    
        
    
if __name__=='__main__':
    server()
```



#### 客户端编程

```python
#client.py

import socket

def client():
    # 1.创建套接字
    s = socket.socket()
    
    # 2.请求连接
    HOST = 127.0.0.1
    PORT = 6666
    s.connect((HOST, PORT))
    
    # 3.信息处理
    s.send(b'Hello,I want to connect!')
    msg = s.recv(1024)
    print('From Server: %s' %msg)
    
    
if __name__=='__main__':
    client()
```



## 面向TCP协议的套接字服务端编程

在网络套接字通信的基础上实现TCP的套接字服务

TCPSever实现接受客户端的TCP连接，Handler实现封装字节流网络请求处理功能，在TCPServer类中使用Handler来请求处理

### 实现网络服务器TCPServer类

#### 基本功能实现

- 启动: server_forever
- 处理请求: get_request、process_request、close_request
- 正常关闭: shutdown

#### 基本属性

- 服务端地址
- 处理器Handler
- 套接字

#### 代码实现

创建一个server文件夹

```python
#socket_server.py

import socket

class TCPServer:
    # TCPServer属性
    def __init__(self, server_addr, handler_class):
        self.server_addr = server_addr
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)# 服务于TCP连接
        self.HandlerClass = handler_class
        self.is_shutdown = False
        
    # 启动服务器（就是一个套接字通信过程）
    def server_forever(self):
        # 1.通过init创建套接字后，绑定
        self.socket.bind((self.server_addr))
        # 2.监听
        self.socket.listen(10)
        # 3.信息处理
        while not self.is_shutdown:
            # ①接受请求
            request, client_addr = self.get_request()
            # ②处理请求
            try:
                self.process_request(request,client_addr)
            except Exception as e:
                print(e)
            finally:
            # ③关闭请求
            	self.close_request(request)
            
    
    # 处理
    # 1.获取请求
    def get_request(self):
        return self.socket.accept()
    
    # 2.处理请求(根据需求自定义部分（修改Handler类）)
    def process_request(self, request, client_addr):
        handler = self.HandlerClass(request, client_addr)
        handler.handler()
    
    # 3.关闭请求
    def close_request(self, request):
        request.shutdown()
        request.close()
    
    # 关闭服务器
    def shut_down(self):
        self.is_shutdown = True
```



### 实现网络请求处理Handler类

需要定义两个类，一个定义连接处理的基本操作和相关属性的BaseRequestHandler类，一个是封装TCP连接处理逻辑的StreamRequestHandler类，StreamRequestHandler类是BaseRequestHandler类的继承类。

#### BaseRequestHandler

##### 相关属性

- 客户端请求（request）
- 客户端地址（client_addr)
- 服务端(server)

##### 基本操作

handler()

#### StreamRequestHandler

##### 基本操作

- 编码/解码：encode/decode
- 读/写消息：read/readline、write_content、send



#### 代码实现

```python
# base_handler.py
class BaseRequestHandler:
    def __init__(self, server, request, client_addr):
        self.server = server
        self.request = request
        self.client_addr = client_addr
        
    def handle(self):
        pass
    
class StreamRequestHandler(BaseRequestHandler):
        def __init__(self, server, request, client_address):
            BaseRequestHandler.__init__(self, server, request, client_address)

            self.rfile = self.request.makefile('rb')
            self.wfile = self.request.makefile('wb')
            self.wbuf = []
        
        # 编码：字符串->字节码
        def encode(self, msg):
            if not isinstance(msg, bytes):
                msg = bytes(msg, encoding='utf-8')
            return msg
        
        # 解码：字节码->字符串
        def decode(self, msg):
            if isinstance(msg, bytes):
                msg = msg.decode()
            return msg
        
        # 读消息
        def read(self, length):
            msg = self.rfile.read(length)
            return self.decode(msg)
        
        # 读取一行消息
        def readline(self, length=65536):
           msg = self.rfile.readline(length).strip()
           return self.decode(msg)
        
        
        # 写消息
        def write_content(self, msg):
             msg = self.encode(msg)
             self.wbuf.append(msg)
        
        # 发送消息
        def send(self):
            for line in self.wbuf:
                self.wfile.write(line)
            self.wfile.flush()
            self.wbuf = []
        
        def close(self):
            self.wfile.close()
            self.rfile.close()

        
```

