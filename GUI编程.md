# GUI编程

## 准备工作

- 什么是GUI编程

  用Java类库写UI界面的编程

- 组件

  - 窗口
  - 面板
  - 弹窗
  - 文本框
  - 监听事件
  - 鼠标事件
  - 键盘事件

## 简介

- GUI核心技术---类库

   Swing、AWT

- 缺点

  界面不美观、需要JRE环境（太大）

- 学习的目的
  1. 熟悉MVC架构、了解监听器
  2. 可以写一些小工具



## AWT 

- 简介

  AWT中文含义是：Abstract Window Tools   即抽象的窗口工具，它包含了很多的类和接口供我们去使用。其所有的类库包含在java.awt包中。

- 组件和容器

  1. frame

     ```java
     package com.zengwei.Lesson01;
     
     import java.awt.*;
     
     public class Test_Frame {
         public static void main(String[] args) {
             Frame frame = new Frame("我的第一个java界面窗口");
             //设置可见性
             frame.setVisible(true);
             //设置窗口大小
             frame.setSize(400,400);
             //设置窗口颜色
             frame.setBackground(new Color(69, 136, 153));
             //设置初始位置
             frame.setLocation(400,400);
             //设置大小固定
             frame.setResizable(false);
         }
     }
     
     
     //将上述封装实现，并创建多个窗口
     package com.zengwei.Lesson01;
     
     import java.awt.*;
     
     public class Test_Frame {
         public static void main(String[] args) {
             myframe myframe1 = new myframe(100, 100, 200, 200, Color.blue);
             myframe myframe2 = new myframe(300, 100, 200, 200, Color.yellow);
             myframe myframe3 = new myframe(100, 300, 200, 200, Color.red);
             myframe myframe4 = new myframe(300, 300, 200, 200, Color.green);
         }
     }
     class myframe extends Frame {
         static  int count=0;  //由于我们可能需要多个窗口，因此我们需要计数器
         public myframe(int x,int y,int h,int w,Color color)
         {
            super("myframe"+(++count));
            setVisible(true);
            setSize(w,h);
            setBackground(color);
            setLocation(x,y);
         }
     }
     ```

     

     

  2. 面板

- 





## Swing

