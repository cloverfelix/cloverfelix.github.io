# Http






## 网络编程

### 1.1、概述

![image-20200722084136388](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200722084136388.png)

#### 计算机网络：

是指将<font color='red'>地理位置不同</font>的具有独立功能的<font color='red'>多台计算机及其外部设备，通过通信线路连接起来</font>，在网络操作系统，网络管理软件及<font color='red'>网络通信协议下</font>，<font color='red'>实现资源共享</font>和信息传递的计算机系统。



#### 网络编程的目的：

无线电台...传播交流信息，数据交换，通信。



#### 想要达到这个效果需要什么：

  1、如何准确的定位网络上的一台主机   192.168.16.124：端口号，定位到这个计算机上的某个资源

  2、找到了这个主机，如何传输数据呢？



javaweb：网页编程  	B/S（架构）

网络编程：TCP/IP       C/S（架构）



### 1.2、网络通信的要素

#### 如何实现网络的通信？

通信双方地址：

- ip
- 端口号

#### 规则： 网络通信的协议

TCP/IP参考模型：

![image-20200722092347050](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200722092347050.png)



小结：

 	1. 网络编程中有两个主要的问题
     - 如何准确的定位到网络上的一台或者多台主机
     - 找到主机之后如何进行通信
	2. 网络编程中的要素
    - IP 和 端口号  ip的一些类
    - 网络通信的协议   UDP和TCP的一些类
	3. 万物皆对象



### 1.3、IP

ip地址：InetAddress

- 唯一定位一台网络上计算机

- 127.0.0.1：代表本机IP，英文名为localhost

- ip地址的分类

  - ipv4/ipv6
    - <font color='red'>IPV4 </font>== 127.0.0.1   由四个字节组成，每个字节长为0~255
    - <font color='red'>IPV6 </font>== fe80::141:5ede:c789:7a8a%12，128位，8个无符号整数！

  - 公网（互联网使用）和私网（局域网）
    - ABCD类地址（了解一下，面试可能会出这种题）
    - 192.168.xx.xx：专门给组织内部使用的

- 域名：记忆IP问题

  - IP：www.vip.com 

~~~JAVA
package com.ren.lesson01;

import java.net.InetAddress;
import java.net.UnknownHostException;

//测试IP
public class TestInetAddress {
    public static void main(String[] args) {
        try {
            //查询本机地址
            InetAddress inetAddress1 = InetAddress.getByName("127.0.0.1");
            System.out.println(inetAddress1);

            //查询网址IP地址
            InetAddress inetAddress2 = InetAddress.getByName("www.baidu.com");
            System.out.println(inetAddress2);

            InetAddress inetAddress3 = InetAddress.getByName("localhost");
            System.out.println(inetAddress3);
            InetAddress inetAddress4 = InetAddress.getLocalHost();
            System.out.println(inetAddress4);

            //常用方法
            System.out.println(inetAddress2.getAddress());
            System.out.println(inetAddress2.getCanonicalHostName());//获取规范的名字
            System.out.println(inetAddress2.getHostAddress());//获得主机名字
            System.out.println(inetAddress2.getHostName());//获取域名，或者自己电脑的名称

        } catch (UnknownHostException e) {
            e.printStackTrace();
        }
    }
}
~~~

结果图：

![image-20200730103218751](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200730103218751.png)



### 1.4、端口

端口表示计算机上的一个程序的进程；

- 不同的进程有不同的端口号！用来区分软件！

- 被规定0~65535

- TCP，UDP：65535*2是最终值   单个协议下，端口号不能冲突。如TCP使用80，UDP也是可以使用80的

- 端口分类

  - 公有端口0~1023
    - HTTP：80
    - HTTPS：443
    - FTP：21
    - SSH：22
    - Telent：23
  - 程序注册端口：1024~49151，分配给用户或者程序
    - Tomcat ：8080
    - MySQL：3306
    - Oracle：1521
  - 动态、私有端口：49152~65535

  ~~~JAVA
  netstat  -ano  # 查看所有的端口
  netstat  -ano|findstr "5900" #查看指定的端口
  tasklist|findstr "8696" #查看指定的端口的进程
  ~~~

  

~~~JAVA
package com.ren.lesson01;

import java.net.InetSocketAddress;

public class TestInetSocketAddress {
    public static void main(String[] args) {
        InetSocketAddress socketAddress = new InetSocketAddress("127.0.0.1", 8080);
        InetSocketAddress socketAddress2 = new InetSocketAddress("localhost", 8080);
        System.out.println(socketAddress);
        System.out.println(socketAddress2);

        System.out.println(socketAddress.getAddress());
        System.out.println(socketAddress.getHostName());//地址
        System.out.println(socketAddress.getPort());//端口
    }
}

~~~

结果图：

![image-20200730111804453](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200730111804453.png)



![image-20200730111822585](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200730111822585.png)



### 1.5、通信协议

协议：约定，就好比我们现在说的是普通话。

##### 网络通信协议 ：速率，传输码率，代码结构，传输控制....

问题：非常的复杂？

大事化小：分层！

#### TCP/IP协议簇

重要：

- TCP：用户传输协议
- UDP：用户数据报协议（相当于写信，传出去就不管了）

出名的协议：

- TCP：
- IP：网络互连协议





##### TCP UDP对比

TCP：比作打电话

- 连接，稳定
- 三次握手， 四次挥手

~~~JAVA
//其原理跟这个对话很相似，理解这个对话之间的联系

最少需要三次，保证稳定连接
A： 你愁啥？
B： 瞅你咋地？
A： 干一场！
    
A： 我要走了！
B： 你真的要走了吗？
B： 你真的真的要走了吗？
A： 我真的要走了！
~~~



- 客户端，服务端
- 传输完成，释放连接，效率低

UDP：比作发短信

- 不连接，不稳定
- 客户端，服务端：没有明确的界限
- 不管有没有准备好，都可以发给你...
- 类似于导弹
- DDOS：洪水攻击！ （饱和式攻击）



### 1.6、 TCP

客户端

​	1、连接服务器，通过socket

​    2、发送消息

~~~JAVA
package com.ren.lesson02;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;
import java.net.UnknownHostException;

//客户端
public class TcpClientDemo01 {
    public static void main(String[] args) {
        Socket socket = null;
        OutputStream os = null;

        try {
            //1.要知道服务器的地址
            InetAddress serverIP = InetAddress.getByName("127.0.0.1");
            int port = 9999;
            //2.创建一个socket连接
            socket = new Socket(serverIP, port);
            //3.发送消息 IO流
            os = socket.getOutputStream();
            os.write("欢迎来到篮球世界".getBytes());

        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            if(os!=null){
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if(socket!= null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}

~~~



服务器

​    1、建立服务的端口ServerSocket

​    2、等待用户的连接，通过accept，它会返回咋们客户端的socket

​    3、接受用户的消息

~~~JAVA
package com.ren.lesson02;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

//服务端
public class TcpServerDemo01 {
    public static void main(String[] args){
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        ByteArrayOutputStream baos = null;
        try {
            //1.我得有一个地址
            serverSocket = new ServerSocket(9999);
            //2.等待客户端连接过来
            socket = serverSocket.accept();
            //3. 读取客户的消息
            is = socket.getInputStream();

            /*
            * 基础不好时的写法
            *      byte[] buffer = new byte[1024];
            *      int len;
            *      while((len = is.read(buffer))!= -1){
            *           String msg = new String(buffer,0,len);
            *           System.out.println(msg);
            *       }
            * */

            //管道流
            baos = new ByteArrayOutputStream();
            byte[] buffer = new byte[1024];
            int len;
            while((len = is.read(buffer))!= -1){
                baos.write(buffer,0,len);
            }
            //将字节转化为字符
            System.out.println(baos.toString());


        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //关闭资源
            if(baos!= null){
                try {
                    baos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if(socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if(serverSocket!= null){
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

~~~





### 文件上传

服务器端

~~~JAVA
package com.ren.lesson02;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TcpServerDemo02 {
    public static void main(String[] args) throws Exception {
        //1.创建服务
        ServerSocket serverSocket = new ServerSocket(9000);
        //2.监听客户端的连接
        Socket socket = serverSocket.accept();//阻塞式监听，会一直等待客户端连接
        //3.获取输入流
        InputStream is = socket.getInputStream();

        //4.文件输出
        FileOutputStream fos = new FileOutputStream(new File("receive.jpg"));
        byte[] buffer = new byte[1024];
        int len;
        while((len=is.read(buffer))!=-1){
            fos.write(buffer,0,len);
        }

        //通知客户端我接受完毕了
        OutputStream os = socket.getOutputStream();
        os.write("我接收完毕了，你可以断开了".getBytes());

        //5.关闭资源
        os.close();
        fos.close();
        is.close();
        socket.close();
        serverSocket.close();
    }
}

~~~

客户端

~~~JAVA
package com.ren.lesson02;

import java.io.*;
import java.net.InetAddress;
import java.net.Socket;

public class TcpClientDemo02 {
    public static void main(String[] args) throws Exception {
        //1.创建一个socket连接
        Socket socket = new Socket(InetAddress.getByName("127.0.0.1"),9000);
        //2.创建一个输出流
        OutputStream os = socket.getOutputStream();
        //3.读取文件
        FileInputStream fis = new FileInputStream(new File("tx.jpg"));
        //4.写出文件
        byte[] buffer = new byte[1024];
        int len;
        while((len=fis.read(buffer))!=-1){
            os.write(buffer,0,len);
        }

        //通知服务器，我已经结束了
        socket.shutdownOutput();//我已经传输完了

        //确定服务器接收完毕，才能断开连接
        InputStream inputStream = socket.getInputStream();
        //String byte[]  管道流
        ByteArrayOutputStream baos = new ByteArrayOutputStream();

        byte[] buffer2 = new byte[1024];
        int len2;
        while ((len2=inputStream.read(buffer2))!=-1){
            baos.write(buffer2,0,len2);
        }
        System.out.println(baos.toString());

        //5.关闭资源
        baos.close();
        inputStream.close();
        fis.close();
        os.close();
        socket.close();
    }
}

~~~

结果图

![image-20200730174847189](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200730174847189.png)



### Tomcat

服务端

- 自定义 S
- Tomcat服务器 S

客户端

- 自定义 C
- 浏览器 B

<font color='red'>所以这就是常说的C/S，B/S</font>



### 1.7 、UDP

相当于发短信：不用连接，需要对方的地址！

客户端

~~~JAVA
package com.ren.lesson03;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

//不需要连接服务器
public class UdpClientDemo01 {
    public static void main(String[] args) throws Exception {
        //1.建立一个Socket
        DatagramSocket socket = new DatagramSocket();

        //2.建个包
        String msg = "你好啊，篮球";
        InetAddress localhost = InetAddress.getByName("localhost");
        int port = 9191;

        //发送的数据，数据的起始长度，要发送给谁
        DatagramPacket packet = new DatagramPacket(msg.getBytes(),0,msg.getBytes().length,localhost,port);

        //3.发送包
        socket.send(packet);

        //4.关闭数据
        socket.close();
    }
}

~~~

服务端

~~~JAVA
package com.ren.lesson03;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

//还是要等待客户端的连接
public class UdpServerDemo01 {
    public static void main(String[] args) throws Exception {
        //开放端口
        DatagramSocket socket = new DatagramSocket(9191);
        //接受数据包
        byte[] buffer = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length);//接收

        socket.receive(packet);//阻塞接收

        System.out.println(packet.getAddress().getHostAddress());
        System.out.println(new String(packet.getData(),0,packet.getLength()));

        //关闭资源
        socket.close();
    }
}

~~~

结果图：

![image-20200731143226768](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200731143226768.png)



#### 循环发送消息

~~~JAVA
package com.ren.chat;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetSocketAddress;
import java.net.SocketException;

public class UdpSenderDemo01 {
    public static void main(String[] args) throws Exception {

        DatagramSocket socket = new DatagramSocket(8888);

        //准备数据：控制台读取  System.in
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        while (true){
            //读取控制台这一行的数据
            String data = reader.readLine();
            //将读取的数据转为字节
            byte[] datas = data.getBytes();

            DatagramPacket packet = new DatagramPacket(datas, 0, datas.length, new InetSocketAddress("localhost", 6666));

            socket.send(packet);

            if (data.equals("bye")){
                break;
            }
        }


        socket.close();
    }
}

~~~



#### 循环接收消息

~~~JAVA
package com.ren.chat;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

public class UdpReceiveDemo01 {
    public static void main(String[] args) throws Exception {

        DatagramSocket socket = new DatagramSocket(6666);

        while (true){
            //准备接受包裹
            byte[] container = new byte[1024];
            DatagramPacket packet = new DatagramPacket(container, 0, container.length);
            socket.receive(packet);//阻塞式接受包裹

            //断开连接
            byte[] data = packet.getData();
            String receiverData = new String(data, 0, data.length);

            System.out.println(receiverData);

            if(receiverData.equals("bye")){
                break;
            }
        }

        socket.close();
    }
}

~~~



结果图：

![image-20200731145616978](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200731145616978.png)

![image-20200731145629395](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200731145629395.png)





#### 在线咨询：两个人都可以是发送方，也都是接收方！

发送端：<font color='red'>采用了线程方法</font>

~~~JAVA
package com.ren.chat;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetSocketAddress;

public class TalkSend implements Runnable {

    private int fromPort;
    private String toIP;
    private int toPort;

    DatagramSocket socket = null;
    BufferedReader reader = null;

    public TalkSend(int fromPort, String toIP, int toPort) {
        this.fromPort = fromPort;
        this.toIP = toIP;
        this.toPort = toPort;

        try{
            socket = new DatagramSocket(fromPort);
            //准备数据：控制台读取  System.in
            reader = new BufferedReader(new InputStreamReader(System.in));
        }catch (Exception e){
            e.printStackTrace();
        }

    }

    @Override
    public void run() {

        while (true){
            try{
                //读取控制台这一行的数据
                String data = reader.readLine();
                //将读取的数据转为字节
                byte[] datas = data.getBytes();

                DatagramPacket packet = new DatagramPacket(datas, 0, datas.length, new InetSocketAddress(toIP, toPort));

                socket.send(packet);

                if (data.equals("bye")){
                    break;
                }
            }catch (Exception e){
                e.printStackTrace();
            }
        }


        socket.close();
    }
}

~~~

接收端：

~~~JAVA
package com.ren.chat;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class TalkReceive implements Runnable {

    DatagramSocket socket = null;

    private int port;
    private String msgFrom;

    public TalkReceive(int port,String msgFrom) {
        this.port = port;
        this.msgFrom = msgFrom;

        try{
            socket = new DatagramSocket(port);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    @Override
    public void run() {

        while (true){
            try{
                //准备接受包裹
                byte[] container = new byte[1024];
                DatagramPacket packet = new DatagramPacket(container, 0, container.length);
                socket.receive(packet);//阻塞式接受包裹

                //断开连接
                byte[] data = packet.getData();
                String receiverData = new String(data, 0, data.length);

                System.out.println(msgFrom+":"+receiverData);

                if(receiverData.equals("bye")){
                    break;
                }
            }catch (Exception e){
                e.printStackTrace();
            }
        }

        socket.close();
    }
}

~~~

学生端：

~~~JAVA
package com.ren.chat;

public class TalkStudent {

    public static void main(String[] args) {
        //开启两个线程
        new Thread(new TalkSend(7777,"localhost",9999)).start();
        new Thread(new TalkReceive(8888,"老师")).start();
    }
}

~~~

老师端：

~~~JAVA
package com.ren.chat;

public class TalkTeacher {
    public static void main(String[] args) {
        //开启两个线程
        new Thread(new TalkSend(5555,"localhost",8888)).start();
        new Thread(new TalkReceive(9999,"学生")).start();
    }
}

~~~

结果图：

![image-20200731154157043](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200731154157043.png)

![image-20200731154216219](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200731154216219.png)



### 1.8、URL

统一资源定位符：定位资源的，定位互联网上的某一个资源

DNS叫域名解析  相当于把  www.baidu.com  解析成 xxx.xxx.xx.xx 的一个IP

~~~
协议：//ip地址：端口号/项目名/资源
~~~


