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
2. **Selector能够检测到多个注册的通道上是否有事件发生(注意：多个Channel以事件的方式可以注册到同一个Selector)**，如果有事件发生，便获取事件然后针对每个事件进行相应的处理。这样就可以只用一个单线程去管理多个通道，也就是管理多个连接和请求。
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

# 5、Netty高性能框架设计

## 5.1、线程模型基本介绍
1. 不同的线程模式，对程序的性能有很大影响，为了搞清楚Netty线程模式，我们来系统的讲解下各个 线程模式，最后看看Netty线程模型有什么优越性
2. 目前存在的线程模型有：**传统阻塞I/O服务模型**、**Reactor模式**
3. **根据Reactor的数量和处理资源池线程数量的不同，有三种典型的实现**
	- 单Reactor 单线程
	- 单Reactor 多线程
	- 主从Reactor 多线程
4. Netty线程模式(Netty主要基于**主从Reactor多线程模型**做了一定的改进，其中主从Reactor多线程模型有多个Reactor)

## 5.2、传统阻塞I/O服务模型
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210912095422.png)
### 5.2.1、工作原理图

黄色的框表示对象，蓝色的框表示线程，白色的框表示方法(API)

### 5.2.2、模型特点
1. 采用阻塞IO模式获取输入的数据
2. 每个连接都需要独立的线程完成数据的输入，业务处理，数据返回

### 5.2.3、问题分析
1. 当并发数很大，就会创建大量的线程，占用很大系统资源
2. 连接创建后，如果当前线程暂时没有数可读，该线程会阻塞在read操作，造成线程资源浪费


## 5.3、Reactor模式

### 5.3.1、针对传统阻塞I/O服务模型的2个缺点，解决方案
1. 基于I/O复用模型：多个连接公用一个阻塞对象，应用程序只需在一个阻塞对象等待，无需阻塞等待所有连接。当某个连接有新的数据可以处理时，操作系统通知应用程序，线程从阻塞状态返回，开始进行业务处理

	Reactor 对应的叫法：1、反应器模式 2、分发者模式(Dispatcher) 3、通知者模式(notifier)
2. 基于线程池复用线程资源：不必再为每个连接创建线程，将连接完成后的业务处理任务分配给线程进行处理，一个线程可以处理多个连接的业务
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210912102218.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210912102940.png)

	I/O复用结合线程池，就是Reactor模式基本设计思想，如图：
	1. Reactor模式，通过一个或多个输入同时传递给服务处理器的模式(基于事件驱动)
	2. 服务器端程序处理传入的多个请求，并将他们同步分派到相应的处理线程，因此Reactor模式也叫Dispatcher模式
	3. Reactor模式使用IO复用监听，收到事件后，分发给某个线程(进程)，这点就是网络服务器高并发处理关键

### 5.3.2、Reactor模式中核心组成：
1. Reactor：Reactor在一个单独的线程中运行，负责监听和分发事件，分发给适当的处理程序来对IO事件做出反应。它就像公司的电话接线员，它接听来自客户的电话并将线路转移到适当的联系人
2. Handlers：处理程序执行I/O事件要完成的实际事件，类似于客户想要与之交谈的公司中的实际官员。Reactor通过调度适当的处理程序来响应I/O事件，处理程序执行非阻塞操作

### 5.3.3、Reactor模式分类：

根据Reactor的数量和处理资源池线程数量的不同，有三种典型的实现
- 单Reactor 单线程
- 单Reactor 多线程
- 主从Reactor 多线程

## 5.4、单Reactor 单线程
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210912110418.png)

前面所写的NIO群聊系统就是一个单Reactor 单线程模式

### 5.4.1、方案说明
1.Select时前面I/O复用模型介绍的标准网络编程API，可以实现应用程序通过一个阻塞对象监听多路连接请求
2.Reactor对象通过Select监听客户端请求事件，收到事件后通过Dispatch进行分发
3.如果是建立连接请求事件，则由Acceptor通过Accept处理连接请求，然后创建一个Handler对象处理连接完成后的后续业务处理
4.如果不是建立连接事件，则Reactor会分发调用对应的Handler来响应
5.Handler会完成Read->业务处理->Send的完整业务流程

结合实例；服务器端用一个线程通过多路复用搞定所有的IO操作(包括连接、读、写等)，编码简单，清晰明了，但是如果客户端连接数量较多，将无法支撑，前面的NIO群聊系统就属于这种类型
	
### 5.4.2、方案优缺点分析
1. **优点**：模型简单，没有多线程、进行通信、竞争的问题，全部都在一个线程中完成
2. **缺点**：性能问题，只有一个线程，无法完全发挥多核CPU的性能。Handler在处理某个连接上的业务时，整个进程无法处理其它连接事件，很容易导致性能瓶颈
3. **缺点**：可靠性问题，线程意外终止，过着进入死循环，会导致整个系统通信模块不可以，不能接收和处理外部消息，造成节点故障
4. **使用场景**：客户端的数量有限，业务处理非常快速，比如Redis在业务处理的时间复杂度O(1)的情况

## 5.5、单Reactor 多线程

工作原理示意图：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210912115652.png)
### 5.5.1、方案说明：
1. Reactor对象通过select监控客户端请求事件，收到事件后，通过Dispatch进行分发
2. 如果是建立连接请求，则由Acceptor通过Accept处理连接请求，然后创建一个Handler对象处理完成连接后的各种事件
3. 如果不是连接请求，则由Reactor分发调用连接对应的Handler来处理
4. Handler只负责响应事件，不做具体的业务处理，通过read读取数据后，会分发给后面的worker线程池的某个线程处理业务
5. worker线程池会分配独立的线程完成真正的业务，并将结果返回给handler
6. handler收到响应后，通过send将结果返回给client

### 5.5.2、方案优缺点分析：
1. **优点**：可以充分的利用多核CPU的处理能力
2. **缺点**：多线程数据共享和访问比较复杂，Reactor处理所有事件的监听和响应，在单线程运行，在高并发场景容易出现性能瓶颈

## 5.6、主从Reactor 多线程

工作原理示意图：
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210912123810.png)

### 5.6.1、方案说明：
1. Reactor主线程MainReactor对象通过select监听事件，收到事件后，通过Acceptor处理连接事件
2. 当Acceptor处理连接事件后，MainReactor将连接分配给SubReactor
3. SubReactor将连接加入到连接队列进行监听，并创建handler进行各种事件处理
4. 当有新的事件发生时，SubReactor就会调用对应的handler处理
5. handler通过read读取数据，分发给后面的worker线程池处理
6. worker线程池分配独立worker线程进行业务处理，并返回结果
7. handler收到响应结果后，再通过send将结果返回给client
8. **Reactor主线程可以对应多个Reactor子线程，即MainReactor可以关联多个SubReactor**


### 5.6.2、方案优缺点说明
1.优点：父线程与子线程的数据交互简单职责明确，父线程只需要接收新连接，子线程完成后续的业务处理
2.优点：父线程与子线程的数据交互简单，Reactor主线程只需要把新连接传给子线程，子线程无需返回数据
3.缺点：编程复杂度较高
		
## 5.7、Reactor模式小结

	1. 单Reactor 单线程，前台接待员和服务员是同一个人，全程为顾客服务
	2. 单Reactor 多线程，1个前台接待员，多个服务员，接待员只负责接待
	3. 主从Reactor 多线程，一个礼仪，多个前台接待员，多个服务生，礼仪负责将顾客领进门，然后交给接待员接待，服务员负责服务

**Reactor模式具有以下优点：**
1. 响应快，不必为单个同步事件所阻塞，因为它有多个`SubReactor`，这个阻塞了，它也可以通过下一个SubReactor来进行处理，虽然Reactor本身依然是同步的
2. 可以最大程度的避免复杂的多线程及同步问题，并且避免了多线程/进程的切换开销
3. 扩展性好，可以方便的通过增加Reactor实例个数来充分利用CPU资源
4. 复用性好，Reactor模型本身与具体事件处理逻辑无关，具有很高的复用性？？？？？？

## 5.8、Netty模型

### 5.8.1、工作原理示意图1-简单版

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210912153424.png)

1. BossGroup线程维护Selector，只关注Accept
2. 当接收到Accept事件后，获取对应的`SockerChannel`，进一步封装成`NIOSocketChannel`并注册到`WorkerGroup线程(事件循环)`，并进行维护
3. 当WorkerGroup线程监听到注册到selector中的通道发生自己感兴趣的事件后，就进行处理(由handler处理)，**注意：handler已经加入到通道中了**

### 5.8.2、工作原理示意图2-进阶版

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210912153853.png)

	这里就很像我们之前的主从Reactor，BossGroup中有多个MainReactor
	
### 5.8.3、工作原理示意图3-详细版
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210912161512.png)

1. Netty抽象出两组线程池BossGroup：专门负责接收客户端的连接，WorkGroup：专门负责网络的读写
2. BossGroup和WorkGroup类型都是`NioEventLoopGroup`
3. NioEventLoopGroup相当于一个事件循环组，这个组中含有多个事件循环，每一个事件循环是NioEventLoop
4. NioEventLoop表示一个不断循环的执行处理任务的线程，每个NioEventLoop都有一个`Selector`，用于监听绑定在其上的socket的网络通讯
5. NioEventLoopGroup可以有多个线程，既可以含有多个NioEventLoop
6. 每个Boss NioEventLoop循环执行的步骤有3步
	1. 轮询accept事件
	2. 处理accept事件，与client建立连接，生成`NioSocketChannel`，`并将其注册到某个WorkerNioEventLoop`上的`Selector`
	3. 处理任务队列的任务，即runAllTasks
7. 每个WorkerNioEventLoop循环执行的步骤
	1. 轮询read，write事件
	2. 处理I/O事件，即read、write事件，在对应的`NioSocketChannel`上处理
	3. 处理任务队列的任务，即runAllTasks
8. 每个WorkerNioEventLoop处理业务时，会使用`pipeline(管道)`，`pipeline中包含了channel`，即通过pipeline可以获取到对应`通道`,管道中维护了很多的`处理器`

### 5.8.4、Netty快速入门实例-TCP服务
1. 实例要求：使用IDEA创建Netty项目
2. Netty服务器在6668端口监听，客户端能发送消息给服务器"hello,服务器"
3. 服务器可以恢复消息给客户端"hello,客户器"
4. 目的：对Netty线程模型有一个初步认识
5. 说明：创建Maven项目，并引入Netty包

**Bootstrap执行流程**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210915100204.png)

	ChannelFuture 在Netty中的所有的I/O操作都是异步执行的，这就意味着任何一个I/O操作会立刻返
	回，不保证在调用结束的时候操作会执行完成。因此，会返回一个ChannelFuture的实例，通过这个实
	例可以获取当前I/O操作的状态。

**服务器端：**
~~~Java
package com.clover.netty.simple;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelOption;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;

public class NettyServer {
  public static void main(String[] args) throws Exception{
      /*
       * 说明
       * 1.创建两个线程组 bossGroup 和 workerGroup
       * 2.bossGroup 只是处理连接请求，真正的和客户端业务处理，会交给 workerGroup 完成
       * 3.两个线程组都是无线循环
       */
      EventLoopGroup bossGroup = new NioEventLoopGroup();
      EventLoopGroup workerGroup = new NioEventLoopGroup();

      try {
          // 创建服务器的启动对象，配置参数
          ServerBootstrap bootstrap = new ServerBootstrap();

          // 使用链式编程来进行设置
          bootstrap.group(bossGroup,workerGroup)// 设置两个线程组
                  .channel(NioServerSocketChannel.class)// 使用NioSocketChannel作为服务器的通道实现类型
                  .option(ChannelOption.SO_BACKLOG,128)// 设置线程队列等待连接个数
                  .childOption(ChannelOption.SO_KEEPALIVE,true)// 设置保持活动连接状态
                  .childHandler(new ChannelInitializer<SocketChannel>() { // 创建一个通道初始化对象(匿名对象)
                      // 向workerGroup中EventLoop所关联的通道对应的pipeline设置处理器
                      @Override
                      protected void initChannel(SocketChannel ch) throws Exception {
                          ch.pipeline().addLast(new NettyServerHandler());
                      }
                  });// 给我们的workerGroup的 EventLoop对应的管道设置处理器

          System.out.println("服务器 is ready");

          /*
           * ChannelFuture 在Netty中的所有的I/O操作都是异步执行的，这就意味着任何一个I/O操作会立刻返回，不保证在调用结束的时候操作会执行完成。因此，会返回一个ChannelFuture的实例，通过这个实例可以获取当前I/O操作的状态。
           */

          // 绑定一个端口，并且同步，生成了一个ChannelFuture 对象
          // 启动服务器(并绑定端口)
          // 为什么使用同步？因为Netty是基于异步操作的，如果不使用同步，可能服务器还未启动就执行下面的语句了，就会产生异常
          ChannelFuture cf = bootstrap.bind(6668).sync();

          // 对关闭通道进行监听(只有当你有一个关闭通道这样的事件发生时，才会去进行处理)
          // 这里为什么也需要使用同步？因为不适用同步，他就会跳过这个语句直接执行finally中的语句关闭线程组了
          cf.channel().closeFuture().sync();
      } finally{
          bossGroup.shutdownGracefully();
          workerGroup.shutdownGracefully();
      }
  }
}
~~~

**服务器端Handler**
~~~Java
package com.clover.netty.simple;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.util.CharsetUtil;

/*
 * 说明
 * 1.我们自定义一个Handler需要继承netty规定好的某个HandlerAdapter(规范)
 * 2.这时我们自定义一个Handler，才能称之为一个handler
 */
public class NettyServerHandler extends ChannelInboundHandlerAdapter {
    /*
     * 读取数据(这里我们可以读取客户端发送的数据)
     * 1.ChannelHandlerContext ctx：上下文对象，含有管道pipeline(业务逻辑处理)，通道channel(数据读写处理)，地址
     * 2.Object msg：就是客户端发送的数据，默认时Object类型
     */
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        System.out.println("server ctx =" + ctx);
        // 将msg转化成一个ByteBuf
        // ByteBuf时Netty提供的，不是NIO的ByteBuffer
        ByteBuf buf = (ByteBuf) msg;
        System.out.println("客户端发送的消息是：" + buf.toString(CharsetUtil.UTF_8));
        System.out.println("客户端地址：" + ctx.channel().remoteAddress());
    }

    // 数据读取完毕返回给客户端的消息
    @Override
    public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {
        // writeAndFlush 是 write方法 + flush方法
        // 将数据写入到缓冲区，并刷新
        // 一般讲，我们对这个发送的数据需要进行编码
        ctx.writeAndFlush(Unpooled.copiedBuffer("hello,客户端",CharsetUtil.UTF_8));
    }

    // 当出现异常时，一般是需要关闭通道
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        ctx.channel().close();
    }
}
~~~

**客户端**
~~~Java
package com.clover.netty.simple;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;

public class NettyClient {
  public static void main(String[] args) throws Exception{
      // 客户端需要一个事件循环组
      EventLoopGroup group = new NioEventLoopGroup();

      try {
          // 创建一个客户端启动对象
          // 注意客户端使用的不是ServerBootstrap，而是Bootstrap
          Bootstrap bootstrap = new Bootstrap();

          // 设置相关参数,链式编程
          bootstrap.group(group) // 设置线程组
                  .channel(NioSocketChannel.class) // 设置客户端通道的实现类型(将来使用反射处理)
                  .handler(new ChannelInitializer<SocketChannel>() {
                      @Override
                      protected void initChannel(SocketChannel ch) throws Exception {
                          ch.pipeline().addLast(new NettyClientHandler());// 加入自己的处理器
                      }
                  });
          System.out.println("客户端 is ok");

          // 启动客户端去连接服务器端
          // 关于 ChannelFuture 后面要分析，涉及到Netty的异步模型
          ChannelFuture channelFuture = bootstrap.connect("127.0.0.1", 6668).sync();

          // 给关闭通道进行监听(sync是让其为非阻塞？？？？？？)
          channelFuture.channel().closeFuture().sync();
      }finally{
          group.shutdownGracefully();
      }
  }
}
~~~

**客户端Handler**
~~~Java
package com.clover.netty.simple;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.util.CharsetUtil;

public class NettyClientHandler extends ChannelInboundHandlerAdapter {
    // 当通道就绪时，就会触发该方法
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        System.out.println("client " + ctx);
        ctx.writeAndFlush(Unpooled.copiedBuffer("hello,Server", CharsetUtil.UTF_8));
    }

    // 当通道有读取事件时，会触发
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        ByteBuf buf = (ByteBuf) msg;
        System.out.println("输出服务器回复的消息：" + buf.toString(CharsetUtil.UTF_8));
        System.out.println("服务器端的地址：" + ctx.channel().remoteAddress());
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}
~~~

### 5.8.5、任务队列中的Task有3种典型使用场景
1. 用户程序自定义的普通任务(使用线程睡眠来解释)
~~~Java
@Override
public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
// 比如这里我们有一个非常耗费时间的业务->异步执行-> 提交到该channel对应的NIOEventLoop的taskQueue中
ctx.channel()
	.eventLoop()
	.execute(new Runnable() {
		  @Override
		  public void run() {
			try {
			  Thread.sleep(10 * 1000);
			  ctx.writeAndFlush(Unpooled.copiedBuffer("hello,客户端2",CharsetUtil.UTF_8));
			} catch (Exception e) {
				System.out.println("发生异常" + e.getMessage());
			}
		  }
		});
}
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210915100759.png)

2. 用户自定义定时任务
~~~Java
// 解决方案2：用户自定义定时任务
        /*
         * 使用scheduled，先执行taskQueue，执行完毕后再执行scheduled。并且scheduled里的延迟时间是从taskQueue执行第一个任务的时候开始计算的
         * 如果你写了Thread.sleep()，那么就会按照该设置的延时加上schedule中设置的时间总和
         */
        ctx.channel().eventLoop().schedule(new Runnable() {
            @Override
            public void run() {
                try {
                    Thread.sleep(5 * 1000);
                    ctx.writeAndFlush(Unpooled.copiedBuffer("hello,客户端3",CharsetUtil.UTF_8));
                } catch (Exception e) {
                    System.out.println("发生异常" + e.getMessage());
                }
            }
        },15, TimeUnit.SECONDS);
~~~
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210915111616.png)

3. 非当前Reactor线程调用Channel的各种方法

		例如在推送系统的业务线程里面，根据用户的表示，找到对应的Channel引用，然后调用Write类方法
		向该用户推送消息，就会进入到这种场景。最终的Write会提交到任务队列中后被异步消费
	
方案再说明：
1. Netty抽象出两组**线程池**，BossGroup专门负责接收客户端连接，WorkerGroup专门负责网络读写操作。
2. NioEventLoop表示一个不断循环执行处理任务的线程，每个NioEventLoop都有一个selector，用于监听绑定在其上的socket网络通道。
3. NioEventLoop内部采用串行化设计，从消息的读取->解码->处理->编码->发送，始终由IO线程NioEventLoop负责

- NioEventLoopGroup下包含多个NioEventLoop
- 每个NioEventLoop中包含一个Selector，一个taskQueue
- 每个NioEventLoop的Selector上可以注册监听多个NioSocketChannel
- 每个NioSocketChannel只会绑定再唯一的NioEventLoop上
- 每个NioSocketChannel都会绑定有一个自己的ChannelPipeline

## 5.9、异步模型

### 5.9.1、基本介绍
1. 异步的概念和同步相对。当一个异步过程调用发出后，调用者不能立刻得到结果。实际处理这个调用的组件在完成后，通过状态、通知和回调来通知调用者
2. **Netty中的IO操作是异步的**，包括Bind、Write、Connect等操作会简单的返回一个ChannelFuture
3. 调用者并不能立刻获得结果，而是通过Future-Listener机制，用户可以方便的主动获取或者通过通知机制获得IO操作结果
4. Netty的异步模型是建立在future和callback之上的，callback就是回调。重点说Future，它的核心思想是：假设一个方法fun，计算过程可能非常耗时，等待fun返回显然不合适。那么可以在调用fun 的时候，立马返回一个Future，后续可以通过Future区监控方法fun的处理过程(即：Future-Listener机制)

### 5.9.2、Future说明
1. 表示**异步的执行结果**，可以通过它提供的方法来检测执行是否完成
2. ChannelFuture是一个接口：public interface ChannelFuture ,我们可以添加监听器，当监听的时间发生时，就会通知到监听器.**案例说明**

### 5.9.3、工作原理示意图
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210915183944.png)
**说明:**
1. 在使用Netty进行编程时，拦截操作和转换出入站数据只需要您提供callback或利用future即可。这使得**链式操作**简单、高效，并有利于编写可重用的、通用的代码。
2. Netty框架的目标就是让你的业务逻辑从网络基础应用编码中分离出来、解脱出来



### 5.9.4、Future-Listener机制

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210915161318.png)

**举例说明：**

演示：绑定端口是异步操作，当绑定操作处理完，将会调用相应的监听器处理逻辑+

~~~Java
cf.addListener(new ChannelFutureListener() {
  @Override
  public void operationComplete(ChannelFuture future) throws Exception {
	  if(future.isSuccess()){
		  System.out.println("监听端口 6668成功");
	  } else {
		  System.out.println("监听端口 6668失败");
	  }
  }
});
~~~

	小结：相比传统阻塞I/O，执行I/O操作后线程会被阻塞住，知道操作完成；异步处理的好处是不会造成
	线程阻塞，线程在I/O操作期间可以执行别的程序，在高并发情况下会更稳定和更高的吞吐量
	
## 5.10、快速入门实例-HTTP服务
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210915183154.png)

**服务器端**	
~~~JAVA 
package com.clover.netty.http;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;

public class TestServer {
  public static void main(String[] args) throws Exception{

      EventLoopGroup bossGroup = new NioEventLoopGroup();
      EventLoopGroup workerGroup = new NioEventLoopGroup();

      try {
          ServerBootstrap serverBootstrap = new ServerBootstrap();

          serverBootstrap.group(bossGroup,workerGroup)
                  .channel(NioServerSocketChannel.class)
                  .childHandler(new TestServerInitializer());

          // 注意：谷歌浏览器使用6668端口可能无法访问，可能存在安全验证，换一个端口就好
          ChannelFuture channelFuture = serverBootstrap.bind(15989).sync();

          channelFuture.channel().closeFuture().sync();
      }finally{
          bossGroup.shutdownGracefully();
          workerGroup.shutdownGracefully();
      }
  }
}
~~~

**向管道加入处理器**
~~~Java
package com.clover.netty.http;

import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.socket.SocketChannel;
import io.netty.handler.codec.http.HttpServerCodec;

public class TestServerInitializer extends ChannelInitializer<SocketChannel> {
    @Override
    protected void initChannel(SocketChannel ch) throws Exception {
        // 向管道加入处理器
        // 得到管道
        ChannelPipeline pipeline = ch.pipeline();

        // 加入一个netty提供的httpServerCodec(编解码器)
        /*
         * HttpServerCodec说明
         * 1.HttpServerCodec 是netty提供处理Http的 编-解码器
         */
        pipeline.addLast("MyHttpServerCodec",new HttpServerCodec());

        // 2.增加一个自定的handler
        pipeline.addLast("MyTestHttpServerHandler",new TestHttpServerHandler());
    }
}
~~~

**自定义handler**
~~~Java
package com.clover.netty.http;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.handler.codec.http.*;
import io.netty.util.CharsetUtil;

import java.net.URI;
import java.nio.charset.Charset;

/*
 * 说明
 * 1.SimpleChannelInboundHandler 是 ChannelInboundHandlerAdapter的子类
 * 2.HttpObject 客户端和服务器端相互通讯的数据被封装成HttpObject
 */
public class TestHttpServerHandler extends SimpleChannelInboundHandler<HttpObject> {

    // channelRead0读取客户端数据
    @Override
    protected void channelRead0(ChannelHandlerContext ctx, HttpObject msg) throws Exception {
        // 判断msg 是不是 httprequest请求
        if( msg instanceof HttpRequest){

            System.out.println("pipeline hascode =" + ctx.pipeline().hashCode()+ " TestHttpServerHandler hascode =" +this.hashCode());

            System.out.println("msg 类型=" + msg.getClass());
            System.out.println("客户端地址" + ctx.channel().remoteAddress());

            // 将msg转化为HttpRequest
            HttpRequest httpRequest = (HttpRequest) msg;
            //获取uri，过滤指定资源
            URI uri = new URI(httpRequest.uri());
            if("/favicon.ico".equals(uri.getPath())){
                System.out.println("请求了 favicon.ico，不做响应");
                return;
            }

            // 回复信息给浏览器,需要满足http协议
            ByteBuf content = Unpooled.copiedBuffer("hello，我是服务器", CharsetUtil.UTF_8);

            // 构造一个http的响应，即httpResponse
            FullHttpResponse response = new DefaultFullHttpResponse(HttpVersion.HTTP_1_1, HttpResponseStatus.OK, content);

            /*
             * 注意：如果中文乱码，则需要在 CONTENT_TYPE 中，修改为text/plain;charset=utf-8，或者直接将CharsetUtil.UTF_8改为UTF_16
             */
            response.headers().set(HttpHeaderNames.CONTENT_TYPE,"text/plain;charset=utf-8");
            response.headers().set(HttpHeaderNames.CONTENT_LENGTH,content.readableBytes());

            // 将构建好的response返回
            ctx.writeAndFlush(response);
        }
    }
}
~~~

	http不是一个长连接，用完即断掉
	当你有多个浏览器连接服务器时，每个连接对应的pipeline和handler都是唯一的、不一样的，不是公用一个pipeline和handler
	浏览器刷新时也会产生一个新的pipeline和handler，这是因为http协议原因所导致的，与TCP协议还是有区别的
	
# 6、核心模块组件
## 6.1、Bootstrap、ServerBootstrap
1. Bootstrap 意思是引导，一个 Netty 应用通常由一个 Bootstrap 开始，主要作用是配置整个 Netty 程序，串联各个组件，Netty中`Bootstrap` 类是`客户端程序`的启动引导类，`ServerBootstrap`是`服务端`启动引导类
2. 常见的方法有
	- public ServerBootstrap group(EventLoopGroup parentGroup, EventLoopGroup childGroup)，该方法用于服务器端，用来设置两个 EventLoop
	- public B group(EventLoopGroup group) ，该方法用于客户端，用来设置一个 EventLoop
	- public B channel(Class<> channelClass)，该方法用来设置一个服务器端的通道实现
	- public <> B option(ChannelOption<> option, T value)，用来给 ServerChannel 添加配置
	- public <> ServerBootstrap childOption(ChannelOption<> childOption, T value)，用来给接收到的通道添加配置
	- public ServerBootstrap childHandler(ChannelHandler childHandler)，该方法用来设置业务处理类(自定义的 handler)
	- public ChannelFuture bind(int inetPort) ，该方法用于服务器端，用来设置占用的端口号
	- public ChannelFuture connect(String inetHost, int inetPort) ，该方法用于客户端，用来连接服务器端	
## 6.2、Future、ChannelFuture
1. **Netty 中所有的 IO 操作都是异步的**，不能立刻得知消息是否被正确处理。但是**可以过一会等它执行完成或者直接注册一个监听**，具体的实现就是通过`Future 和 ChannelFutures`，他们可以注册一个监听，当操作执行成功或失败时监听会自动触发注册的监听事件
2. 常见的方法有
	- Channel channel()，返回当前正在进行 IO 操作的通道
	- ChannelFuture sync()，等待同步操作执行完毕

## 6.3、Channel
1. Netty 网络通信的组件，能够用于执行网络 I/O 操作
2. 通过Channel 可获得当前网络连接的通道的状态
3. 通过Channel 可获得 网络连接的配置参数 （例如接收缓冲区大小）
4. **Channel 提供异步的网络 I/O 操作(如建立连接，读写，绑定端口)**，异步调用意味着任何 I/O 调用都将立即返回，并且不保证在调用结束时所请求的 I/O 操作已完成
5. 调用立即返回一个 ChannelFuture 实例，通过注册监听器到 ChannelFuture 上，可以 监控I/O 操作成功、失败或取消时回调通知调用方
6. 支持关联 I/O 操作与对应的处理程序
7. 不同协议、不同的阻塞类型的连接都有不同的 Channel 类型与之对应，常用的 Channel 类型:
	- NioSocketChannel，异步的客户端 TCP Socket 连接。
	- NioServerSocketChannel，异步的服务器端 TCP Socket 连接。
	- NioDatagram Channel，异步的 UDP 连接。
	- NioSctpChannel，异步的客户端 Sctp 连接。
	- NioSctpServerChannel，异步的 Sctp 服务器端连接，这些通道涵盖了 UDP 和 TCP 网络 IO 以及文件 IO。

## 6.4、Selector
1. Netty 基于 Selector 对象实现 I/O 多路复用，通过 Selector 一个线程可以监听多个连接的 Channel 事件
2. 当向一个 Selector 中注册 Channel 后，**Selector 内部的机制就可以自动不断地查询(Select) 这些注册的 Channel 是否有已就绪的 I/O 事件（例如可读，可写，网络连接完成等）**，这样程序就可以很简单地使用一个线程高效地管理多个 Channel 

## 6.5、ChannelHandler 及其实现类
1. ChannelHandler 是一个接口，处理 I/O 事件或拦截 I/O 操作，并将其转发到其 ChannelPipeline(业务处理链)中的下一个处理程序
2. ChannelHandler 本身并没有提供很多方法，因为这个接口有许多的方法需要实现，方便使用期间，可以继承它的子类
3. **ChannelHandler 及其实现类一览图**
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210915220803.png)
	- ChannelInboundHandler 用于处理入站 I/O 事件
	- ChannelOutboundHandler 用于处理出站 I/O 操作
	`适配器`
	- ChannelInboundHandlerAdapter 用于处理入站 I/O 事件
	- ChannelOutboundHandlerAdapter 用于处理出站 I/O 操作
	- ChannelDuplexHandler 用于处理入站和出站事件

4. 我们经常需要自定义一个 Handler 类去继承 ChannelInboundHandlerAdapter，然后通过重写相应方法实现业务逻辑，我们接下来看看一般都需要重写哪些方

~~~Java
public class Channel InboundHandlerAdapter extends Channel HandlerAdapter 
implements Channel InboundHandler {
    public Channel InboundHandlerAdapter() { }
    public void channelRegistered(ChannelHandlerContext ctx) throws Exception {
        ctx.fireChannelRegistered();
    }
    public void channelUnregistered(ChannelHandlerContext ctx) throws Exception {
        ctx.fireChannelUnregistered();
    }
    //通道就绪事件
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        ctx.fireChannelActive();
    }
    public void channelInactive(ChannelHandlerContext ctx) throws Exception {
        ctx.fireChannelInactive();
    }
    //通道读取数据事件
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        ctx.fireChannelRead(msg);
	}
	//数据读取完毕事件
	public void channelReadComplete(ChannelHandlerContext ctx, Object msg) throws Exception {
        ctx.fireChannelRead(msg);
	}
	//数据读取完毕事件
	public void channelReadComplete(ChannelHandlerContext ctx, Object msg) throws Exception {
        ctx.fireChannelRead(msg);
	}
	public void userEventTriggered(ChannelHandlerContext ctx, Object evt) throws Exception {
        ctx.fireUserEventTriggered(evt);
	}
	public void channelWritabilityChanged(ChannelHandlerContext ctx) throws Exception {
        ctx.fireChannelWritabilityChanged(msg);
	}
	//通道发生异常事件
	public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        ctx.fireExceptionCaught(msg);
	}
~~~

## 6.6、Pipeline和ChannelPipeline 

**ChannelPipeline 是一个重点：**

1. ChannelPipeline 是一个 Handler 的集合，它负责处理和拦截 inbound 或者 outbound 的事件和操作，相当于一个贯穿 Netty 的链。(**也可以这样理解：ChannelPipeline 是保存 ChannelHandler 的 List，用于处理或拦截 Channel 的入站事件和出站操作**)
2. ChannelPipeline 实现了一种高级形式的拦截过滤器模式，使用户可以完全控制事件的处理方式，以及 Channel 中各个的 ChannelHandler 如何相互交互
3. 在 Netty 中每个 Channel 都有且仅有一个 ChannelPipeline 与之对应，它们的组成关系如下
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210916102025.png)
	- 一个 Channel 包含了一个 ChannelPipeline，而 ChannelPipeline 中又维护了一个由 ChannelHandlerContext 组成的双向链表，并且每个 ChannelHandlerContext 中又关联着一个 ChannelHandler
	- 入站事件和出站事件在一个双向链表中，`入站事件`会**从链表 head 往后传递**到最后一个入站的 handler，`出站事件`会**从链表 tail 往前传递**到最前一个出站的 handler，两种类型的 handler 互不干扰
 4. 常用方法
	- Channel Pipeline addFirst(ChannelHandler... handlers)，把一个业务处理类（handler）添加到链中的第一个位置
	- Channel Pipeline addLast(ChannelHandler... handlers)，把一个业务处理类（handler）添加到链中的最后一个位置

## 6.7、ChannelHandlerContext
1. 保存 Channel 相关的所有上下文信息，同时关联一个 ChannelHandler 对象
2. 即ChannelHandlerContext 中包含一个具体的事件处理器 ChannelHandler ， 同 时ChannelHandlerContext 中也绑定了对应的 pipeline 和 Channel 的信息，方便对 ChannelHandler进行调用
3. 常用方法
	- ChannelFuture close()，关闭通道
	- ChannelOutboundInvoker flush()，刷新
	- ChannelFuture writeAndFlush(Object msg) ， 将数据写到ChannelPipeline中当前ChannelHandler 的下一个 ChannelHandler 开始处理 **(出站)**

 
## 6.8、ChannelOption
1. Netty 在创建 Channel 实例后,一般都需要设置 ChannelOption 参数
2. ChannelOption 参数如下:
	- **ChannelOption.SO_BACKLOG**
		- 对应TCP/IP协议 listen 函数中的 backlog 参数，用来初始化服务器可连接队列大小。服务端处理客户端连接请求是顺序处理的，所以同一时间只能处理一个客户端连接。多个客户端来的时候，服务端将不能处理的客户端连接请求放在队列中等待处理，backlog参数指定了队列的大小
	- **ChannelOption.SO_KEEPALIVE**
		- 一直保持连接活动状态

## 6.9、EventLoopGroup 和其实现类 NioEventLoopGroup
1. EventLoopGroup是一组 EventLoop 的抽象，Netty为了更好的利用多核 CPU 资源，一般会有多个 EventLoop 同时工作，每个EventLoop维护着一个Selector实例
2. EventLoopGroup 提供 next 接口，可以从组里面按照一定规则获取其中一个 EventLoop来处理任务。在 Netty 服务器端编程中，我们一般都需要提供两个 EventLoopGroup，例如：BossEventLoopGroup 和 WorkerEventLoopGroup
3. 通常一个服务端口即一个 ServerSocketChannel对应一个Selector 和一个EventLoop线程。BossEventLoop 负责接收客户端的连接并将 SocketChannel 交给 WorkerEventLoopGroup 来进行 IO 处理，如下图所示
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210916111322.png)
	- BossEventLoopGroup 通常是一个单线程的 EventLoop，Event Loop 维护着一个注册了ServerSocketChannel 的 Selector 实例，BossEventLoop 不断轮询Selector 将连接事件分离出来
	- 通常是OP_ACCEPT 事件，然后将接收到的 SocketChannel 交给 WorkerEventLoopGroup
	- WorkerEventLoopGroup 会由 next 选择其中一个 EventLoop来将这个SocketChannel 注册到其维护的Selector 并对其后续的 IO 事件进行处理
4. 常用方法
	- public NioEventLoopGroup()，构造方法
	- public Future<> shutdownGracefully()，断开连接，关闭线程

 ## 6.10、Unpooled类
 1. Netty 提供一个专门用来`操作缓冲区`(**即Netty的数据容器**)的工具类
 2. 常用方法如下所示
	~~~java 
	//通过给定的数据和字符编码返回一个 ByteBuf 对象（类似于 NIO 中的 ByteBuffer 但有区别）
	public static ByteBuf copiedBuffer(CharSequence string, Charset charset)
	~~~
 3. 举例说明Unpooled 获取 Netty的数据容器ByteBuf 的基本使用 `(案例说明)`
	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210916215409.png)

**实例代码一**
~~~Java
package com.clover.netty.buf;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;

public class NettyByteBuf01 {
  public static void main(String[] args) {
      // 先创建一个ByteBuf
      /*
       * 说明
       * 1.创建对象，该对象包含一个数组array，是一个byte[10]类型的数组
       * 2.在netty的buffer中，不需要使用flip反转
       *    底层维护了 readerIndex 和 writerIndex
       * 3.通过 readerIndex 和 writerIndex 和 capacity ，将buffer分成三个区域
       * 0---readerIndex，已经读取的区域
       * readerIndex --- writerIndex，可以读取的区域
       * writerIndex --- 0，可写的区域
       */
      ByteBuf buf = Unpooled.buffer(10);

      for (int i = 0; i < 10; i++){
          buf.writeByte(i);
      }

      System.out.println("capacity=" + buf.capacity());

    // 输出
    //      for (int i = 0; i < buf.capacity(); i++){
    //          // 这种方法是通过下标获取对应的值，不会使readerIndex改变值
    //          System.out.println(buf.getByte(i));
    //      }

      // 改变readerIndex的输出方式
      for (int i = 0;i < buf.capacity(); i++) {
          System.out.println(buf.readByte());
      }
  }
}
~~~

**实例代码二**

~~~Java
package com.clover.netty.buf;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.util.CharsetUtil;

import java.nio.charset.Charset;

public class NettyByteBuf02 {
  public static void main(String[] args) {
      //创建ByteBuf
      /*
       * 说明
       * 第一个参数代表写入内容，第二个代表内容的字符集编码
       */
      ByteBuf buf = Unpooled.copiedBuffer("hello,world!", Charset.forName("utf-8"));

      // 使用相关的方法
      if (buf.hasArray()){
          // 获取buf的数组
          byte[] content = buf.array();

          System.out.println(new String(content,CharsetUtil.UTF_8));
          System.out.println("bytebuf=" + buf);

          // 获取数组位移偏移量
          System.out.println(buf.arrayOffset());// 0
          System.out.println(buf.readerIndex());// 0
          System.out.println(buf.writerIndex());// 12
          System.out.println(buf.capacity());// 36

          //System.out.println(buf.readByte());// 104 这是h代表的ASCII码
          System.out.println(buf.getByte(0));// 104 这是h代表的ASCII码

          int len = buf.readableBytes();// 获取可读字节数
          System.out.println("len=" + len);// 如果在前面没有使用readbyte读取数据，则不会对可读字节产生影响，如果使用就会有影响

          // 使用for循环取出各个字节
          for (int i = 0; i < len; i++) {
              System.out.println((char) buf.getByte(i));
          }

          // 按照某个范围读取
          System.out.println(buf.getCharSequence(0,4,Charset.forName("utf-8")));
          System.out.println(buf.getCharSequence(4,6,Charset.forName("utf-8")));
      }
  }
}
~~~


## 6.11、群聊系统升级
1. 编写一个 Netty 群聊系统，实现服务器端和客户端之间的数据简单通讯（非阻塞）
2. 实现多人群聊
3. 服务器端：可以监测用户上线，离线，并实现消息转发功能
4. 客户端：通过channel 可以无阻塞发送消息给其它所有用户，同时可以接受其它用户发送的消息(由服务器转发得到)
5. 目的：进一步理解Netty非阻塞网络编程机制

**服务器端：**
~~~Java
package com.clover.netty.groupchat;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.handler.codec.string.StringDecoder;
import io.netty.handler.codec.string.StringEncoder;

public class GroupChatServer {
    // 监听端口
    private int port;

    public GroupChatServer(int port){
        this.port = port;
    }

    public void run() throws Exception{

        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try{
            ServerBootstrap bootstrap = new ServerBootstrap();

            bootstrap.group(bossGroup,workerGroup)
                    .channel(NioServerSocketChannel.class)
                    .option(ChannelOption.SO_BACKLOG,128)
                    .childOption(ChannelOption.SO_KEEPALIVE,true)
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel ch) throws Exception {
                            // 获取到pipeline
                            ChannelPipeline pipeline = ch.pipeline();
                            // 向pipeline中加入解码器
                            pipeline.addLast("decoder",new StringDecoder());
                            // 向pipeline中加入编码器
                            pipeline.addLast("encoder",new StringEncoder());
                            // 加入自己的业务处理handler
                            pipeline.addLast(new GroupChatServerHandler());
                        }
                    });

            System.out.println("netty 服务器启动");
            ChannelFuture channelFuture = bootstrap.bind(port).sync();

            // 监听关闭事件
            channelFuture.channel().closeFuture().sync();
        }finally{
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }

    }

  public static void main(String[] args) throws Exception {
        new GroupChatServer(7000).run();
  }
}
~~~

**服务器端handler：**
~~~Java
package com.clover.netty.groupchat;

import io.netty.channel.Channel;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.channel.group.ChannelGroup;
import io.netty.channel.group.DefaultChannelGroup;
import io.netty.util.concurrent.GlobalEventExecutor;

import java.text.SimpleDateFormat;
import java.util.Date;

public class GroupChatServerHandler extends SimpleChannelInboundHandler<String> {
    // 定义一个channel组，管理所有的channel
    // GlobalEventExecutor.INSTANCE 是全局的事件执行器，是一个单例
    private static ChannelGroup channelGroup= new DefaultChannelGroup(GlobalEventExecutor.INSTANCE);
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");// 输出接收消息时的时间

    // handlerAdded表示连接建立，一旦连接，第一个被执行
    // 将当前channel加入到channelGroup
    @Override
    public void handlerAdded(ChannelHandlerContext ctx) throws Exception {
        Channel channel = ctx.channel();
        /*
         * 将该客户端加入聊天的信息推送给其它在线的客户端
         * 该方法会将channelGroup中所有的channel遍历一遍，并发送消息，我们不需要自己遍历
         */
        channelGroup.writeAndFlush("[客户端]" + channel.remoteAddress() + " 加入聊天" + sdf.format(new Date()) + "\n");
        channelGroup.add(channel);
    }

    // 表示断开连接了,将xxx客户离开信息推送给当前在线的客户
    // 只要触发了handlerRemoved函数就会自动在channelGroup中移除自己，不需要自己手动去清除
    @Override
    public void handlerRemoved(ChannelHandlerContext ctx) throws Exception {
        Channel channel = ctx.channel();
        channelGroup.writeAndFlush("[客户端]" + channel.remoteAddress() + "离开了\n");
        System.out.println("channelGroup size =" + channelGroup.size());
    }

    // 表示channel处于活动状态，可以提示xx上线
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        System.out.println(ctx.channel().remoteAddress()+ "上线了~");
    }

    // 表示channel处于非活动状态，可以提示xx离线
    @Override
    public void channelInactive(ChannelHandlerContext ctx) throws Exception {
        System.out.println(ctx.channel().remoteAddress()+ "离线了~");
    }

    // 读取数据
    @Override
    protected void channelRead0(ChannelHandlerContext ctx, String msg) throws Exception {
        // 获取到当前的channel
        Channel channel = ctx.channel();
        // 这个时候我们需要遍历channelGroup，根据不同的情况回送不同的消息，主要是为了剔除自己
        channelGroup.forEach( ch ->{
            // 不是当前的channel，就直接转发消息
            if (channel !=ch){
                // 将当前channel通道的信息转发给ch，即channelGroup中遍历出来的某一个channel
                ch.writeAndFlush("[客户]" + channel.remoteAddress() + "发送了消息：" + msg + "\n");
            } else {
                ch.writeAndFlush("[自己]发送了消息" + msg + "\n");
            }
        });
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        // 关闭通道
        ctx.channel().close();
    }
}
~~~

**客户端：**
~~~Java
package com.clover.netty.groupchat;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;
import io.netty.handler.codec.string.StringDecoder;
import io.netty.handler.codec.string.StringEncoder;

import java.util.Scanner;

public class GroupChatClient {
    // 属性
    private final String host;
    private final int port;

    public GroupChatClient(String host, int port) {
        this.host = host;
        this.port = port;
    }

    public void run() throws Exception{
        EventLoopGroup group = new NioEventLoopGroup();

        try {
            Bootstrap bootstrap = new Bootstrap();

            bootstrap.group(group)
                    .channel(NioSocketChannel.class)
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel ch) throws Exception {
                            // 得到pipeline
                            ChannelPipeline pipeline = ch.pipeline();
                            // 加入相关的handler
                            // 向pipeline中加入解码器
                            pipeline.addLast("decoder",new StringDecoder());
                            // 向pipeline中加入编码器
                            pipeline.addLast("encoder",new StringEncoder());
                            // 加入自定义的handler
                            pipeline.addLast(new GroupChatClientHandler());
                        }
                    });

            ChannelFuture channelFuture = bootstrap.connect(host, port).sync();
            // 得到channel
            Channel channel = channelFuture.channel();
            System.out.println("-----------" + channel.localAddress() + "-------------");

            // 需要输入信息，因此我需要一个扫描器
            Scanner scanner = new Scanner(System.in);
            while(scanner.hasNextLine()){
                String msg = scanner.nextLine();
                // 通过channel发送信息到服务器端
                channel.writeAndFlush(msg + "\r\n");
            }
        } finally{
            group.shutdownGracefully();
        }
    }

  public static void main(String[] args) throws Exception{
        new GroupChatClient("127.0.0.1",7000).run();
  }
}
~~~

**客户端handler：**
~~~Java
package com.clover.netty.groupchat;

import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;


public class GroupChatClientHandler extends SimpleChannelInboundHandler<String> {

    @Override
    protected void channelRead0(ChannelHandlerContext ctx, String msg) throws Exception {
        System.out.println(msg.trim());
    }
}
~~~

## 6.12、Netty心跳检测机制案例

实例要求：
1. 编写一个 Netty心跳检测机制案例, 当服务器超过3秒没有读时，就提示读空闲
2. 当服务器超过5秒没有写操作时，就提示写空闲
3. 实现当服务器超过7秒没有读或者写操作时，就提示读写空闲

~~~Java
package com.clover.netty.heartbeat;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;
import io.netty.handler.logging.LogLevel;
import io.netty.handler.logging.LoggingHandler;
import io.netty.handler.timeout.IdleStateHandler;

import java.util.concurrent.TimeUnit;

public class MyServer {
  public static void main(String[] args) throws Exception{
      // 创建两个线程组
      NioEventLoopGroup bossGroup = new NioEventLoopGroup(1);
      NioEventLoopGroup workerGroup = new NioEventLoopGroup();

      try {

          ServerBootstrap serverBootstrap = new ServerBootstrap();

          serverBootstrap.group(bossGroup,workerGroup)
                  .channel(NioServerSocketChannel.class)
                  .handler(new LoggingHandler(LogLevel.INFO)) // 在我们的bossGroup中增加一个日志处理器
                  .childHandler(new ChannelInitializer<SocketChannel>() {
                      @Override
                      protected void initChannel(SocketChannel ch) throws Exception {
                          ChannelPipeline pipeline = ch.pipeline();
                          // 加入一个netty提供的处理器 IdleStateHandler
                          /*
                           * 说明
                           * 1.IdleStateHandler 是netty提供的处理空闲状态的处理器
                           * 2.long readerIdleTime : 表示多长时间没有读，就会发送一个心跳检测包检测是否连接
                           * 3.long writerIdleTime : 表示多长时间没有写，就会发送一个心跳检测包检测是否连接
                           * 4.long allIdleTime : 表示多长时间没有读写，就会发送一个心跳检测包检测是否连接
                           * 5.当IdleStateHandler触发后，就会传递给换到的下一个handler去处理，通过调用(触发)下一个handler的userEventTriggered
                           *   在该方法中去处理IdleStateHandler(读空闲，写空闲，读写空闲)
                           * 6.如果是0表示不检测，所以如果全是0，则相当于没添加这个 IdleStateHandler，连接是个普通的短连接。
                           */
                          pipeline.addLast(new IdleStateHandler(3,5,7, TimeUnit.SECONDS));
                          // 加入一个对空闲检测进一步处理的handler(自定义)
                          pipeline.addLast(new MyServerHandler());
                      }
                  });
          ChannelFuture channelFuture = serverBootstrap.bind(7000).sync();
          channelFuture.channel().closeFuture().sync();

      } finally{
          bossGroup.shutdownGracefully();
          workerGroup.shutdownGracefully();
      }
  }
}
~~~

~~~Java
package com.clover.netty.heartbeat;

import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.handler.timeout.IdleStateEvent;

public class MyServerHandler extends ChannelInboundHandlerAdapter {
    /*
     * @param ctx 上下文
     * @param evt 事件
     * @throws Exception
     */
    @Override
    public void userEventTriggered(ChannelHandlerContext ctx, Object evt) throws Exception {
        if (evt instanceof IdleStateEvent){
            // 将evt向下转型 IdleStateEvent
            IdleStateEvent event = (IdleStateEvent) evt;
            String eventType = null;
            switch (event.state()){
                case READER_IDLE:
                    eventType = "读空闲";
                    break;
                case WRITER_IDLE:
                    eventType = "写空闲";
                    break;
                case ALL_IDLE:
                    eventType = "读写空闲";
                    break;
            }

            System.out.println(ctx.channel().remoteAddress() + "---超时事件---" + eventType);
            System.out.println("服务器开始做相应的处理");

            // 如果发生空闲，我们关闭通道，结果显示就只会显示一次空闲后就不在显示了
            ctx.channel().close();
        }
    }
}
~~~


## 6.13、Netty 通过WebSocket编程实现服务器和客户端长连接

实例要求：
1. Http协议是无状态的, 浏览器和服务器间的请求响应一次，下一次会重新创建连接
2. 要求：实现基于webSocket的长连接的全双工的交互
3. 改变Http协议多次请求的约束，实现长连接了， 服务器可以发送消息给浏览器
4. 客户端浏览器和服务器端会相互感知，比如服务器关闭了，浏览器会感知，同样浏览器关闭了，服务器会感知
5. 运行界面如下

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20210923093240.png)

**服务器端：**
~~~Java
package com.clover.netty.websocket;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.handler.codec.http.HttpObjectAggregator;
import io.netty.handler.codec.http.HttpServerCodec;
import io.netty.handler.codec.http.websocketx.WebSocketServerProtocolHandler;
import io.netty.handler.logging.LogLevel;
import io.netty.handler.logging.LoggingHandler;
import io.netty.handler.stream.ChunkedWriteHandler;

public class MyServer {
  public static void main(String[] args) throws Exception{
      // 创建两个线程组
      EventLoopGroup bossGroup = new NioEventLoopGroup(1);
      EventLoopGroup workerGroup = new NioEventLoopGroup();

      try{

          ServerBootstrap serverBootstrap = new ServerBootstrap();

          serverBootstrap.group(bossGroup,workerGroup)
                  .channel(NioServerSocketChannel.class)
                  .handler(new LoggingHandler(LogLevel.INFO))
                  .childHandler(new ChannelInitializer<SocketChannel>() {
                      @Override
                      protected void initChannel(SocketChannel ch) throws Exception {
                          ChannelPipeline pipeline = ch.pipeline();

                          // 因为基于http协议，使用http的编码和解码器
                          pipeline.addLast(new HttpServerCodec());
                          // 是以块方式写，添加ChunkedWriteHandler处理器
                          pipeline.addLast(new ChunkedWriteHandler());

                          /*
                           * 说明
                           * 1.http数据在传输过程中是分段，HttpObjectAggregator，作用就是可将多个段聚合
                           * 2.这就是为什么，当浏览器发送大量数据时，就会发出多次http请求
                           */
                          pipeline.addLast(new HttpObjectAggregator(8192));
                          /*
                           * 说明
                           * 1.对应的websocket，它的数据是以 帧(frame) 形式传递
                           * 2.可以看到WebSocketFrame 下面有六个子类
                           * 3.浏览器请求时 ws://localhost:7000/xxx 表示请求的qurl
                           * 4.WebSocketServerProtocolHandler 核心功能是将http协议审计为ws协议，即保持长连接
                           * 5.是通过一个 状态码 101
                           */
                          pipeline.addLast(new WebSocketServerProtocolHandler("/hello"));

                          // 自定义的handler，处理业务逻辑
                          pipeline.addLast(new MyTextWebSocketFrameHandler());
                      }
                  });

          ChannelFuture channelFuture = serverBootstrap.bind(7000).sync();

          channelFuture.channel().closeFuture().sync();

      } finally{
          bossGroup.shutdownGracefully();
          workerGroup.shutdownGracefully();
      }
  }
}
~~~

**对应的handler：**
~~~Java
package com.clover.netty.websocket;

import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.handler.codec.http.websocketx.TextWebSocketFrame;

import java.time.LocalDateTime;

// 这里 TextWebSocketFrame 类型，表示一个文本帧(frame)
public class MyTextWebSocketFrameHandler extends SimpleChannelInboundHandler<TextWebSocketFrame> {
    @Override
    protected void channelRead0(ChannelHandlerContext ctx, TextWebSocketFrame msg) throws Exception {
        System.out.println("服务器收到消息：" + msg.text());

        // 回复浏览器消息
        ctx.channel().writeAndFlush(new TextWebSocketFrame("服务器时间：" + LocalDateTime.now() + " " + msg.text()));
    }

    // 当web客户端连接后，就会触发这个方法
    @Override
    public void handlerAdded(ChannelHandlerContext ctx) throws Exception {
        // id 表示唯一的值，LongText 这个值是唯一的，ShortText 这个值不是唯一的
        System.out.println("handlerAdded 被调用了" + ctx.channel().id().asLongText());
        System.out.println("handlerAdded 被调用了" + ctx.channel().id().asShortText());
    }

    @Override
    public void handlerRemoved(ChannelHandlerContext ctx) throws Exception {
        System.out.println("handlerRemoved 被调用了" + ctx.channel().id().asLongText());
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        System.out.println("异常发生" + cause.getMessage());
        ctx.close();// 关闭连接
    }
}
~~~

**客户端：**
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script>
    var socket;
    // 判断当前浏览器是否支持websocket
    if (window.WebSocket){
        socket = new WebSocket("ws://localhost:7000/hello");
        socket.onmessage = function (ev) {
            var rt = document.getElementById("responseText");
            rt.value = rt.value + "\n" + ev.data;
        };

        // 相当于连接开启(感知到连接开启)
        socket.onopen = function (ev) {
            var rt = document.getElementById("responseText");
            rt.value = "连接开启了...";
        };

        // 相当于连接关闭(感知到连接关闭)
        socket.onclose = function (ev) {
            var rt = document.getElementById("responseText");
            rt.value = rt.value + "\n" + "连接关闭了...";
        };
    } else {
        alert("当前浏览器不支持webSocket");
    }

    // 发送消息到服务器
    function send(message) {
        // 先判断websocket是否创建好
        if (!window.socket){
            return ;
        }
        if (socket.readyState == WebSocket.OPEN){
            // 通过socket发送消息
            socket.send(message);
        } else {
            alert("连接未开启...");
        }
    }
</script>
    <form onsubmit="return false">
        <textarea name="message" style="height: 300px;width: 300px"></textarea>
        <input type="button" value="发送消息" onclick="send(this.form.message.value)"/>
        <textarea id="responseText" style="height: 300px;width: 300px"></textarea>
        <input type="button" value="清空内容" onclick="document.getElementById('responseText').value=''"/>
    </form>

</body>
</html>
~~~



