# Netty


# 1、Netty的介绍

1、Netty是由JBOSS提供的一个Java开源框架，现为Github上的独立项目

2、Netty是一个`异步的`、`基于事件驱动`的`网络`应用框架，用以快速开发高性能、高可靠性的网络IO程序
-	基于事件驱动：比如一个网页中有一个按钮，当你点击时会触发一个事件，这个事件就会去调用一个函数

3、Netty主要针对在`TCP协议`下，面向Clients端的高并发应用，或者Peer-to-Peer场景下的大量数据持续传输的应用。

4、Netty本质是一个`NIO框架`，适用于服务器通讯相关的多种应用场景

5、要透彻理解Netty，需要先学习NIO，这样才能阅读Netty源码

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210906172744.png)

# 2、BIO

## 2.1、I/O模型基本说明

1. I/O模型简单理解：就是用什么样的通道进行数据的发送和接收，很大程度上决定了程序通信的性能
2. Java共支持3种网络编程模型I/O模型：BIO、NIO、AIO
3. Java BIO：同步并阻塞(**传统阻塞型**)，服务器实现模式为一个连接对应一个线程，即客户端有连接请求时服务端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210906205501.png)

4. Java NIO：**同步非阻塞**，服务器实现模式为一个线程处理多个请求(连接)，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求就进行处理
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210906205426.png)

5.Java AIO(NIO.2)：异步非阻塞，AIO引入异步通道的概念，采用了Proactor模式，简化了程序编写，有效的请求才启动线程，它的特点是先由操作系统完成后才通知服务端程序启动线程去处理，一般适用于连接数较多且连接时间较长的应用 

## 2.2、I/O模型适用场景

1. BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前唯一选择，但是程序简单易理解。
2. NIO方式适用于**连接数目多且连接比较短**(轻操作)的架构，比如聊天服务器，弹幕系统，服务器间通讯等。编程比较复杂，JDK1.4开始支持。
3. AIO方式适用于**连接数目多且连接比较长**(重操作)的架构，比如相册服务器，充分调用操作系统参与并发操作，编程比较复杂，JDK7开始支持。

## 2.3、BIO基本介绍

1. Java BIO就是**传统的java io编程**，其相关的类和接口再java.io
2. BIO(blocking I/O)：**同步阻塞**，服务器实现模式为一个连接对应一个线程，即客户端有连接请求时服务端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，可以通过**线程池机制**改善(实现多个客户连接服务器)

## 2.4、BIO工作机制

1. 服务器端启动一个ServerSocket
2. 客户端启动Socket对服务器进行通信，默认情况下服务器端需要对每个客户建立一个线程与之通信
3. 客户端发出请求后，先咨询服务器是否有线程响应，如果没有则等待，或者被拒绝
4. 如果有响应，客户端线程会等待请求结束后，再继续执行

## 2.5、BIO应用实例

**实例说明**

1. 使用BIO模型编写一个服务器端，监听6666端口，当有客户端连接时，就启动一个线程与之通讯
2. 要求使用线程池机制改善，可以连接多个客户端
3. 服务器端可以接受客户端发送的数据(`telnet`方式即可)

~~~java 
package com.clover.bio;

import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class BIOServer {
    public static void main(String[] args) throws IOException {
        // 线程池机制

        //思路
        //1.创建一个线程池
        //2.如果有客户端连接，就创建一个线程，与之通信(单独写一个方法)

        ExecutorService newCachedThreadPool = Executors.newCachedThreadPool();

        // 创建ServerSocket
        ServerSocket serverSocket = new ServerSocket(6666);

        System.out.println("服务器启动了");

        while(true){
            // 监听，等待客户端连接
            System.out.println("线程信息 id =" + Thread.currentThread().getId() + "名字=" + Thread.currentThread().getName());
            System.out.println("等待连接.........");
            final Socket socket = serverSocket.accept();
            System.out.println("连接到一个客户端");

            // 连接到一个客户端就创建一个线程，与之通信(单独写一个方法)
            newCachedThreadPool.execute(new Runnable() {
                // 我们可以重写这个方法
                @Override
                public void run() {
                    // 可以和客户端通讯
                    handler(socket);
                }
            });
        }
    }

    // 编写一个handler方法和客户端通讯
    public static void handler(Socket socket){

        try {
            System.out.println("线程信息 id =" + Thread.currentThread().getId() + "名字=" + Thread.currentThread().getName());
            byte[] bytes = new byte[1024];

            // 通过socket获取输入流
            InputStream inputStream = socket.getInputStream();

            // 循环读取客户端发送的数据
            while(true){
                System.out.println("线程信息 id =" + Thread.currentThread().getId() + "名字=" + Thread.currentThread().getName());

                System.out.println("read.........");
                int read = inputStream.read(bytes);
                if(read != -1){
                    // 不等于-1说明还未读完，还可以继续读取
                    // 输出客户端发送的数据
                    System.out.println(new java.lang.String(bytes,0,read));
                }else{
                    break;
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            System.out.println("关闭和client的连接");
            try {
                socket.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

}
~~~

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210907183049.png)

还可以测试每一次客户端连接后对应的线程是否与它通讯的线程是否一样

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210907183754.png)

这个代码中的`accept和read`函数会产生阻塞情况

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210907184009.png)

连接过后

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210907184240.png)

有连接的就会开在`read`处，但是当你发送数据后又会阻塞再read处，这就会使性能很低

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210907184527.png)

## 2.6、Java BIO问题分析

1. 每个请求都需要创建独立的线程，与对应的客户端进行数据Read，业务处理，数据Write
2. 当并发数较大时，**需要创建大量线程来处理连接**，系统资源占用比较大
3. 连接建立后，如果当前线程暂时没有数据可读，则线程就会阻塞再Read操作上，造成线程资源浪费

# 3、NIO

## 3.1、Java NIO基本介绍

1. Java NIO全称Java non-blocking IO，是指JDK提供的新API。从JDK1.4开始，Java提供了一系列改进的输入/输出的新特性，被称为NIO(即New IO)，是**同步非阻塞**的。
2. NIO相关类都被放在Java.nio包及子包下，并且对原java.io包中的很多类进行改写。
3. NIO有三大核心部分：**Channel(通道)**，**Buffer(缓冲区)**，**Selector(选择器)**
4. NIO是面向**缓冲区，或者面向块**编程的。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动，这就增加了处理过程中的灵活性，使用它可以提供**非阻塞式**的高伸缩性网络。
5. Java NIO的非阻塞模式，使一个线程从某通道发送请求或者读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取，而**不是保持线程阻塞**，所以直至数据变的可以读取之前，该线程可以继续做其它的事情。非阻塞写也是如此，一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情
6. 通俗理解：NIO时可以做到用一个线程来处理多个操作的。假设有10000个请求过来，根据实际情况，可以分配50或者100个线程来处理。不像之前的阻塞IO那样，非得分配10000个。
7. HTTP2.0使用了**多路复用技术**，做到同一个连接并发处理多个请求，而且并发请求的数量比HTTP1.1大了好几个数量级。

## 3.2、NIO和BIO的比较

1. BIO是以流的方式处理数据，而NIO以块的方式处理数据，块I/O的效率比流I/O高很多
2. BIO是阻塞的，NIO则是非阻塞的
3. BIO基于字节流和字符流进行操作，而NIO基于**Channel(通道)** 和 **Buffer(缓冲区)** 进行操作，`数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中`。Selector(选择器)用于监听多个通道的事件(比如；连接请求，数据到达等)，因此使用**单个线程就可以监听多个客户端**通道

## 3.3、NIO三大核心原理示意图

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210908151106.png)

**Selector、Channel和Buffer的关系图说明**

1. 每个Channel都会对应一个Buffer
2. 每个Selector都对应一个线程，一个线程对应多个Channel(连接)
3. 该图反应了有三个Channel注册到了该Selector(使用程序解释)
4. 程序切换到哪个Channel是由事件决定的，Event就是一个重要的概念
5. Selector会根据不同的事件，在各个通道上切换
6. Buffer就是一个内存块，底层是有一个数组
7. 数据的读取写入是通过Buffer，这个和BIO有本质的区别，BIO中要么是输入流，或者是输出流，不能双向，但是NIO的Buffer是可以读也可以写，需要flip方法切换
8. Channel是双向的，可以返回底层操作系统的情况，比如Linux，底层的操作系统通道就是双向的

## 3.4、缓冲区(Buffer)

### 3.4.1、基本介绍

缓冲区(Buffer)：缓冲区本质上是一个**可以读写数据的内存块**，可以理解成是一个**容器对象(含数组)**，该对象提供了**一组方法**，可以更轻松地使用内存块，缓冲区对象内置了一些机制，能够跟踪和记录缓冲区的状态变化情况。Channel提供从文件、网络读取数据的渠道，但是读取或写入的数据都必须经由Buffer。如图：(后面举例说明)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210908162426.png)

### 3.4.2、Buffer类及其子类

1. 在NIO中，Buffer是一个顶层父类，它是一个抽象类，类的层级关系图

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210908165147.png)

2. Buffer类定义了所有的缓冲区都具有的四个属性来提供关于其所包含的数据元素的信息 

- **Capacity**：容量，即可以容纳的最大数据量；在缓冲区创建的时候被设定并且不能改变
- **Limit**：表示缓冲区的当前终点，不能对缓冲区超过极限的位置进行读写操作。且极限是可以修改的
- **Position**：位置，下一个要被读或写的元素的索引，每次读写缓冲区数据时都会改变值，为下次读写做准备
- **Mark**：标记

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210908165856.png)

3. Buffer类相关方法

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210908170155.png) 

### 3.4.3、ByteBuffer

从前面可以看出对于Java中的基本数据类型(boolean除外)，都有一个Buffer类型与之相对应，**最常用**的自然是ByteBuffer类(二进制数据)，该类的主要方法如下

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210908170707.png)

## 3.5、通道(Channel)

### 3.5.1、基本介绍

1. NIO的通道类似于流，但是有些区别如下
	- 通道可以同时进行读写，而流只能读或者写
	- 通道可以实现异步读写数据
	- 通道可以从缓冲区读数据，也可以写数据到缓冲区中

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210908172448.png)

2. BIO中的Stream是单向的，例如FileInputStream对象只能进行读取数据的操作，而NIO中的通道(Channel)是双向的，可以读操作，也可以写操作
3. Channel在NIO中是一个接口`public interface Channel extends Closeable{}`
4. 常用的Channel类有：FileChannel、DatagramChannel、**ServerSocketChannel**和**SocketChannel**
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210908173140.png)
	- 这里需要注意**ServerSocketChannel**和**SocketChannel**两个类
	- 当有一个连接产生的时候，我们Server中的`ServerSocketChannel`会产生一个与该客户端对应的通道，通道类型为`SocketChannel`，而其真实的实现类型为`SockerChannelImpl`，然后再通过该通道与服务器进行通讯
5. FileChannel用于文件的数据读写，DatagramChannel用于UDP的数据读写，ServerSocketChannel和SocketChannel用于TCP的数据读写

### 3.5.2、FileChannel类

FileChannel主要用来对本地文件进行IO操作，常见的方法有 

1. `public int read (ByteBuffer dst)` ，从通道读取数据并放入缓冲区中
2. `public int write(ByteBuffer src)` ，把缓冲区的数据写到通道中
3. `public long transferFrom(ReadableByteChannel src,long position,long count)` , 从目标通道中复制数据到当前通道
4. `public long transferTo(long position,long count,WriteableByteChannel target)` , 把数据从当前通道复制给目标通道 

### 3.5.3、应用实例1-本地文件写数据

实例要求：
1. 使用前面学习后的ByteBuffer(缓冲)和FileChannel(通道)，将"hello,韋"写入到file01.txt中
2. 文件不存在就创建 **(一个汉字3个字节)**
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909091517.png)
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909091823.png)

~~~Java
package com.clover.nio;

import java.io.FileOutputStream;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

public class NIOFileChannel01 {
  public static void main(String[] args) throws Exception{
      String str = "hello,韋";
      //创建一个输出流->然后获取内置channel
      FileOutputStream fileOutputStream = new FileOutputStream("H:\\file01.txt");

      //通过FileOutputStream获取对应的FileChannel
      //这个FileChannel真实类型为FileChannelImpl
      FileChannel fileChannel = fileOutputStream.getChannel();

      //创建一个缓冲区 ByteBuffer
      ByteBuffer byteBuffer = ByteBuffer.allocate(1024);

      //将数据放入缓冲区中
      byteBuffer.put(str.getBytes());

      //数据写入完毕后要将buffer反转
      byteBuffer.flip();

      // 将缓冲区数据写入fileChannel
      fileChannel.write(byteBuffer);
      //因为fileChannel中封装了Java的输出流，最终写入文件中也是通过流写入，所以最后需要关闭
      fileOutputStream.close();
  }
}
~~~

### 3.5.4、应用实例2-本地文件读写数据

实例要求：
1. 使用前面学习后的ByteBuffer(缓冲)和FileChannel(通道)，将file01.txt中的数据读入到程序，并显示再控制台屏幕
2. 假定文件已存在
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909093607.png)
~~~Java
package com.clover.nio;

import java.io.File;
import java.io.FileInputStream;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

public class NIOFileChannel02 {
  public static void main(String[] args) throws Exception {
      //创建一个文件的输入流->然后获取内置channel
      File file = new File("H:\\file01.txt");
      FileInputStream fileInputStream = new FileInputStream(file);

      //通过fileInputStream获取对应的channel
      FileChannel fileChannel = fileInputStream.getChannel();

      //创建一个缓冲区
      ByteBuffer byteBuffer = ByteBuffer.allocate((int) file.length());

      //将通道中的数据向缓冲区中写入
      fileChannel.read(byteBuffer);

      //将byteBuffer的字节数据转成String
      System.out.println(new String(byteBuffer.array()));

      fileInputStream.close();
  }
}
~~~

### 3.5.5、应用实例3-使用一个Buffer完成文件读取、写入

实例要求：
1. 使用FIleChannel(通道)和方法read,write，完成文件的拷贝
2. 拷贝一个文本文件 1.txt，放在项目下即可
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909094437.png)
~~~java 
package com.clover.nio;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

public class NIOFileChannel03 {
  public static void main(String[] args) throws Exception {
      // 创建一个文件
      File file = new File("H:\\1.txt");
      FileInputStream fileInputStream = new FileInputStream(file);
      // 通过文件输入流获取channel
      FileChannel fileInputStreamChannel = fileInputStream.getChannel();

      // 创建一个输出流，读取数据
      FileOutputStream fileOutputStream = new FileOutputStream("H:\\2.txt");
      // 获取对应的channel
      FileChannel fileOutputStreamChannel = fileOutputStream.getChannel();

      // 创建一个缓冲区
      ByteBuffer byteBuffer = ByteBuffer.allocate((int) file.length());

      while (true){
          // 这里有一个重要操作，一定不能忘记
          byteBuffer.clear(); // 清空buffer
          // 将通道数据写入缓冲区
          int read = fileInputStreamChannel.read(byteBuffer);
          if (read == -1){
              break;
          }
          // buffer写入完数据后要反转为后面从buffer读取数据做准备
          byteBuffer.flip();
          // 将缓冲区数据读取到通道中
          fileOutputStreamChannel.write(byteBuffer);
      }

      // 关闭相关的流
      fileInputStream.close();
      fileOutputStream.close();
  }
}
~~~

### 3.5.6、应用实例4-拷贝文件transferFrom方法

实例要求：
1. 使用FileChannel(通道)和方法 transferFrom，完成文件的拷贝
2. 拷贝一张图片
~~~Java
package com.clover.nio;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.nio.channels.FileChannel;

public class NIOFileChannel04 {
  public static void main(String[] args) throws Exception {
      // 创建相关的流
      FileInputStream fileInputStream = new FileInputStream("H:\\1.jpg");
      FileOutputStream fileOutputStream = new FileOutputStream("H:\\2.jpg");

      // 获取相关流所对应的通道
      FileChannel fileInputStreamChannel = fileInputStream.getChannel();
      FileChannel fileOutputStreamChannel = fileOutputStream.getChannel();

      // 将文件从目标通道拷贝到当前通道
      fileOutputStreamChannel.transferFrom(fileInputStreamChannel,0,fileInputStreamChannel.size());

      // 关闭相关的通道和流
      fileInputStreamChannel.close();
      fileOutputStreamChannel.close();
      fileInputStream.close();
      fileOutputStream.close();
  }
}
~~~

### 3.5.7、关于Buffer和Channel的注意事项和细节

1. ByteBuffer支持类型化的put和get，put放入的是什么数据类型，get就应该使用响应的数据类型来取出，否则可能有BufferUnderflowException异常
~~~Java
package com.clover.nio;

import java.nio.ByteBuffer;

public class NIOByteBufferPutGet {
  public static void main(String[] args) {
      // 创建一个Buffer
      ByteBuffer buffer = ByteBuffer.allocate(64);

      //类型化方式放入数据
      buffer.putInt(824);
      buffer.putLong(8);
      buffer.putChar('韋');
      buffer.putShort((short) 4);

      // 取出数据
      buffer.flip();

      System.out.println(buffer.getInt());
      System.out.println(buffer.getLong());
      System.out.println(buffer.getChar());
      System.out.println(buffer.getShort());
  }
}
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909111514.png)

2. 可以将一个普通的Buffer转成只读Buffer
~~~Java
package com.clover.nio;

import java.nio.ByteBuffer;

public class ReadOnlyBuffer {
  public static void main(String[] args) {
      // 创建一个Buffer
      ByteBuffer buffer = ByteBuffer.allocate(64);
      for (int i= 0; i < 64; i++){
          buffer.put((byte) i);
      }

      //读取
      buffer.flip();

      // 得到一个只读的Buffer
      ByteBuffer readOnlyBuffer = buffer.asReadOnlyBuffer();
      System.out.println(readOnlyBuffer.getClass());

      while (readOnlyBuffer.hasRemaining()){
          // 这个方法会让我们的position自动加1
          System.out.println(readOnlyBuffer.get());
      }

      readOnlyBuffer.put((byte) 824); // 用只读的Buffer进行写入操作会抛出ReadOnlyBufferException
  }
}
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909112807.png)

3. NIO还提供了MappedByteBuffer，可以让文件直接在内存(堆外的内存)中进行修改，而如何同步到文件由NIO来完成
~~~Java
package com.clover.nio;

import java.io.RandomAccessFile;
import java.nio.MappedByteBuffer;
import java.nio.channels.FileChannel;

/*
 * 说明
 * 1.MappedByteBuffer 可以让文件直接在内存(堆外内存)中修改，操作系统不需要拷贝一次
 */

public class MappedByteBufferTest {
  public static void main(String[] args) throws Exception {
      RandomAccessFile randomAccessFile = new RandomAccessFile("H:\\1.txt", "rw");
      //获取对应的通道
      FileChannel channel = randomAccessFile.getChannel();

      /*
       * 参数1：FileChannel.MapMode.READ_WRITE 使用的读写模式
       * 参数2：0 ：可以直接修改的起始位置
       * 参数3：5 ：是映射到内存的大小(不是索引位置)，即将 1.txt的多少个字节映射到内存
       * 可以直接修改的范围就是0-5
       * MappedByteBuffer 实际类型 DirectByteBuffer
       */
      MappedByteBuffer mappedByteBuffer = channel.map(FileChannel.MapMode.READ_WRITE, 0, 5);

      mappedByteBuffer.put(0, (byte) 'H');
      mappedByteBuffer.put(3, (byte) '8');
      mappedByteBuffer.put(5, (byte) '8');  //这个地方的5并不是代表索引，而是代表可以修改字节数的大小，超过5会抛出异常IndexOutOfBoundsException

      randomAccessFile.close() ;

  }
}
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909120125.png)

4. 前面我们讲的读写操作，都是通过一个Buffer完成的，**NIO还支持通过多个Buffer(即Buffer数组)完成读写操作**，即Scattering和Gatering
~~~Java
package com.clover.nio;

import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Arrays;

/*
 * Scattering：将数据写入到buffer时，可以采用buffer数组，依次写入  [分散]
 * Gathering：从buffer读取数据时，可以采用buffer数组，依次读取
 */
public class ScatteringAndGatheringTest {
  public static void main(String[] args) throws Exception {
      // 使用ServerSocketChannel 和 SocketChannel  这就需要网络了
      ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
      InetSocketAddress inetSocketAddress = new InetSocketAddress(7000);

      // 绑定端口到socket，并启动
      serverSocketChannel.socket().bind(inetSocketAddress);

      // 创建buffer数组
      ByteBuffer[] byteBuffers = new ByteBuffer[2];
      byteBuffers[0] = ByteBuffer.allocate(5);
      byteBuffers[1] = ByteBuffer.allocate(3);

      // 等待客户端连接(telnet)
      SocketChannel socketChannel = serverSocketChannel.accept();

      int messageLength = 8;   // 假设我们从客户端最多接受8个字节

      // 循环读取
      while (true){
          // 统计读取了多少个字节
          int byteRead = 0;
          while (byteRead < messageLength){
              long l = socketChannel.read(byteBuffers);
              byteRead += l;// 累计读取的字节数
              System.out.println("byteRead =" + byteRead);
              // 使用流打印，查看当前buffer的position和limit
              Arrays.asList(byteBuffers).stream().map(buffer -> "position=" + buffer.position() + ",limit=" + buffer.limit()).forEach(System.out::println);
          }

          // 将所有的buffer进行一个反转flip
          Arrays.asList(byteBuffers).forEach(buffer -> buffer.flip());

          // 将数据读取出来显示在控制台
          long byteWrite = 0;// 查看显示在控制台字节大小
          while (byteWrite < messageLength){
              long w = socketChannel.write(byteBuffers);
              byteWrite += w;
          }

          // 将所有的buffer进行clear
          Arrays.asList(byteBuffers).forEach(buffer -> {
              buffer.clear();
          });

          System.out.println("byteRead=" + byteRead + ",byteWrite=" + byteWrite + ",messageLength=" + messageLength);
      }
  }
}
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909171320.png)
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909171205.png)

## 3.6、Selector(选择器)

### 3.6.1、基本介绍
1. Java的NIO，用非阻塞的IO方式。可以用一个线程，处理多个的客户端连接，就会使用到**Selector(选择器)**
2. **Selector能够检测到多个注册的通道上是否有事件发生(注意：多个Channel以事件的方式可以注册到同一个Selector)**，如果有事件发生，变获取事件然后针对每个事件进行相应的处理。这样就可以只用一个单线程去管理多个通道，也就是管理多个连接和请求。
3. 只有在通道真正有读写事件发生时，才会进行读写，就大大的减少了系统开销，并且不必为每个连接都创建一个线程，不用去维护多个线程
4. 避免了多线程之间的上下文切换导致的开销

### 3.6.2、Selector示意图和特点说明
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909174806.png)

**特点再说明：**
1. Netty的IO线程NioEventLoop聚合了Selector(选择器，也叫多路复用器),可以同时并发处理成百上千个客户端的连接
2. 当线程从某客户端Socket通道进行读写数据时，若没有数据可读写时，该线程可以进行其它任务。
3. 线程通常将非阻塞IO的空闲事件用于在其它通道上执行IO操作，所以单独的线程可以管理多个输入和输出通道
4. 由于读写操作都是非阻塞的，这就可以充分提示IO线程的运行效率，避免由于频繁IO阻塞导致的线程挂起
5. 一个IO线程可以并发处理N个客户端连接和读写操作，这从根本上解决了传统同步阻塞IO，一个连接一个线程模型，架构的性能、弹性伸缩能力和可靠性都得到了极大的提升。

### 3.6.3、Selector类相关的方法

Selector类是一个抽象类，常用方法和说明如下：

~~~Java
public abstract class Selector implements Closeable{
		public static Selector open(); // 得到一个选择器对象
		public int select(long timeout); // 监控所有注册的通道，当其中有IO操作可以进行时，将对应的SelectionKey加入到内部集合中并返回，参数用来设置超时时间
		public Set<SelectionKey> selectedKeys(); // 从内部集合中得到所有的SelectionKey
}
~~~

	注意事项
	1. NIO中的ServerSocketChannel功能类似ServerSocket，SocketChannel功能类似Socket
	2. Selector相关方法说明
		   selector.select(); //阻塞
		   selector.select(1000); // 阻塞1000毫秒，在1000毫秒后返回
		   selector.wakeup(); // 唤醒selector
		   selector.selectNow(); // 不阻塞，立马返还

 ## 3.7、NIO非阻塞网络编程原理分析图
 NIO非阻塞网络编程相关的(Selector、SelectionKey、ServerSocketChannel和SocketChannel)关系梳理图
 ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210909210151.png)
**对上图说明：**
1. 当客户端连接时，会通过`ServerSocketChannel`得到`SocketChannel`
2. Selector进行监听，使用`select`方法，可以使用阻塞的也可以使用非阻塞的，返回有事件发生的通道的个数
3. 将`SocketChannel`注册到Selector上，使用`register(Selector sel,int ops)`，一个selector上可以注册多个`SocketChannel`
4. 注册后返回一个`SelectionKey`，会和该Selector关联`该key被Selector管理起来，做成一个集合的形式`
5. 进一步得到各个SelectionKey(有事件发生)
6. 再通过SelectionKey反向获取`SocketChannel`，通过channel()方法
7. 可以通过得到的`channel`，完成业务处理

## 3.8、NIO非阻塞网络编程快速入门

案例要求：
1. 编写一个NIO入门案例，实现服务器端和客户端之间的数据简单通讯(非阻塞)
2. 目的：理解NIO非阻塞网络编程机制

**服务器端：**
~~~Java
package com.clover.nio;

import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.util.Iterator;
import java.util.Set;

public class NIOServer {
  public static void main(String[] args) throws Exception{
      // 创建ServerSocketChannel -> ServerSocket
      ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();

      // 得到一个Selector对象
      Selector selector = Selector.open();

      // 绑定一个端口6666，在服务器端监听
      serverSocketChannel.socket().bind(new InetSocketAddress(6666));

      // 设置为非阻塞
      serverSocketChannel.configureBlocking(false);

      // 把 serverSocketChannel 注册到 selector 关心事件为OP_ACCEPT
      serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

      // 循环等待客户端连接
      while (true){
          // 这里我们等待1秒，如果没有事件发生，就返回
          if (selector.select(1000) == 0){
              // 没有事件发生
              System.out.println("服务器等待了1秒，无连接");
              continue;
          }

          // 如果返回的 >0,就获取到相关的selectionKey集合
          // 1.如果返回的 >0,表示已经获取到关注的事件
          // 2.selector.selectedKeys() 返回关注事件的集合
          // 3.通过 selectionKeys 反向获取通道
          Set<SelectionKey> selectionKeys = selector.selectedKeys();

          // 遍历 et<SelectionKey> ，使用迭代器遍历
          Iterator<SelectionKey> keyIterator = selectionKeys.iterator();

          while (keyIterator.hasNext()){
              // 获取到SelectionKey
              SelectionKey key = keyIterator.next();

              //根据key对应的通道发生的事件做相应的处理
              if (key.isAcceptable()){
                  // 如果是 OP_ACCEPT，有新的客户端连接
                  // 给该客户端生成一个ServerSocket
                  // 这里使用 accept 为什么不会产生阻塞？ 因为你原本的判断就已经判定了它时处于哪一个事件，自然不会在去等待
                  SocketChannel socketChannel = serverSocketChannel.accept();
                  System.out.println("客户端连接成功  生成了一个 socketChannel " + socketChannel.hashCode());
                  socketChannel.configureBlocking(false);
                  // 将 socketChannel 注册到selector，关注事件为 OP_READ，同时给该 socketChannel 关联一个Buffer
                  /*
                   * 这里需要注意：
                   *    如果你在将 socketChannel 注册到selector时，给其关联一个buffer分配空间过大时，在
                   *    后面使用输出语句进行输出时，没有字节填充时，后面会默认输出乱码
                   */
                  socketChannel.register(selector,SelectionKey.OP_READ, ByteBuffer.allocate(9));
              }

              if (key.isReadable()){
                  // 发生 OP_READ 事件
                  // 通过key反向获取到对应的channel
                  SocketChannel channel = (SocketChannel)key.channel();

                  // 获取到该Channel关联的Buffer
                  ByteBuffer buffer = (ByteBuffer)key.attachment();

                  // 将通道中的数据读取到Buffer
                  channel.read(buffer);
                  System.out.println("从客户端发送的数据" + new String(buffer.array()));
              }

              // 手动从集合中移除当前的 SelectionKey ，防止重复操作
              keyIterator.remove();
          }
      }
  }
}
~~~

**客户端：**
~~~Java
package com.clover.nio;

import jdk.nashorn.internal.ir.CallNode;

import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SocketChannel;

public class NIOClient {
  public static void main(String[] args) throws Exception{
      // 得到一个网络通道
      SocketChannel socketChannel = SocketChannel.open();
      // 设置非阻塞
      socketChannel.configureBlocking(false);
      // 提供服务器端的ip 和 端口
      InetSocketAddress inetSocketAddress = new InetSocketAddress("127.0.0.1", 6666);
      // 连接服务器
      if (!socketChannel.connect(inetSocketAddress)){
          while (!socketChannel.finishConnect()){
                System.out.println("因为连接需要事件，客户端不会阻塞，可以做其它工作");
          }
      }

      // 如果连接成功，就发送数据‘
      String str = "hello,韋";
      // 使用这种方式就不需要再去指定字节大小了，也不再浪费空间大小
      ByteBuffer buffer = ByteBuffer.wrap(str.getBytes());
      // 发送数据,将Buffer数据写入channel
      socketChannel.write(buffer);
      System.in.read();// 使代码停止在此
  }
}
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210910110433.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210910111115.png) 

## 3.9、SelectionKey
1. SelectionKey，表示**Selector和网络通道的注册关系**，共四种：
	-	int OP_ACCEPT：有新的网络连接可以accept，值为16
	-	int OP_CONNECT：代表连接已经建立，值为8
	-	int OP_READ：代表读写操作，值为1
	-	int OP_WRITE：代表写操作，值为4
2. 源码中
	-	public static final int OP_READ = 1 << 0;
	-	public static final int OP_WRITE = 1 << 2;
	-	public static final int OP_CONNECT = 1 << 3;
	-	public static final int OP_ACCEPT = 1 << 4;
3. SelectionKey相关方法

~~~Java
public abstract class SelectionKey {
		public abstract Selector selector();// 得到与之关联的Selector对象
		public abstract SelectableChannel channel();//得到与之关联的通道
		public final Object attachment();//得到与之关联的共享数据
		public abstract SelectionKey interestOps(int ops);//设置或改变监听事件，比如：原本是监听连接事件，你可以将其改为监听读事件，这个根据需求来定
		public final boolean isAcceptable();//是否可以accept
		public final boolean isReadable();//是否可以读
		public final boolean isWriteable();//是否可以写
}
~~~

## 3.10、ServerSocketChannel

1. ServerSocketChannel在**服务器端监听新的客户端Socket连接，产生一个SocketChannel**
2. 相关方法

~~~Java
public abstract class ServerSocketChannel extends AbstractSelectableChannel implements NetworkChannel{
		public static ServerSocketChannel open();//得到一个ServerSocketChannel通道
		public final ServerSocketChannel bind(SocketAddress local);//设置服务器端端口号
		public final SelectableChannel configureBlocking(boolean block);//设置阻塞或非阻塞模式，取值false表示采用非阻塞模式
		public abstract SocketChannel accept();//接受一个连接，返回代表这个连接的通道对象
		public final SelectionKey register(Selector sel, int ops);//注册一个选择器并设置监听事件
}
~~~

## 3.11、SocketChannel
1. SocketChannel，网络IO通道，**具体负责进行读写操作**。NIO包缓冲区的数据写入通道，或者把通道里的数据读到缓冲区
2. 相关方法

~~~Java
public abstract class SocketChannel extends AbstractSelectableChannel implements ByteChannel, ScatteringByteChannel, GatheringByteChannel, NetworkChannel {
		public static SocketChannel open();//得到一个Socket Channel通道
		public fianl SelectableChannel configureBlocking(boolean block);// 设置阻塞或非阻塞模式，取值false表示采用非阻塞模式
		public abstract boolean connect(SocketAddress remote);//连接服务器
		public abstract boolean finishConnect();//如果上面的方法连接失败，接下来就要通过该方法完成连接操作
		public int write(ByteBuffer src);//往通道里面写数据
		public int read(ByteBuffer dst);//从通道里面读取数据
		public final SelectionKey register(Selector sel,int ops,Object att);//注册一个选择器并设置监听事件，最后一个参数可以设置共享数据
		public final void close();//关闭通道
}
~~~
## 3.12、NIO网络编程应用实例-群聊系统

实例要求：
1. 编写一个搞NIO群聊系统，实现服务器端和客户端之间的数据简单通讯(非阻塞)
2. 实现多人群聊
3. 服务器端：可以监测用户上线、离线，并实现消息转发功能
4. 客户端：通过Channel可以无阻塞发送消息给其它所有用户，同时可以接受到其它用户发送的消息(由服务器转发得到)
5. 目的：进一步理解NIO非阻塞网络编程
6. 示意图
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210910211940.png)
**服务器端：**
~~~Java
package com.clover.nio.groupchat;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.util.Iterator;

public class GroupChatServer {
    // 定义属性
    private Selector selector;
    private ServerSocketChannel listenChannel;
    private static final int PORT=6667;

    // 构造器
    // 初始化工作
    public GroupChatServer(){
        try {
            // 得到选择器
            selector = Selector.open();
            // 得到ServerSocketChannel
            listenChannel = ServerSocketChannel.open();
            // 绑定端口
            listenChannel.socket().bind(new InetSocketAddress(PORT));
            // 设置非阻塞模式
            listenChannel.configureBlocking(false);
            // 将该listenChannel 注册到Selector
            listenChannel.register(selector, SelectionKey.OP_ACCEPT);
        } catch (IOException e){
            e.printStackTrace();
        }
    }

    // 监听
    public void listen(){
        try {
            // 循环处理
            while(true){
                int count = selector.select();
                if(count > 0){ // 有事件处理
                    // 遍历得到selectionKey集合
                    Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();
                    while (iterator.hasNext()){
                        // 取出selectionKey
                        SelectionKey key = iterator.next();

                        //根据key对应的通道发生的事件做相应的处理
                        if(key.isAcceptable()){
                            SocketChannel sc = listenChannel.accept();
                            sc.configureBlocking(false);
                            // 将该sc注册到Selector
                            sc.register(selector,SelectionKey.OP_READ);
                            // 输出提示
                            System.out.println(sc.getRemoteAddress() + "上线");
                        }

                        if (key.isReadable()){// 通道发送read事件，即通道是可读状态
                            // 处理读事件(专门写方法)
                            readData(key);
                        }

                        // 手动从集合中移除当前的 SelectionKey ，防止重复操作
                        iterator.remove();
                    }
                }  else {
                    System.out.println("等待......");
                }
            }

        } catch (Exception e){
            e.printStackTrace();
        } finally{

        }
    }

    // 读取客户端数据 通过对应的key获取到对应的通道
    public void readData(SelectionKey key){
        // 定义channel属性
        SocketChannel channel = null;
        try {
            // 得到channel
            channel = (SocketChannel) key.channel();
            // 创建Buffer
            ByteBuffer buffer = ByteBuffer.allocate(1024);//33
            int count = channel.read(buffer);
            // 根据count的值做处理
            if(count > 0){
                // 把缓冲区的数据转成字符串
                String msg = new String(buffer.array());
                // 输出该消息
                System.out.println("from 客户端：" + msg);

                // 向其它的客户端转发消息(去掉自己)，专门写一个方法来处理
                sendInfoToOtherClients(msg,channel);
            }
        } catch (IOException e){
            try {
                System.out.println(channel.getRemoteAddress() + " 离线了....");
                // 取消注册
                key.cancel();
                // 关闭通道
                channel.close();
            } catch (IOException e1){
                e1.printStackTrace();
            }

        }
    }

    // 转发消息给其它客户端(通道)
    public void sendInfoToOtherClients(String msg,SocketChannel self) throws IOException{
        System.out.println("服务器转发消息中..........");
        // 遍历 所有注册到Selector 上的 SocketChannel，并排除自身
        for (SelectionKey key:selector.keys()){
            // 通过key得到对应的 SocketChannel
            Channel targetChannel = key.channel();

            // 排除自己
            if (targetChannel instanceof SocketChannel && targetChannel != self){
                // 转型
                SocketChannel dest = (SocketChannel)targetChannel;
                // 将msg存储到buffer
                ByteBuffer buffer = ByteBuffer.wrap(msg.getBytes());
                // 将buffer的数据写入通道
                dest.write(buffer);
            }
        }
    }



    public static void main(String[] args) {
        // 创建服务器对象
        GroupChatServer groupChatServer = new GroupChatServer();

        groupChatServer.listen();
    }
}
~~~

**客户端：**
~~~Java
package com.clover.nio.groupchat;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Scanner;

public class GroupChatClient {
    // 定义相关的属性
    private final String HOST = "127.0.0.1"; // 服务器的IP
    private final int PORT = 6667;
    private Selector selector;
    private SocketChannel socketChannel;
    private String username;

    // 构造器,完成初始化工作
    public GroupChatClient() throws IOException {
        selector = Selector.open();
        // 连接服务器
        socketChannel = SocketChannel.open(new InetSocketAddress(HOST,PORT));
        // 设置非阻塞
        socketChannel.configureBlocking(false);
        // 将channel注册到selector
        socketChannel.register(selector, SelectionKey.OP_READ);
        // 得到username
        username = socketChannel.getLocalAddress().toString();

        System.out.println(username + " is ok.....");
    }

    // 向服务器发送消息
    public void sendInfo(String info){
        info = username + "说：" + info;

        try{
            socketChannel.write(ByteBuffer.wrap(info.getBytes()));
        } catch (IOException e){
            e.printStackTrace();
        }
    }

    // 读取从服务器端回复的消息
    public void readInfo(){
        try {
            int readChannels = selector.select();
            if (readChannels > 0){
                Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();
                while (iterator.hasNext()){
                    SelectionKey key = iterator.next();
                    if(key.isReadable()){
                        SocketChannel channel = (SocketChannel)key.channel();
                        ByteBuffer buffer = ByteBuffer.allocate(1024);
                        channel.read(buffer);
                        // 将读取到的数据转化为字符串
                        String msg = new String(buffer.array());
                        // 输出消息并去掉头尾空格
                        System.out.println(msg.trim());
                    }
                    // 手动从集合中移除当前的 SelectionKey ，防止重复操作
                    iterator.remove();
                }
            } else {
//                System.out.println("没有可以用的通道....");
            }
        } catch (IOException e){
            e.printStackTrace();
        }
    }

    public static void main(String[] args) throws Exception{
        // 启动我们的客户端
        GroupChatClient chatClient = new GroupChatClient();

        // 启动一个线程,每隔3秒，读取从服务器端发送的数据
        new Thread(){
            public void run(){
                while (true){
                    chatClient.readInfo();
                    try {
                        Thread.currentThread().sleep(3000);
                    }catch (InterruptedException e){
                        e.printStackTrace();
                    }
                }
            }
        }.start();

        // 发送数据给服务器端
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNextLine()){
            String s = scanner.nextLine();
            chatClient.sendInfo(s);
        }
    }
}
~~~

	注意：这里面得到一个通道时，记得设置为非阻塞模式；其次，这个里面的离线部分没理解清楚，参考B站一位评论
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210910212227.png)

## 3.13、NIO与零拷贝

### 3.13.1、零拷贝基本介绍
1. 零拷贝是网络编程的关键，很多性能优化都离不开。
2. 在Java程序中，常用的零拷贝有mmap(内存映射)和sendFile。那么，他们在OS里面，到底是怎么样的一个设计？我们分析mmap和sendFile这两个零拷贝
3. 另外我们看下NIO中如何使用零拷贝

### 3.13.2、传统IO

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210910215929.png)

### 3.13.3、mmap优化
1. mmap**通过内存映射**，将文件映射到内核缓冲区，同时，用户空间可以共享内核空间的数据。这样，在进行网络传输时，就可以减少内核空间到用户控件的拷贝次数
2. mmap示意图
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210910220324.png)

### 3.13.4、sendFile优化
1. Linux2.1版本提供了sendFile函数，其基本原理如下：数据根本不经过用户态，直接从内核缓冲区进入到Socket Buffer，同时，由于和用户态完全无关，就减少了一次上下文切换
2. 示意图和小结
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210910215124.png)

	提示：零拷贝从操作系统角度看是没有CPU拷贝
3. Linux在2.4版本中，做了一些修改，避免了从**内核缓冲区**拷贝到**Socket buffer**的操作，直接拷贝到**协议栈**，从而再一次减少了数据拷贝，具体如下图
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210910215425.png)

	注意：这里其实有一次CPU拷贝kernel buffer -> socket buffer，但是，拷贝的信息很少，比如length，offset，消耗低，可以忽略不计

### 3.13.5、零拷贝的再次理解
1. 我们说零拷贝是从操作系统的角度来说的。因为内核缓冲器之间，没有数据是重复的(只有kernel buffer 有一份数据)
2. 零拷贝不仅仅带来更少的数据复制，还能带来其它的性能又是，例如更少的上下文切换，更少的CPU缓存伪共享以及无CPU校验和计算

### 3.13.6、mmap和sendFile的区别
1. mmap适合小数据量读写，sendFile适合大文件传输
2. mmap需要**4**次上下文切换，**3**次数据拷贝；sendFile需要**3**次上下文切换，**最少2**次数据拷贝
3. sendFile可以利用DMA方式，减少CPU拷贝，mmap则不能(必须从内核拷贝到Socket缓冲区)


## 3.14、零拷贝实例

案例要求：
1. 使用传统的IO方法传递一个大文件
2. 使用NIO零拷贝方式传递(transferTo)一个大文件
3. 看看两种传递方式耗时时间分别是多少

### 3.14.1、传统IO方式

**服务器端：**
~~~Java
package com.clover.nio.zerocopy;

import java.io.DataInputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class OldIOServer {

    public static void main(String[] args) throws Exception {
        ServerSocket serverSocket = new ServerSocket(7001);

        while (true) {
            Socket socket = serverSocket.accept();
            DataInputStream dataInputStream = new DataInputStream(socket.getInputStream());

            try {
                byte[] byteArray = new byte[4096];

                while (true) {
                    int readCount = dataInputStream.read(byteArray, 0, byteArray.length);

                    if (-1 == readCount) {
                        break;
                    }
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }
}
~~~

**客户端：**
~~~Java
package com.clover.nio.zerocopy;

import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.InputStream;
import java.net.Socket;

public class OldIOClient {

    public static void main(String[] args) throws Exception {
        Socket socket = new Socket("localhost", 7001);

        String fileName = "protoc-3.6.1-win32.zip";
        InputStream inputStream = new FileInputStream(fileName);

        DataOutputStream dataOutputStream = new DataOutputStream(socket.getOutputStream());

        byte[] buffer = new byte[4096];
        long readCount;
        long total = 0;

        long startTime = System.currentTimeMillis();

        while ((readCount = inputStream.read(buffer)) >= 0) {
            total += readCount;
            dataOutputStream.write(buffer);
        }

        System.out.println("发送总字节数： " + total + ", 耗时： " + (System.currentTimeMillis() - startTime));

        dataOutputStream.close();
        socket.close();
        inputStream.close();
    }
}
~~~

结果图：
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210911161411.png)

### 3.14.2、transferTo

**服务器端：**
~~~Java
package com.clover.nio.zerocopy;

import java.net.InetSocketAddress;
import java.net.ServerSocket;
import java.nio.ByteBuffer;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;

public class NewIOServer {
  public static void main(String[] args) throws Exception{
    ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
    InetSocketAddress inetSocketAddress = new InetSocketAddress(7001);
    ServerSocket serverSocket = serverSocketChannel.socket();
    serverSocket.bind(inetSocketAddress);

    //创建Buffer
    ByteBuffer buffer = ByteBuffer.allocate(4096);

    while (true){
      SocketChannel socketChannel = serverSocketChannel.accept();

      int readCount = 0;

      while (-1 != readCount){
        try {
          readCount = socketChannel.read(buffer);
        } catch (Exception e){
          e.printStackTrace();
        }

        // 将buffer倒带，因为这个buffer这次使用后下次还需使用
        buffer.rewind();// 使 position = 0 mark=-1 作废
      }
    }
  }
}
~~~

**客户端：**
~~~Java
package com.clover.nio.zerocopy;

import java.io.FileInputStream;
import java.net.InetSocketAddress;
import java.nio.channels.FileChannel;
import java.nio.channels.SocketChannel;

public class NewIOClient {
  public static void main(String[] args) throws Exception{
      SocketChannel socketChannel = SocketChannel.open();
      socketChannel.connect(new InetSocketAddress("localhost",7001));
      String filename = "protoc-3.6.1-win32.zip";

      // 得到一个文件channel
      FileChannel fileChannel = new FileInputStream(filename).getChannel();

      // 准备发送
      long startTime = System.currentTimeMillis();

      // 在Linux下，一个transferTo 方法就可以完成传输
      // 在windows下，一次调用transferTo只能发送8M文件，就需要分段传输文件，而且要注意传输位置
      long transferCount = fileChannel.transferTo(0, fileChannel.size(), socketChannel);

      System.out.println("发送的总的字节数 =" + transferCount + " 耗时："+(System.currentTimeMillis()-startTime));

      // 关闭通道
      fileChannel.close();
  }
}
~~~

结果图：
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210911161735.png)

## 3.15、Java AIO基本介绍
1. JDK7引入了Asynchronous I/O，即AIO。在进行I/O编程中，常用到两种模式：Reactor和Proactor。Java 的NIO就是Reactor，当有时间触发时，服务器端得到通知，进行相应的处理
2. AIO即NIO2.0，叫做异步不阻塞的IO。即AIO引入异步通道的概念，采用了Proactor模式，简化了程序编写，有效的请求才启动线程，它的特点时先由操作系统完成后才通知服务端程序启动线程区处理，一般适用于连接数较多且连接时间较长的应用
3. 目前AIO还没有广泛应用，Netty也是基于NIO，而不是AIO，因此我们就不详解AIO了，有兴趣的同学可以参考[Java新一代网络编程模型AIO原理及Linux系统AIO介绍](http://www.52im.net/thread-306-1-1.html)

## 3.16、BIO、NIO、AIO对比表
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210911164234.png)

**举例说明**
1. **同步阻塞**：到理发店理发，就一直等理发师，知道轮到自己理发
2. **同步非阻塞**：到理发店理发，发现前面有其它人理发，给理发师说下，先干其它事情，一会过来看是否轮到自己
3. **异步非阻塞**：给理发师打电话，让理发师上门服务，自己干其它事情，理发师自己来家给你理发

# 4、Netty概述

## 4.1、原生NIO存在的问题
1. NIO的类库和API复杂，使用麻烦：需要熟练掌握Selector、ServerSocketChannel、SocketChannel、ByteBuffer等
2. 需要具备其它的额外技能：要熟悉Java多线程编程，因为NIO编程涉及到Reactor模式，你必须对多线程和网络编程非常熟悉，才能编写出高直连的NIO程序
3. 开发工作量和难度都非常大：例如客户端面临断连重连、网络闪断、半包读写、失败缓存、网络拥塞和异常流的处理等等
4. JDK NIO的BUG：例如臭名昭著的 Epoll Bug，它会导致Selector空轮询，最终导致CPU100%。直到JDK 1.7 版本该问题仍旧存在，没有被根本解决

## 4.2、Netty官网说明

[Netty官网](https://netty.io/)

Netty is _an asynchronous event-driven network application framework_  
for rapid development of maintainable high performance protocol servers & clients.
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210911172202.png)

## 4.3、Netty的优点

Netty对JDK自带的NIO的API进行了封装，解决了上述问题

1. 设计优雅：适用于各种传输类型的统一API阻塞和非阻塞Socket；基于灵活且可扩展的事件模型，可以清晰的分离关注点；高度可定制的线程模型，单线程，一个或多个线程池
2. 使用方便：详细记录的Javadoc，用户指南和实例；没有其它依赖项，JDK 5(Netty 3.x)或6 (Netty 4.x)就足够了
3. **高性能、吞吐量更高：延迟耕地；减少资源消耗；最小化不必要的内存复制**
4. 安全：完整的SSL/TLS和StartTLS支持
5. 社区活跃、不断更新、版本迭代周期端，发现的Bug可以被及时修复，同时，更多的性功能会被加入

## 4.4、版本说明
1. netty版本分别为netty3.x、netty4.x和netty5.x
2. 因为netty5出现重大Bug，已经被官网废弃了，目前推荐使用的是Netty4.x的稳定版本
3. 目前在官网可下载的版本netty3.x、netty4.0.x和netty4.1.x
4. [Netty下载地址](https://github.com/netty/netty/releases)
