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

    

 ## TCP 实现聊天系统

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

  