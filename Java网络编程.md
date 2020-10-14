# Java网络编程

## 1. 网络通信的要素

如何实现互相通信？

- 通信双方的通信地址

  1. IP地址:决定跟哪台计算机进行通信。
  2. 端口号：决定跟哪台计算机当中的哪个应用进行通信。

- 通信规则

  网络通信协议


## 2. IP

- IP基础知识

  1. IP地址的操作类：==InetAddress==

  2. 本机IP地址：127.0.0.1：环回地址

  3. IP地址的分类方法：IP地址分类（IPV4-IPV6）、公网-私网分类

  

- 实践：通过代码获取IP获取IP地址

  ```java
  import java.net.InetAddress;
  import java.net.UnknownHostException;
  
  public class Test {
      public static void main(String[] args) {
          try {
              InetAddress inetAddress=InetAddress.getByName("www.baidu.com");
              InetAddress inetAddress1=InetAddress.getLocalHost();
              System.out.println(inetAddress);
              System.out.println(inetAddress1);
          } catch (UnknownHostException e) {
              e.printStackTrace();
          }
      }
  }
  //InetAddress没有构造函数，如需new出一个对象需要调用该类的静态函数来返回IP地址
  ```

  结果显示：

  ```c++
  www.baidu.com/112.80.248.76
  DESKTOP-OF-ZENGWEI/10.101.6.59
  ```



## 3. 端口（port)

- 端口基本知识

  每个端口号对应电脑上某一运行的进程，即不同的进程对应不同的端口号。

- 在不同的协议（TCP、UDP）端口号都是从：0-65535，不同协议相同端口号不冲突。

- 端口分类

  1. 公有端口：0-1023（尽量不要去使用，它们被系统内置的进程占用）

     - http:80端口
     - https:443端口
     - FTP:21端口
     - telent：23端口

  2. 程序注册端口：1024-49151  这些端口用来分配给用户从程序的

     - tomcat：8080端口
     - MySQL：3306端口
     - Oracle：1521端口

  3. 动态端口（私有端口）：49152-65535

     常用命令

     ```c
     netstat -ano    //查看所有端口
     netstat -ano | findstr "字符串"  // 查看指定端口
     tasklist | findstr "字符串"     //查看指定端口的进程
     ```

  4. 实现类

     ```java
     import java.net.InetSocketAddress;
     public class SocketTest {
         public static void main(String[] args) {
             InetSocketAddress inetSocketAddress = new InetSocketAddress(                                                                           "127.0.0.1",8080);
             System.out.println(inetSocketAddress.getHostName());
             System.out.println(inetSocketAddress);
         }
     }
     ```

     

## 4. 通信协议

- 协议的含义

  解决通信双方的规则问题，是统一语言的方式，方便信息的编码与解编码

- 网络协议非常的多也非常的复杂，如何解决？
  ==分层==，TCP/IP协议簇是当经主流的网络协议簇

  TCP/IP协议簇比较重要的协议：

  - TCP：用户传输协议

  - IP：网络互连协议

    

 ## 5. TCP实践

###  实现聊天系统

- 服务端设计

  ```java
  
    package com.zengwei.Demo01;
  
  import java.io.ByteArrayOutputStream;
  import java.io.IOException;
  import java.io.InputStream;
  import java.net.ServerSocket;
  import java.net.Socket;
  
  //客户端
  public class TCPServer {
      public static void main(String[] args) {
          ByteArrayOutputStream byteArrayOutputStream =null;
          InputStream inputStream =null;
          Socket accept =null;
          ServerSocket serverSocket =null;
          try {
              //我需要一个地址
              //1. 创建地址
              serverSocket = new ServerSocket(9999);
              //2. 等待客户端连接,返回的accept为客户端申请连接的socket
               accept = serverSocket.accept();
              //3. 读取客户端的消息,利用管道流进行读取
               inputStream = accept.getInputStream();
              byteArrayOutputStream=  new ByteArrayOutputStream();
              byte [] buffer= new byte[1024];
              int len;
              while((len=inputStream.read(buffer))!=-1)
              {
                  byteArrayOutputStream.write(buffer,0,len);
              }
              System.out.println(byteArrayOutputStream.toString());
  
          } catch (IOException e) {  
              e.printStackTrace();
          }finally {   //关闭资源，类似于C++当中的析构函数，关闭顺序和创建顺序相反
              if(byteArrayOutputStream!=null)
              {
                  try {
                      byteArrayOutputStream.close();
                  } catch (IOException e) {
                      e.printStackTrace();
                  }
              }
              //关闭流
             if(inputStream!=null)
             {
                 try {
                     inputStream.close();
                 } catch (IOException e) {
                     e.printStackTrace();
                 }
             }
            if(accept!=null)
            {
                try {
                    accept.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
           if(serverSocket!=null)
           {
               try {
                   serverSocket.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }
  
          }
      }
  }  
  ```

- 客户端设计

  ```java
  package com.zengwei.Demo01;
  
  import java.io.IOException;
  import java.io.OutputStream;
  import java.net.Inet4Address;
  import java.net.InetAddress;
  import java.net.Socket;
  import java.net.UnknownHostException;
  
  //服务端
  public class TCPClient {
      public static void main(String[] args) {
          Socket socket =null;
          OutputStream outputStream=null;
          try {
              //我需要服务端的地址
              //1. 获取服务端的地址
              InetAddress  serverIp = Inet4Address.getByName("127.0.0.1");
              //2. 端口号
              int port=9999;
              //创建一个连接
               socket = new Socket(serverIp,port);
              //发送消息
               outputStream = socket.getOutputStream();
              outputStream.write("hello server".getBytes());
          } catch (Exception e) {
              e.printStackTrace();
          }finally {      //关闭资源，类似于C++当中的析构函数，关闭顺序和创建顺序相反
              if(outputStream!=null)
              {
                  try {
                      outputStream.close();
                  } catch (IOException e) {
                      e.printStackTrace();
                  }
              }
  
              if(socket!=null)
              {
                  try {
                      socket.close();
                  } catch (IOException e) {
                      e.printStackTrace();
                  }
              }
          }
      }
  }
  ```

### TCP实现文件上传

- 前言

  先找到一个图片，放到根目录当中。

- 服务端设计

  ```java
  package com.zengwei.Demo02;
  
  import java.io.*;
  import java.net.ServerSocket;
  import java.net.Socket;
  
  public class TCPServer {
      public static void main(String[] args) throws IOException {
          //1. 创建地址
          ServerSocket serverSocket = new ServerSocket(9000);
          //2. 等待连接
          Socket accept = serverSocket.accept();
          //3. 获取输入流
          InputStream inputStream = accept.getInputStream();
          //4. 文件输入
          FileOutputStream fileOutputStrea=new FileOutputStream(new File("receive.jpg"));
          byte[] buffer=new byte[1024];
          int len=0;
          while((len=inputStream.read(buffer))!=-1)
          {
              fileOutputStrea.write(buffer,0,len);
          }
          //5. 通知客户端我已经接受完毕，可以关闭资源了
          OutputStream  outputStream = accept.getOutputStream();
           outputStream.write("Geting is end! you can close the resources".getBytes());
  
           outputStream.close();
           fileOutputStrea.close();
           inputStream.close();
           accept.close();
           serverSocket.close();
      }
  }
  
  ```

  

- 客户端设计

  ```java
  package com.zengwei.Demo02;
  
  import java.io.*;
  import java.net.Inet4Address;
  import java.net.InetAddress;
  import java.net.Socket;
  import java.net.UnknownHostException;
  
  public class TCPClient {
      public static void main(String[] args) throws Exception {
          //1. 创建一个连接
          Socket socket = new Socket(Inet4Address.getByName("127.0.0.1"),9000);
          //2.创建一个输出流
          OutputStream outputStream = socket.getOutputStream();
          //3. 读取文件(此处为图片)
          FileInputStream fis=new FileInputStream(new File("dog.jpg"));
          //4. 写入流
          byte[] buffer=new byte[1024];
          int len=0;
          while((len=fis.read(buffer))!=-1)
          {
              outputStream.write(buffer,0,len);
          }
          //通知服务器我已经发送完毕，这一步很重要，否则会造成阻塞
          socket.shutdownOutput();
          //判断服务器是否收到消息
          InputStream inputStream = socket.getInputStream();
          //String Bytes[]
          ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
          byte [] buffer2=new byte[1024];
          int len2;
          while((len2=inputStream.read(buffer2))!=-1)
          {
              byteArrayOutputStream.write(buffer2,0,len2);
          }
          System.out.println(byteArrayOutputStream.toString());
  
          //5. 关闭资源
          fis.close();
          outputStream.close();
          socket.close();
      }
  }
  ```




## 6. UDP实践

###  简单应用

- 前言

  UDP是面向无连接的通信协议，该协议进行的动作只有两个：1. 建立Socket  2. 发送数据包

- 实验

  ```java
  //发送端
  package com.zengwei.Demo03;
  import java.net.*;
  public class UDPClient {
      public static void main(String[] args) throws Exception {
          //1.建立一个socket
          DatagramSocket  socket = new DatagramSocket();
          //2. 建立包
          String message="hello server";
          //获取发送目标地址
          InetAddress localhost = InetAddress.getByName("localhost");
          int port =9999;
          DatagramPacket datagramPacket = new DatagramPacket(message.getBytes(), 0, message.getBytes().length, localhost, port);//注意第三个参数为字节数组的长度，而非字符长度
          //3. 发送包
          socket.send(datagramPacket);
          //4. 关闭流
          socket.close();
      }
  }
  
  
  
  //接收端
  package com.zengwei.Demo03;
  import java.net.DatagramPacket;
  import java.net.DatagramSocket;
  import java.net.SocketException;
  public class UDPServer {
      public static void main(String[] args) throws  Exception {
          //开放端口
          DatagramSocket socket = new DatagramSocket(9999);
          //接受包\数据
          byte[] bytes = new byte[1024];
          DatagramPacket  Packet = new DatagramPacket(bytes, 0, bytes.length);
          socket.receive(Packet);      //阻塞方式接收数据包
          System.out.println(Packet.getAddress().getHostAddress());
          System.out.println(new String(Packet.getData()));//通过构造器将bytes类型转换成string类型
  
          socket.close();
  
      }
  }
  
  ```

### UDP实现简单的聊天系统

#### 1. 单向通信

```java
//sender
package com.zengwei.UDP_Chat;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetSocketAddress;
import java.net.SocketException;

public class Sender {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket(8888);
        //准备数据，从控制台读取
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
       while(true)
       {
           String data=bufferedReader.readLine();
           byte[] bytes = data.getBytes();
           DatagramPacket Packet = new DatagramPacket(bytes,0,bytes.length,new InetSocketAddress("localhost",6666));
           socket.send(Packet);
           if(data.equals("bye"))
           {
               break;
           }
       }


        bufferedReader.close();
        socket.close();
    }
}

//receiver
package com.zengwei.UDP_Chat;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

public class Receiver {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket(6666);
        while (true)
        {
            //准备接受数据
            byte[]  container = new byte[1024];
            DatagramPacket Packet = new DatagramPacket(container,0,container.length);
            socket.receive(Packet);  						  //阻塞式接受包裹

            //判断是否断开
            byte[] data=Packet.getData();                      //将包裹拆开
            String ReceiveData=new String(data,0,data.length); //string格式化
            System.out.println(ReceiveData);
            if(ReceiveData.equals("bye"))
            {
                break;
            }
        }
        socket.close();
    }
}

```

#### 2. 双向通信





