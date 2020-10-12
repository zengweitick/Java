# Java网络编程

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

  