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

     - 面板属于组件（Panel继承component）可以看成一个空间，但是不能单独存在，需要放在Frame上。

     - 在面板的源代码当中，有一个==FlowLayout==（流布局）概念。

     - 实验

       ```java
       package com.zengwei.Lesson02;
       
       import java.awt.*;
       import java.awt.event.WindowAdapter;
       import java.awt.event.WindowEvent;
       
       public class TestPanel {
           public static void main(String[] args) {
               Frame frame = new Frame();
               Panel panel = new Panel();
               //设置布局
               frame.setLayout(null);
               //设置坐标
               frame.setBounds(300,300,500,500);
               frame.setBackground(new Color( 255,255,255));
               //panel设置坐标（相对于frame来说）
               panel.setBounds(50,50,300,300);
               panel.setBackground(new Color(7, 7, 7));
               //将panel加入frame当中
               frame.add(panel);
               frame.setVisible(true);
               //监听窗口关闭事件
              frame.addWindowListener(new WindowAdapter() {
                  @Override
                  public void windowClosing(WindowEvent e) {
                      System.exit(0);
                  }
              });
           }
       }
       
       ```

       

- 三种布局管理器

  - 流式布局

    ```java
    package com.zengwei.Lesson02;
    import java.awt.*;
    import java.awt.event.WindowAdapter;
    import java.awt.event.WindowEvent;
    
    public class TestFlowLayout {
        public static void main(String[] args) {
            Frame frame = new Frame();
            //组件--button
            Button button_one = new Button("Button_one");
            Button button_two = new Button("Button_two");
            Button button_three = new Button("Button_three");
    
            //将按钮添加到frame当中去（可以先加入panel当中）,此处设置为流式布局
            //frame.setLayout(new FlowLayout());
            frame.setLayout(new FlowLayout(FlowLayout.LEFT));
            frame.add(button_one);
            frame.add(button_two);
            frame.add(button_three);
    
            frame.setSize(400,400);
            frame.setVisible(true);
    
    
            frame.addWindowListener(new WindowAdapter() {
                @Override
                public void windowClosing(WindowEvent e) {
                    System.exit(0);
                }
            });
        }
    }
    
    ```

    

  - 东西南北中布局

    ![image-20201019105531814](C:\Users\曾伟\Desktop\typora笔记\Github\项目\Java\assets\image-20201019105531814.png)

    - 实验代码

    ```java
    package com.zengwei.Lesson02;
    
    import java.awt.*;
    import java.awt.event.WindowAdapter;
    import java.awt.event.WindowEvent;
    
    public class TestDirection {
        public static void main(String[] args) {
            Frame frame = new Frame();
            Button east = new Button("east");
            Button west = new Button("west");
            Button north = new Button("north");
            Button south = new Button("south");
            Button center = new Button("center");
            //将按钮添加到BorderLayout布局文件当中去
            frame.add(east,BorderLayout.EAST);
            frame.add(west, BorderLayout.WEST);
            frame.add(north,BorderLayout.NORTH);
            frame.add(south,BorderLayout.SOUTH);
            frame.add(center,BorderLayout.CENTER);
            frame.setSize(400,400);
            frame.setVisible(true);
    
    
    
            frame.addWindowListener(new WindowAdapter() {
                @Override
                public void windowClosing(WindowEvent e) {
                    System.exit(0);
                }
            });
        }
    }
    
    ```

    - 效果图

      ![image-20201019110901668](C:\Users\曾伟\Desktop\typora笔记\Github\项目\Java\assets\image-20201019110901668.png)

  - 表格式布局

    - 实验

      ```java
      package com.zengwei.Lesson02;
      import java.awt.*;
      import java.awt.event.WindowAdapter;
      import java.awt.event.WindowEvent;
      public class TestGrid {
          public static void main(String[] args) {
              Frame frame = new Frame();
              //组件--button
              Button button_one = new Button("Button_one");
              Button button_two = new Button("Button_two");
              Button button_three = new Button("Button_three");
              Button button_five = new Button("button_five");
              Button button_four = new Button("button_four");
              Button button_six = new Button("button_six");
      
              frame.setLayout(new GridLayout(3,2));
              //将按钮添加进frame，GridLayout会自动补充
              frame.add(button_one);
              frame.add(button_two);
              frame.add(button_three);
              frame.add(button_four);
              frame.add(button_five);
              frame.add(button_six);
              frame.pack();//可选，pack（）函数会将布局优化
              frame.setSize(400,400);
              frame.setVisible(true);
              frame.addWindowListener(new WindowAdapter() {
                  @Override
                  public void windowClosing(WindowEvent e) {
                      System.exit(0);
                  }
              });
          }
      }
      ```

    - 实验结果

      ![image-20201019112824001](C:\Users\曾伟\Desktop\typora笔记\Github\项目\Java\assets\image-20201019112824001.png)

- 总结

  三种布局重点是如何灵活运用嵌套的方式使用三种布局。

## Swing

