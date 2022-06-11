# 网络编程— —Socket

本文介绍Socket编程的知识。

[toc]

## 一、Socket基础知识

套接字(Socket)使用TCP提供了两台计算机之间的通信机制。 客户端程序创建一个套接字，并尝试连接服务器的套接字。当连接建立时，服务器会创建一个 Socket 对象。客户端和服务器现在可以通过对 Socket 对象的写入和读取来进行通信。

`java.net.Socket `类代表一个套接字， `java.net.ServerSocket `类为服务器程序提供了一种来监听客户端，并与他们建立连接的机制。

以下步骤描述了两台计算机如何使用套接字建立TCP连接：

- 服务器实例化一个 `ServerSocket` 对象，表示通过服务器上的端口通信。
- 服务器调用 `ServerSocket` 类的 `accept()` 方法，该方法将一直等待，直到客户端连接到服务器指定的端口。
- 服务器正在等待时，一个客户端实例化一个 `Socket` 对象，指定服务器IP地址和端口号来请求连接。
- `Socket` 类的构造函数试图将客户端连接到指定的服务器和端口号。如果通信被建立，则在客户端创建一个 `Socket` 对象，使之能够与服务器进行通信。
- 在服务器端，`accept()` 方法返回服务器上一个新的 `Socket` 引用，该 `socket` 连接到客户端的 `socket`。

连接建立后，通过使用 I/O 流进行通信，每一个`Socket`都有一个输出流和一个输入流，客户端的输出流连接到服务器端的输入流，而客户端的输入流连接到服务器端的输出流。

TCP 是一个双向的通信协议，因此数据可以通过两个数据流在同一时间发送。



## 二、常用方法

### 2.1 Socket常用方法

- **public Socket(String host, int port) **
  构造方法，创建一个套接字并将其连接到指定主机上的指定端口号。

- **public InputStream getInputStream() **

  返回此套接字的输入流。

- **public OutputStream getOutputStream() **

  返回此套接字的输出流。

- **public void shutdownOutput()**

  关闭输出流。

- **public void close()** 

  关闭套接字。



### 2.2 ServerSocket常用方法

- **public ServerSocket(int port)**
  创建绑定到特定端口的服务器套接字。
- **public Socket accept() **
  侦听并接受到此套接字的连接。



## 三、Socket编程演示

服务器端：

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8888);
        while (true) {
            Socket socket = serverSocket.accept();
            // Thread.run() start...
            InputStream inputStream = socket.getInputStream();
            byte[] msgBytes = new byte[0];
            byte[] buffer = new byte[1024];
            byte[] endFlag = new byte[2];
            int len = 0;
            while ((len = inputStream.read(buffer)) != -1) {
                byte[] tmp = new byte[msgBytes.length + len];
                System.arraycopy(msgBytes, 0, tmp, 0, msgBytes.length);
                System.arraycopy(buffer, 0, tmp, msgBytes.length, len);
                msgBytes = tmp;
                endFlag[0] = msgBytes[msgBytes.length-2];
                endFlag[1] = msgBytes[msgBytes.length-1];
                if (endFlag[0] =='\r' && endFlag[1] =='\n') {
                    break;
                }
            }
            System.out.println(new String(msgBytes, "UTF-8"));

            OutputStream outputStream = socket.getOutputStream();
            outputStream.write("收到".getBytes());
            socket.close();
            // Thread.run() end...
        }
    }
}

```

Client端代码：

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

public class Client {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("127.0.0.1",8888);
        OutputStream outputStream = socket.getOutputStream();
        StringBuffer stringBuffer = new StringBuffer();
        for(int i = 0; i < 1000; i++) {
            stringBuffer.append("你");
        }
        stringBuffer.append("\r\n");
        String msg = stringBuffer.toString();

        outputStream.write(new String(msg.getBytes(),"utf-8").getBytes());
        // 由于使用\r\n来表示消息的终止，则不用关闭输出流
        //socket.shutdownOutput();
        InputStream inputStream = socket.getInputStream();
        byte[] buffer = new byte[1024];
        int len = 0;
        while((len = inputStream.read(buffer))!=-1){
            System.out.println(new String(buffer,0,len));
        }
        socket.close();
    }
}

```





