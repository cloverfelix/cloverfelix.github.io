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
2. 将`SocketChannel`注册到Selector上，使用`register(Selector sel,int ops)`，一个selector上可以注册多个`SocketChannel`
3. 注册后返回一个`SelectionKey`，会和该Selector关联`该key被Selector管理起来，做成一个集合的形式`
4. Selector进行监听，使用`select`方法，可以使用阻塞的也可以使用非阻塞的，返回有事件发生的通道的个数
5. 进一步得到各个SelectionKey(有事件发生)
6. 再通过SelectionKey反向获取`SocketChannel`，通过channel()方法
7. 可以通过得到的`channel`，完成业务处理


## 3.8、NIO





