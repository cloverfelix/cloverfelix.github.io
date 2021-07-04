# GUI


# GUI编程

## 1.简介

#### Gui的核心技术：Swing，AWT。

#### 不流行的原因： 1.因为界面不美观

####                              2.需要jre环境 



### 为什么我们要学习？

#### 1. 是MVC架构的基础

#### 2.可以写出自己心中一些想要的工具

#### 3.工具时候，也可能需要维护swing界面，概率极小

#### 4.了解MVC架构，了解监听



## 2.AWT

### 2.1、AWT介绍

#### 1.包含了很多类和接口！GUI：图形用户界面编程

#### 2. 元素： 窗口，按钮，文本

#### 3. java.awt包

### 2.2 、组件和容器

#### 1.Frame

~~~java
package com.ren.lesson;

import java.awt.*;

//GUI第一个界面
public class TestFrame {
    public static void main(String[] args) {
        //Frame对象,看源码
        Frame frame = new Frame("我的第一个Java图像界面窗口");

        //需要设置可见性
        frame.setVisible(true);

        //设置窗口大小
        frame.setSize(400,400);

        //设置背景颜色
        frame.setBackground(new Color(85,150,68));

        //弹出初始位置
        frame.setLocation(200,200);

        //设置大小固定
        frame.setResizable(false);
    }
}

~~~

运行结果：

![image-20200623210634024](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200623210634024.png)

问题：发现窗口关闭不掉，停止Java运行！



尝试回顾封装：

~~~Java
package com.ren.lesson;

import java.awt.*;

public class TestFrame2 {
    public static void main(String[] args) {
        //展示多个窗口 
        MyFrame myFrame1 = new MyFrame(100,100,200,200,Color.BLACK);
        MyFrame myFrame2 = new MyFrame(300,100,200,200,Color.PINK);
        MyFrame myFrame3 = new MyFrame(100,300,200,200,Color.yellow);
        MyFrame myFrame4 = new MyFrame(300,300,200,200,Color.orange);


    }
}
class MyFrame extends Frame {
    static int id = 0;//可能存在多个窗口，我们需要一个计数器

    public  MyFrame(int x, int y, int w, int h,Color color){
        super("MyFrame"+(++id));
        setBackground(color);
        setBounds(x,y,w,h);
        setVisible(true);
        setResizable(false);
    }
}

~~~

结果

![image-20200623212644936](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200623212644936.png)

#### 2. 面板Pannel

解决了关闭窗口事件！

~~~Java
package com.ren.lesson;

import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

//Panel  可以看成是一个空间，但是不能单独存在
public class TestPanel {
    public static void main(String[] args) {

        Frame frame = new Frame();

        //面板中存在一个布局的概念
        Panel panel = new Panel();

        //设置布局
        frame.setLayout(null);

        //设置坐标
        frame.setBounds(300,300,500,500);
        frame.setBackground(Color.GREEN);

        //panel设置坐标，相对于frame
        panel.setBounds(50,50,400,400);
        panel.setBackground(new Color(193, 14, 26));

        //将面板添加到frame（窗口）中
        frame.add(panel);

        frame.setVisible(true);


        //监听事件，监听窗口关闭事件  System.exit(0)
        //适配器模式：
        frame.addWindowListener(new WindowAdapter() {
            //点击关闭窗口的时候需要做的事情
            @Override
            public void windowClosing(WindowEvent e) {
                //结束程序
                System.exit(0);
            }
        });
    }
}
~~~

结果

![image-20200623215138460](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200623215138460.png)

#### 3.布局管理器

- 流式布局   从左到右

~~~Java
package com.ren.lesson;

import java.awt.*;

public class TestFlowLayout {
    public static void main(String[] args) {
        Frame frame = new Frame();

        //组件---按钮组件
        Button button1 = new Button("button1");
        Button button2 = new Button("button2");
        Button button3 = new Button("button3");

        //设置为流式布局
        //frame.setLayout(new FlowLayout());
        //frame.setLayout(new FlowLayout(FlowLayout.LEFT));
        frame.setLayout(new FlowLayout(FlowLayout.RIGHT));

        frame.setSize(500,500);

        //把按钮添加上去
        frame.add(button1);
        frame.add(button2);
        frame.add(button3);

        frame.setVisible(true);


    }
}
~~~

- 东南西北中  上下结构

~~~Java
package com.ren.lesson;

import java.awt.*;

public class TestBorderLayout {
    public static void main(String[] args) {
        Frame frame = new Frame("TestBorderLayout");

        Button east = new Button("East");
        Button west = new Button("West");
        Button south = new Button("South");
        Button north = new Button("North");
        Button center = new Button("Center");

        frame.add(east,BorderLayout.EAST);
        frame.add(west,BorderLayout.WEST);
        frame.add(south,BorderLayout.SOUTH);
        frame.add(north,BorderLayout.NORTH);
        frame.add(center,BorderLayout.CENTER);


        frame.setVisible(true);
        frame.setSize(400,400);

    }
}
~~~

![image-20200623221203417](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200623221203417.png)

- 表格布局  三行两列布局 

~~~Java
package com.ren.lesson;

import java.awt.*;

public class TestGridLayout {
    public static void main(String[] args) {
        Frame frame = new Frame("TestGridLayout");

        Button btn1 = new Button("btn1");
        Button btn2 = new Button("btn2");
        Button btn3 = new Button("btn3");
        Button btn4 = new Button("btn4");
        Button btn5 = new Button("btn5");
        Button btn6 = new Button("btn6");

        frame.setLayout(new GridLayout(3,2));

        frame.add(btn1);
        frame.add(btn2);
        frame.add(btn3);
        frame.add(btn4);
        frame.add(btn5);
        frame.add(btn6);

        frame.pack();//这是一个Java函数，他会选择一个最优秀的布局。相当于自动布局，一般不用；这个可以不用设置大小，它会自动填充
        frame.setVisible(true);
    }
}
~~~

运行结果

![image-20200623222101158](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200623222101158.png)

<font color='red'>练习题</font>

![image-20200623230607106](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200623230607106.png)

分析及思路

![image-20200623231557916](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200623231557916.png)

### <font color='red'>切记，不要一开始就动手写代码，应当先分析</font>

~~~Java
package com.ren.lesson;

import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class ExDemo {

    public static void main(String[] args) {

        //总的窗口
        Frame frame = new Frame();
        frame.setSize(500,500);
        frame.setLocation(300,400);
        frame.setBackground(Color.GREEN);
        frame.setLayout(new GridLayout(2,1));
        frame.setVisible(true);

        //创建四个面板
        Panel p1 = new Panel(new BorderLayout());
        Panel p2 = new Panel(new GridLayout(2,1));
        Panel p3 = new Panel(new BorderLayout());
        Panel p4 = new Panel(new GridLayout(2,2));

        //先处理上半部分
        p1.add(new Button("East-1"),BorderLayout.EAST);
        p1.add(new Button("West-1"),BorderLayout.WEST);
        p2.add(new Button("p2-btn-1"));
        p2.add(new Button("p2-btn-2"));
        p1.add(p2,BorderLayout.CENTER);

        //在处理下半部分
        p3.add(new Button("East-2"),BorderLayout.EAST);
        p3.add(new Button("West-2"),BorderLayout.WEST);
        for (int i = 0; i < 4; i++) {
            p4.add(new Button("p4-btn-"+(++i)));
        }
        p3.add(p4,BorderLayout.CENTER);

        //再将所有的面板加入总窗口中
        frame.add(p1);
        frame.add(p3);

        //设置监听事件
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
}
~~~

结果图

![image-20200623234302447](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200623234302447.png)

### 总结：

#### 1.Frame是一个顶级窗口

#### 2.Panel无法单独显示，必须添加到某个容器中。

#### 3. 布局管理器

####              1. 流式

####              2. 东南西北中

####              3. 表格

####              4. 大小，定位，背景颜色，可见性，监听！





#### 4. 事件监听

~~~Java
package com.ren.lesson2;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.sql.SQLOutput;

public class TestActionEvent {
    public static void main(String[] args) {
        //按下按钮，触发一些事件
        Frame frame = new Frame();

        Button button = new Button("test1");

        //因为，addActionListener()需要一个ActionListener，所以我们需要构造一个ActionListener

        button.addActionListener(new MyActionListener());
        frame.add(button,BorderLayout.CENTER);
        frame.pack();
        windowClose(frame);
        frame.setVisible(true);

    }

    //关闭窗口的事件
    private static void windowClose(Frame frame){
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
}
//事件监听
class MyActionListener implements ActionListener{

    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("aaa");
    }
}
~~~



#### 多个按钮共享一个事件

~~~Java
package com.ren.lesson2;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestActionEventTwo {
    public static void main(String[] args) {
        //两个按钮实现同一个监听
        Frame frame = new Frame("开始-停止");

        Button button1 = new Button("start");
        Button button2 = new Button("stop");

        //可以显示的定义触发会返回的命令，如果不显示的定义，则会走默认的的值！一般不怎么用
        //可以多个按钮只写一个监听类
        button2.setActionCommand("button-stop");

        MyMonitor myMonitor = new MyMonitor();
        button1.addActionListener(myMonitor);
        button2.addActionListener(myMonitor);

        frame.add(button1,BorderLayout.NORTH);
        frame.add(button2,BorderLayout.SOUTH);

        frame.pack();
        windowClose(frame);
        frame.setVisible(true);
    }
    //关闭窗口的事件
    private static void windowClose(Frame frame){
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
}

class MyMonitor implements ActionListener{

    @Override
    public void actionPerformed(ActionEvent e) {
        //e.getActionCommand() 获取按钮的信息
        System.out.println("按钮被点击了：msg=>"+e.getActionCommand());
        if(e.getActionCommand().equals("start")){

        }
    }
}
~~~



#### 5. 输入框TextField监听

~~~Java
package com.ren.lesson2;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class TestText01 {

    public static void main(String[] args) {
        //main方法只管启动
        new MyFrame();
    }
}
class MyFrame extends Frame{
    public MyFrame(){
        TextField textField = new TextField();
        add(textField);


        //监听这个文本输入框的文字
        MyActionListener2 myActionListener2 = new MyActionListener2();
        //按下enter键就会触发这个输入框的事件
        textField.addActionListener(myActionListener2);

        //设置替换编码
        textField.setEchoChar('*');

        setVisible(true);
        pack();
    }
}

class MyActionListener2 implements ActionListener{

    @Override
    public void actionPerformed(ActionEvent e) {
        TextField field = (TextField) e.getSource();//获取一些资源，返回的是一个对象
        System.out.println(field.getText());//获取输入框的文本
        field.setText("");//按下回车后输入框清空
    }
}
~~~

#### 6.简易计算器，组合+内部类回顾复习

#### oop原则：组合大于继承！

##### 目前代码  （面向过程）

~~~Java
package com.ren.lesson2;

/*
* 1.先创建3个文本框 1个按钮 1个标签 （需要对按钮事件进行监听）
* 2.在对上面的所创建的进行布局，并且添加到Frame框中
* 3.创建监听类对按钮事件进行监听，并且还需获取文本框中的参数
* 4.在进行计算机的计算 首现获取加数跟被加数  再将这个值加法运算后，放入第三个框  最后清除前两个框中内容
* */

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

//简易计算机
public class TestCalc {

    public static void main(String[] args) {
        new Calculator();
    }
}

class Calculator extends Frame{
    public Calculator(){
        //3 个文本框
        TextField num1 = new TextField(10);//参数为文本框字符数
        TextField num2 = new TextField(10);//参数为文本框字符数
        TextField num3 = new TextField(20);//参数为文本框字符数

        //1 个按钮
        Button button = new Button("=");
        button.addActionListener(new MyCalculator(num1,num2,num3));

        //1 个标签
        Label label = new Label("+");

        //布局
        setLayout(new FlowLayout());

        add(num1);
        add(label);
        add(num2);
        add(button);
        add(num3);

        //关闭窗口事件
        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
        pack();
        setVisible(true);
    }
}

class MyCalculator implements ActionListener{

    private TextField num1,num2,num3;

    public MyCalculator(TextField num1,TextField num2, TextField num3){
        this.num1 = num1;
        this.num2 = num2;
        this.num3 = num3;
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        //1.获取加数跟被加数
        int n1 = Integer.parseInt(num1.getText());
        int n2 = Integer.parseInt(num2.getText());

        //2.将这个值加法运算后，放到第三个框
        num3.setText(""+(n1+n2));

        //清除前两个框
        num1.setText("");
        num2.setText("");
    }
}
~~~



##### 完全改造为面向对象写法（组合）

~~~Java
package com.ren.lesson2;

/*
* 1.先创建3个文本框 1个按钮 1个标签 （需要对按钮事件进行监听）
* 2.在对上面的所创建的进行布局，并且添加到Frame框中
* 3.创建监听类对按钮事件进行监听，并且还需获取文本框中的参数
* 4.在进行计算机的计算 首现获取加数跟被加数  再将这个值加法运算后，放入第三个框  最后清除前两个框中内容
* */

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

//简易计算机
public class TestCalc {

    public static void main(String[] args) {
        new Calculator().loadFrame();
    }
}

class Calculator extends Frame{

    public Calculator(){

    }

    //属性
    TextField num1,num2,num3;

    //方法
    public void loadFrame(){
        //3 个文本框
        num1 = new TextField(10);//参数为文本框字符数
        num2 = new TextField(10);//参数为文本框字符数
        num3 = new TextField(20);//参数为文本框字符数

        //1 个标签
        Label label = new Label("+");

        //1 个按钮
        Button button = new Button("=");
        button.addActionListener(new MyCalculator(this));

        //布局
        setLayout(new FlowLayout());

        add(num1);
        add(label);
        add(num2);
        add(button);
        add(num3);

        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
        pack();
        setVisible(true);
    }
}

class MyCalculator implements ActionListener{

    //获取三个变量
    //private TextField num1,num2,num3;
    //获取计算器这个对象，在一个类中组合另外一个类
    Calculator calculator = null;

    public MyCalculator(Calculator calculator){
        this.calculator = calculator;
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        //1.获取加数跟被加数
        int n1 = Integer.parseInt(calculator.num1.getText());
        int n2 = Integer.parseInt(calculator.num2.getText());

        //2.将这个值加法运算后，放到第三个框
        calculator.num3.setText(""+(n1+n2));

        //清除前两个框
        calculator.num1.setText("");
        calculator.num2.setText("");
    }
}
~~~



##### 内部类

更好的包装

~~~Java
package com.ren.lesson2;

/*
* 1.先创建3个文本框 1个按钮 1个标签 （需要对按钮事件进行监听）
* 2.在对上面的所创建的进行布局，并且添加到Frame框中
* 3.创建监听类对按钮事件进行监听，并且还需获取文本框中的参数
* 4.在进行计算机的计算 首现获取加数跟被加数  再将这个值加法运算后，放入第三个框  最后清除前两个框中内容
* */

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

//简易计算机
public class TestCalc {
    public static void main(String[] args) {
        new Calculator().loadFrame();
    }
}

class Calculator extends Frame{

    public Calculator(){

    }

    //属性
    TextField num1,num2,num3;

    //方法
    public void loadFrame(){
        //3 个文本框
        num1 = new TextField(10);//参数为文本框字符数
        num2 = new TextField(10);//参数为文本框字符数
        num3 = new TextField(20);//参数为文本框字符数

        //1 个标签
        Label label = new Label("+");

        //1 个按钮
        Button button = new Button("=");
        button.addActionListener(new MyCalculator());

        //布局
        setLayout(new FlowLayout());

        add(num1);
        add(label);
        add(num2);
        add(button);
        add(num3);

        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
        pack();
        setVisible(true);
    }

    //监听器类
    //内部类最大的好处，就是可以畅通无阻的访问外部类的属性和方法
    private class MyCalculator implements ActionListener{

        //获取三个变量
        //private TextField num1,num2,num3;
        //获取计算器这个对象，在一个类中组合另外一个类
//        Calculator calculator = null;
//
//        public MyCalculator(Calculator calculator){
//            this.calculator = calculator;
//        }

        @Override
        public void actionPerformed(ActionEvent e) {
            //1.获取加数跟被加数
            int n1 = Integer.parseInt(num1.getText());
            int n2 = Integer.parseInt(num2.getText());

            //2.将这个值加法运算后，放到第三个框
            num3.setText(""+(n1+n2));

            //清除前两个框
            num1.setText("");
            num2.setText("");
        }
    }
}
~~~



#### 7.画笔

~~~Java
package com.ren.lesson03;

import java.awt.*;

public class TestPaint {
    public static void main(String[] args) {
        new MyPaint().loadframe();
    }
}
class MyPaint extends Frame{

    public void loadframe(){
        setBounds(200,200,600,500);
        setVisible(true);
    }

    //画笔
    @Override
    public void paint(Graphics g) {
        //super.paint(g);
        //1、画笔需要颜色  2、画笔可以画画
        g.setColor(Color.red);
        //g.drawOval(100,100,100,100);
        g.fillOval(100,100,100,100);//实心的圆
        g.setColor(Color.GREEN);
        g.fillRect(150,200,200,200);

        //养成习惯，画笔用完将它还原到最初的颜色
    }
}
~~~

#### 8.鼠标监听

目的：想要实现鼠标画画！

思路图

![image-20200625192237083](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200625192237083.png)

~~~Java
package com.ren.lesson03;

import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.util.ArrayList;
import java.util.Iterator;

//鼠标监听事件
public class TestMouseListener {
    public static void main(String[] args) {
        new MyFrame("画画");
    }
}

//自己的类
class MyFrame extends Frame{

    ArrayList points;

    public MyFrame(String title){
        super(title);
        setBounds(200,200,400,300);
        //存放鼠标点击的点
        points = new ArrayList<>();

        setVisible(true);


        //鼠标监听器，针对这个窗口
        this.addMouseListener(new MyMouseListener());

        //窗口关闭监听器
        addWindowListener(new MyListener());
    }

    //窗口关闭事件
    class MyListener extends WindowAdapter{
        @Override
        public void windowClosing(WindowEvent e) {
            //setVisible(false);//隐藏窗口，并不是关闭了，通过按钮，隐藏当前窗口
            System.exit(0);//正常退出
        }
    }


    @Override
    public void paint(Graphics g) {
        //画画，监听鼠标的事件
        Iterator iterator = points.iterator();
        while (iterator.hasNext()){
            Point point = (Point) iterator.next();//指针指向下一个点
            g.setColor(Color.GREEN);//设置点的颜色
            g.fillOval(point.x,point.y,10,10);
        }
    }

    //添加一个点到界面上
    public void addPaint(Point point){
        //将鼠标点击的点存放到集合中
        points.add(point);
    }

    //采用适配器模式，如果使用接口，则需要实现多个方法
    private class MyMouseListener extends MouseAdapter{
        //鼠标  按下，弹起，按住不放
        @Override
        public void mousePressed(MouseEvent e) {
            MyFrame frame = (MyFrame)e.getSource();
            //这个点就是鼠标的点
            frame.addPaint(new Point(e.getX(),e.getY()));

            //每次点击鼠标都需要重新画一遍
            frame.repaint();//刷新后，就会有一个点就会去执行画画
        }
    }
}
~~~

#### 9.窗口监听

~~~Java
package com.ren.lesson03;

import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestWindow {
    public static void main(String[] args) {
        new WindowFrame();
    }
}
class WindowFrame extends Frame{
    public WindowFrame(){

        setBackground(Color.GREEN);
        setBounds(100,100,200,200);
        setVisible(true);

        this.addWindowListener(new WindowAdapter() {
            //匿名内部类  更方便
            @Override
            public void windowClosing(WindowEvent e) {
                System.out.println("windowClosing");
                System.exit(0);
            }

            @Override
            public void windowActivated(WindowEvent e) {
                WindowFrame source = (WindowFrame) e.getSource();
                source.setTitle("被激活了");
                System.out.println("windowActivated");
            }
        });
    }
}
~~~

#### 10.键盘监听

~~~Java
package com.ren.lesson03;

import java.awt.*;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;

public class TestKeyListener {
    public static void main(String[] args) {
        new KeyFrame();
    }
}

class KeyFrame extends Frame{
    public KeyFrame() {
        setBounds(1,2,300,400);
        setVisible(true);

        this.addKeyListener(new KeyAdapter() {
            //键盘按下
            @Override
            public void keyPressed(KeyEvent e) {
                //获得键盘按下的键是哪一个   当前的码
                int keyCode = e.getKeyCode();//不需要去记录这个数值，直接使用静态属性  VK_XXXX
                System.out.println(keyCode);
                if(keyCode == KeyEvent.VK_UP){
                    System.out.println("你按下了上键");
                }
                //根据按下不同操作，产生不同结果
            }
        });
    }
}
~~~





## 3.Swing

#### 3.1、窗口、面板

~~~JAVA
package com.ren.lesson04;

import javax.swing.*;
import java.awt.*;

public class JFrameDemo {

    //init()初始化
    public void init(){
        JFrame jf = new JFrame("这是一个JFrame窗口");
        jf.setVisible(true);
        jf.setBounds(100,100,400,500);
        jf.setBackground(Color.GREEN);

        //设置文字 Jlabel
        JLabel jLabel = new JLabel("欢迎来到篮球世界");

        jf.add(jLabel);

        //容器实例化(窗口本身也是一个容器，我们要让这个窗口能看得到，所以对其进行实例化)

        //关闭事件
        jf.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }


    public static void main(String[] args) {
        //建议一个窗口
        new JFrameDemo().init();

    }
}
~~~



#### 标签居中

~~~JAVA
package com.ren.lesson04;

import javax.swing.*;
import java.awt.*;

public class JFrameDemo02 {
    public static void main(String[] args) {
        new MyJFrame2().init();
    }
}
class MyJFrame2 extends JFrame {
    public void init(){
        this.setVisible(true);
        this.setBounds(10,10,400,500);

        //设置文字 Jlabel
        JLabel jLabel = new JLabel("欢迎来到篮球世界");

        this.add(jLabel);

        //让我们的文本标签居中  设置水平对齐
        jLabel.setHorizontalAlignment(SwingConstants.CENTER);

        //获得一个容器（这个容器中的颜色才是它真正的颜色）
        Container container = this.getContentPane();
        container.setBackground(Color.red);
    }
}
~~~



#### 3.2 、弹窗

JDialog  用来被弹出，默认就有关闭事件！

~~~JAVA
package com.ren.lesson04;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

//主窗口
public class DialogDemo extends JFrame {

    public DialogDemo() {
        this.setVisible(true);
        this.setSize(700,600);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

        //JFrame 放东西，需要一个容器
        Container container = this.getContentPane();
        //绝对布局
        container.setLayout(null);

        //创建按钮
        JButton button = new JButton("点击弹出一个对话框");
        button.setBounds(30,30,200,50);

        //点击这个按钮的时候，弹出一个窗口
        button.addActionListener(new ActionListener() { //监听器
            @Override
            public void actionPerformed(ActionEvent e) {
                //显示弹窗
                new MyDialogDemo();
            }
        });

        container.add(button);

    }

    public static void main(String[] args) {
        new DialogDemo();
    }
}

class MyDialogDemo extends JDialog{
    public MyDialogDemo() {
        this.setVisible(true);
        this.setBounds(200,200,300,300);

        Container container = this.getContentPane();
        container.setLayout(null);

        JLabel label = new JLabel("欢迎来到篮球世界");
        container.add(label);

    }
}
~~~

结果图：

![image-20200710112454592](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710112454592.png)



#### 3.3、标签

label

~~~JAVA
new JLabel("xxxxx");
~~~

图标  ICON

~~~JAVA
 package com.ren.lesson04;


import javax.swing.*;
import java.awt.*;

//图标是一个接口，需要实现类
public class IconDemo extends JFrame implements Icon {
    private int width;
    private int height;

    public IconDemo(){}

    public IconDemo(int width,int height){
        this.width = width;
        this.height = height;
    }

    public void init(){
        IconDemo iconDemo = new IconDemo(15,15);
        //图片放在标签上，也可以放在按钮上！
        JLabel label = new JLabel("icontest",iconDemo,SwingConstants.CENTER);

        Container container = getContentPane();
        container.add(label);

        this.setVisible(true);
        this.setBounds(100,100,350,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }



    public static void main(String[] args) {
        new IconDemo().init();
    }

    @Override
    public void paintIcon(Component c, Graphics g, int x, int y) {
        g.fillOval(x,y,width,height);
    }

    @Override
    public int getIconWidth() {
        return width;
    }

    @Override
    public int getIconHeight() {
        return height;
    }
}
~~~

结果图：

![image-20200710173615391](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710173615391.png)

图片Icon

~~~JAVA
package com.ren.lesson04;

import javax.swing.*;
import java.awt.*;
import java.net.URL;

public class ImageDemo extends JFrame {

    public ImageDemo() {
        //获取图片的地址
        JLabel label = new JLabel("ImageIcon");
        //获取当前类目录下的资源
        URL url = ImageDemo.class.getResource("tx.jpg");

        //将图片放到标签中(命名不要冲突了)
        ImageIcon imageIcon = new ImageIcon(url);
        label.setIcon(imageIcon);
        label.setHorizontalAlignment(SwingConstants.CENTER);

        Container container = getContentPane();
        container.add(label);

        setVisible(true);
        setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        setBounds(100,100,350,350);
    }

    public static void main(String[] args) {
        new ImageDemo();
    }
}
~~~

<font color='red'>出现报错，空指针异常，为找出哪里异常！！！重新开启IDEA就好了 </font>

结果图：

![image-20200710183720525](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710183720525.png)

#### 3.4、面板

##### JPanel

~~~JAVA
package com.ren.lesson05;

import javax.swing.*;
import java.awt.*;

public class JPanelDemo extends JFrame {
    public JPanelDemo() {
        Container container = this.getContentPane();

        container.setLayout(new GridLayout(2,1,10,10));//后面参数的意思是间距

        JPanel panel1 = new JPanel(new GridLayout(1,3));
        JPanel panel2= new JPanel(new GridLayout(1,2));
        JPanel panel3 = new JPanel(new GridLayout(2,1));
        JPanel panel4 = new JPanel(new GridLayout(3,2));

        panel1.add(new JButton("1"));
        panel1.add(new JButton("1"));
        panel1.add(new JButton("1"));
        panel2.add(new JButton("2"));
        panel2.add(new JButton("2"));
        panel3.add(new JButton("3"));
        panel3.add(new JButton("3"));
        panel4.add(new JButton("4"));
        panel4.add(new JButton("4"));
        panel4.add(new JButton("4"));
        panel4.add(new JButton("4"));
        panel4.add(new JButton("4"));
        panel4.add(new JButton("4"));

        container.add(panel1);
        container.add(panel2);
        container.add(panel3);
        container.add(panel4);

        this.setVisible(true);
        this.setSize(350,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new JPanelDemo();
    }
}
~~~

结果图：

![image-20200710175624428](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710175624428.png)

##### JScrollPanel

~~~java 
package com.ren.lesson05;

import javax.swing.*;
import java.awt.*;

public class JScrollDemo extends JFrame {

    public JScrollDemo() {
        Container container = getContentPane();

        //文本域
        JTextArea area = new JTextArea(20,50);
        area.setText("欢迎来到篮球世界");

        //Scroll面板
        JScrollPane scrollPane = new JScrollPane(area);
        container.add(scrollPane);

        this.setVisible(true);
        this.setBounds(100,100,550,550);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new JScrollDemo();
    }
}
~~~

结果图：

![image-20200710180858271](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710180858271.png)



#### 3.5、按钮

- ##### 图片按钮

~~~JAVA
package com.ren.lesson05;

import javax.swing.*;
import java.awt.*;
import java.net.URL;

public class JButtonDemo01 extends JFrame {

    public JButtonDemo01() {
        Container container = this.getContentPane();

        //将一个图片变成一个图标
        URL resource = JButtonDemo01.class.getResource("tx.jpg");
        Icon icon = new ImageIcon(resource);

        //把这个图标放在按钮上
        JButton button = new JButton();
        button.setIcon(icon);
        button.setToolTipText("图片按钮");

        container.add(button);

        this.setVisible(true);
        this.setSize(350,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);


    }

    public static void main(String[] args) {
        new JButtonDemo01();
    }
}
~~~

结果图：

![image-20200710184052112](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710184052112.png)

- ##### 单选按钮

~~~JAVA
package com.ren.lesson05;

import javax.swing.*;
import java.awt.*;
import java.net.URL;

public class JButtonDemo02 extends JFrame {

    public JButtonDemo02()  {
        Container container = this.getContentPane();

        //将一个图片变成一个图标
        URL resource = JButtonDemo02.class.getResource("tx.jpg");
        Icon icon = new ImageIcon(resource);

        //单选框
        JRadioButton radioButton1 = new JRadioButton("JRadioButton1");
        JRadioButton radioButton2 = new JRadioButton("JRadioButton2");
        JRadioButton radioButton3 = new JRadioButton("JRadioButton3");

        //由于单选框只能选择一个，所以得进行分组，一个组中只能选择一个
        ButtonGroup group = new ButtonGroup();
        group.add(radioButton1);
        group.add(radioButton2);
        group.add(radioButton3);

        container.add(radioButton1,BorderLayout.CENTER);
        container.add(radioButton2,BorderLayout.NORTH);
        container.add(radioButton3,BorderLayout.SOUTH);

        this.setVisible(true);
        this.setSize(350,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

    }

    public static void main(String[] args) {
        new JButtonDemo02();
    }
}
~~~

结果图：

![image-20200710185118242](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710185118242.png)

- ##### 复选按钮

~~~JAVA
package com.ren.lesson05;

import javax.swing.*;
import java.awt.*;
import java.net.URL;

public class JButtonDemo03 extends JFrame {

    public JButtonDemo03()  {
        Container container = this.getContentPane();

        //将一个图片变成一个图标
        URL resource = JButtonDemo02.class.getResource("tx.jpg");
        Icon icon = new ImageIcon(resource);

        //多选框
        JCheckBox checkBox1 = new JCheckBox("checkBox1");
        JCheckBox checkBox2 = new JCheckBox("checkBox2");

        container.add(checkBox1,BorderLayout.NORTH);
        container.add(checkBox2,BorderLayout.SOUTH);

        this.setVisible(true);
        this.setSize(350,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new JButtonDemo03();
    }
}
~~~

结果图：

![image-20200710192139293](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710192139293.png)

#### 3.6、列表

- ##### 下拉框

~~~JAVA
package com.ren.lesson06;

import javax.swing.*;
import java.awt.*;

public class TestComboboxDemo01 extends JFrame {

    public TestComboboxDemo01() {
        Container container = this.getContentPane();

        JComboBox status = new JComboBox();
        //获取下拉框里面的内容
        status.addItem(null);
        status.addItem("正在热映");
        status.addItem("已下架");
        status.addItem("正在上映");

        container.add(status);


        this.setVisible(true);
        this.setSize(350,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new TestComboboxDemo01();
    }
}
~~~

结果图：

![image-20200710192859045](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710192859045.png)

- ##### 列表框

~~~JAVA
package com.ren.lesson06;

import javax.swing.*;
import java.awt.*;
import java.util.Vector;

public class TestComboboxDemo02 extends JFrame {
    public TestComboboxDemo02() {
        Container container = this.getContentPane();

        //生成列表的内容(静态数据)
        //String[] contents = {"1","2","3"};
        Vector contents = new Vector();

        //列表中需要放入内容
        JList jList = new JList(contents);

        contents.add("zhangsan");
        contents.add("lisi");
        contents.add("wanwu");

        container.add(jList);


        this.setVisible(true);
        this.setSize(350,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new TestComboboxDemo02();
    }
}
~~~

结果图：

![image-20200710193818345](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710193818345.png)

- ##### 应用场景
  
  + 选择地区，或者一些单个选项
  + 列表，展示信息，一般是动态扩容！！！



#### 3.7、文本框

- ##### 文本框

~~~JAVA
package com.ren.lesson06;

import javax.swing.*;
import java.awt.*;
import java.util.Vector;

public class TestTextDemo01 extends JFrame {
    public TestTextDemo01() {
        Container container = this.getContentPane();

        JTextField textField = new JTextField("hello");
        JTextField textField2 = new JTextField("world",20);
        //因为采用东西南北中的布局方法所以设置的长度会被布局自动填充，想让其有效可以采用绝对布局

        container.add(textField,BorderLayout.NORTH);
        container.add(textField2,BorderLayout.SOUTH);

        this.setVisible(true);
        this.setSize(350,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new TestTextDemo01();
    }
}
~~~

结果图：

![image-20200710195602754](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710195602754.png)

- ##### 密码框

~~~JAVA
package com.ren.lesson06;

import javax.swing.*;
import java.awt.*;

public class TestTextDemo02 extends JFrame {
    public TestTextDemo02() {
        Container container = this.getContentPane();

        //一般还是配合面板操作 不在container中直接使用

        //默认布局是东西南北中
        JPasswordField passwordField = new JPasswordField();
        passwordField.setEchoChar('*');

        container.add(passwordField);

        this.setVisible(true);
        this.setSize(350,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new TestTextDemo02();
    }
}
~~~

结果图：

![image-20200710200113353](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710200113353.png)

- ##### 文本域

~~~JAVA
package com.ren.lesson05;

import javax.swing.*;
import java.awt.*;

public class JScrollDemo extends JFrame {

    public JScrollDemo() {
        Container container = getContentPane();

        //文本域
        JTextArea area = new JTextArea(20,50);
        area.setText("欢迎来到篮球世界");

        //Scroll面板
        JScrollPane scrollPane = new JScrollPane(area);
        container.add(scrollPane);

        this.setVisible(true);
        this.setBounds(100,100,550,550);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new JScrollDemo();
    }
}
~~~

结果图：

![image-20200710180858271](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200710180858271.png)







## 贪吃蛇

#### 帧： 如果时间片足够小，就是动画，一秒三十帧。连起来是动画，拆开就是静态的图片！

#### 键盘监听

#### 定时器Timer

### 思路：

1、先创建一个主类，创建一个窗口设置其相关数据；

2、创建一个游戏面板类，游戏界面一般都在面板上，最后创建一个数据类

3、在游戏面板类中先对该游戏的一些数据进行定义，如蛇、食物、成绩、蛇的走向、游戏状态以及定时器；在再其构造函数中调用一个初始化函数（是对该游戏的一些数据进行初始化），并且获得两个监听事件以及一个定时器的启动

4、<font color='red'>构建一个绘制面板</font>，在其中绘画一些需要的文字，图片，图标、蛇、游戏状态等

5、<font color='red'>重写一个键盘监听函数</font>，对于需要键盘操作的事件都对其进行监听，对于其中的一些操作还需要用到重新绘制函数

6、<font color='red'>重写一个事件监听函数</font>，通过状态来判断是否要进行接下来的操作；有吃食物、蛇的移动、走向、失败的判断、重画页面以及最后的定时器启动

<font color='red'>*重点：</font>对于每一块数据的操作，如先是写出静态的蛇、图片、背景等操作，在让其动起来时可以按以下步骤操作

- 定义数据
- 画到面板上去
- 监听事件
  - 键盘监听事件
  - 事件监听












