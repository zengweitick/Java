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
      
    - 总结、
    
      三种布局重点是如何灵活运用嵌套的方式使用三种布局。

## 1. 事件监听

  ```java
  package com.zengwei.Lesson02;
  
  import java.awt.*;
  import java.awt.event.ActionEvent;
  import java.awt.event.ActionListener;
  import java.awt.event.WindowAdapter;
  import java.awt.event.WindowEvent;
  
  public class TestAction {
      public static void main(String[] args) {
          Frame frame = new Frame();
          Button button = new Button("Press");
          MyListener myListener = new MyListener();
          button.addActionListener(myListener);
          button.setActionCommand("first");    //给事件传值
          frame.add(button);
          frame.pack();
          windowsClose(frame);
          frame.setVisible(true);
      }
      private  static  void windowsClose(Frame frame)
      {
          frame.addWindowListener(new WindowAdapter() {
              @Override
              public void windowClosing(WindowEvent e) {
                   System.exit(0);
              }
          });
      }
  }
  class MyListener implements ActionListener
  {
      @Override
      public void actionPerformed(ActionEvent e) {
          System.out.println(e.getActionCommand()+"you Pressed me");//获取值打印
      }
  }
  //监听类可以只写一个，对不同的事件分辨可以通过ActionEvent e参数获取
  ```



## 2. 输入框TextField监听

```java
package com.zengwei.Lesson02;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class TestText {
    public static void main(String[] args) {
      new Myframe();

    }
}
class Myframe extends Frame
{
    public  Myframe()
    {
        TextField textField = new TextField();
        add(textField);   //因为已经继承了Frame因此不需要写new
        TextFieldListener textFieldListener = new TextFieldListener();
        //按下回车键就会触发事件
        textField.addActionListener(textFieldListener);
        //设置成替换文本来模拟密码框
        textField.setEchoChar('*');
        setVisible(true);
        pack();
    }
}
class TextFieldListener implements ActionListener
{

    @Override
    public void actionPerformed(ActionEvent e) {
        TextField textField=(TextField) e.getSource();//获取资源，并返回对象Object（该对象可以强制转化成任何对象）
        String text = textField.getText();//在此处获取文本框的输入值
        System.out.println(text);

    }
}
```

## 实战：编写计算器（组合+内部类的回顾）

- 前言

  什么是组合？

  ```java
  //如何实现A类使用B类所有功能？
  1. 继承
      Class A extend B
  {
      
  }
  2. 在A当中私有一个B类
      Class A{
      public B b;
  }
  ```

---



- 代码实现（先面向过程的写代码，明白需要编写的东西，随后我们去优化代码）

  ```java
  package com.zengwei.Lesson02;
  import java.awt.*;
  import java.awt.event.ActionEvent;
  import java.awt.event.ActionListener;
  public class Caculater {
      public static void main(String[] args) {
          Calcu calcu = new Calcu();
      }
  }
  //计算器类
  class  Calcu extends Frame
  {
      public  Calcu()
      {
        //1. 一个按钮
          Button button = new Button("=");
  
          //2. 三个文本框
          TextField textField_1 = new TextField(10);//int 表示字符数
          TextField textField_2 = new TextField(10);
          TextField textField_3 = new TextField(10);
          //3. 一个标签
          Label label = new Label("+");
          //设置监听属性
          button.addActionListener(new CacuListener(textField_1,textField_2,textField_3));
          //布局
          setLayout(new FlowLayout());//流式布局
          //在布局当中加入组件
          add(textField_1);
          add(label);
          add(textField_2);
          add(button);
          add(textField_3);
          //设置自适应大小
          pack();
          //设置可见性
          setVisible(true);
      }
  }
  //监听器类
  class CacuListener implements ActionListener
  {
      //构造器设置有参方便传参
       private TextField f1,f2,f3;
  
      public CacuListener(TextField f1,TextField f2,TextField f3) {
          this.f1=f1;
          this.f2=f2;
          this.f3=f3;
      }
  
      @Override
      public void actionPerformed(ActionEvent e) {
          //1. 获得加数与被加数
          int first = Integer.parseInt(f1.getText());
          int  second = Integer.parseInt(f2.getText());
          //2. 将结果进行运算后放入第三个文本框当中
          f3.setText(""+(first+second));
          //3. 清除前两个框为下次输入做好准备
          f1.setText("");
          f2.setText("");
      }
  }
  ```

- 代码优化1--使用组合类（类中组合其他类）

  ```java
   
  package com.zengwei.Lesson02;
  import java.awt.*;
  import java.awt.event.ActionEvent;
  import java.awt.event.ActionListener;
  public class Caculater {
      public static void main(String[] args) {
         new Calcu().loadCacu();
      }
  }
  //计算器类
  class  Calcu extends Frame
  {
      TextField textField_1,textField_2,textField_3;
      //方法
      public void loadCacu()
      {
           textField_1 = new TextField(10);  
           textField_2 = new TextField(10);
           textField_3 = new TextField(10);
          Button button = new Button("=");
          button.addActionListener(new CacuListener(this));
          Label label = new Label("+");
          setLayout(new FlowLayout());
          add(textField_1);
          add(label);
          add(textField_2);
          add(button);
          add(textField_3);
          pack();
          setVisible(true);
      }
  }
  //监听器类
  class CacuListener implements ActionListener
  {
      
       Calcu  calcu=null;
      public CacuListener( Calcu  outCacu) {
         this.calcu=outCacu;
      }
      @Override
      public void actionPerformed(ActionEvent e) {
          int first = Integer.parseInt( calcu.textField_1.getText());
          int  second = Integer.parseInt(calcu.textField_2.getText());
          calcu.textField_3.setText(""+(first+second));
          calcu.textField_1.setText("");
          calcu.textField_2.setText("");
      }
  }
  
  ```

- 代码优化2--内部类（为了更好的包装即：内部类最大的好处是能够畅通无阻的访问外部类的方法）

  ```java
  package com.zengwei.Lesson02;
  
  import java.awt.*;
  import java.awt.event.ActionEvent;
  import java.awt.event.ActionListener;
  public class Caculater {
  public static void main(String[] args) {
         new Calcu().loadCacu();
      }
  }
  //计算器类
  class  Calcu extends Frame
  {
      TextField textField_1,textField_2,textField_3;
      //方法
      public void loadCacu()
      {
           textField_1 = new TextField(10); 
           textField_2 = new TextField(10);
           textField_3 = new TextField(10);
          Button button = new Button("=");
          button.addActionListener(new CacuListener());//直接调用监听不需要设置监听对象
          Label label = new Label("+");
          setLayout(new FlowLayout()); 
          add(textField_1);
          add(label);
          add(textField_2);
          add(button);
          add(textField_3); 
          pack();
          setVisible(true);
      }
   //将监听器代码写入类当中省去传对象操作，同时记得声明为private私有类
   private class CacuListener implements ActionListener
      {
          @Override
          public void actionPerformed(ActionEvent e) {
              
               int first = Integer.parseInt(textField_1.getText());
               int  second = Integer.parseInt(textField_2.getText());
               textField_3.setText(""+(first+second));
               textField_1.setText("");
               textField_2.setText("");
          }
      }
  }
  ```

  

## 3. 画笔（paint）

- 直接进入实验

  ```java
  package com.zengwei.Lesson02;
  
  import java.awt.*;
  
  public class Paint {
      public static void main(String[] args) {
        new MyPaint().loadFame();
      }
  
  }
  class MyPaint extends Frame
  {
      public void loadFame()
      {
          setBounds(200,200,600,500);
          setVisible(true);
      }
      //画笔
      @Override
      public void paint(Graphics g) {
          //设置画笔属性：颜色；以及功能：画画
          g.setColor(Color.red);
          g.drawOval(100,100,100,200);
          g.fillOval(100,100,100,100);//画实心的圆
          
      }
  }
  //在使用画笔结束时，记得把画笔还原成最初的颜色(将设置颜色代码注释掉重新运行一遍即可)，否则下次再次使用画笔时颜色可能不一样
  
  ```

- 结合鼠标监听事件模拟画图工具

  

- 

  

## Swing

