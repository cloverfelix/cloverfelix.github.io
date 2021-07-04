# Thread




## 线程简介

 ### Process：进程  程序是指令和数据的有效集合，其本身没有任何运行的含义，是一个静态的概念。而<font color='red'>进程则是执行程序的一次执行过程</font>，它是一个动态的概念。是系统资源分配的单位。一个进程中可以包含若干个线程，当然一个进程中最少包含一个线程。线程是CPU调度和执行的单位。



### Thread：线程  



![image-20200801163243535](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200801163243535.png)





### 两条线程是同时、交替执行的



#### <font color='red'>注意，线程开启不一定立即执行，由CPU调度执行</font>





### 方法一：继承Thread类，重写run()方法，调用start开启线程

~~~JAVA
package com.ren.lesson01;

//创建线程方式一：继承Thread类，重写run()方法，调用start开启线程

//总结：注意，线程开启不一定立即执行，由CPU调度执行

public class TestThread01 extends Thread{
    @Override
    public void run() {
        //run方法线程体
        for (int i = 0; i < 200; i++) {
            System.out.println("我去打篮球了---"+i);
        }
    }

    public static void main(String[] args) {
        //main线程，主线程

        //创建一个线程对象
        TestThread01 testThread01 = new TestThread01();

        //调用start()方法开启线程
        testThread01.start();

        for (int i = 0; i < 1000; i++) {
            System.out.println("我准备出门了---"+i);
        }
    }
}

~~~

结果图：

![](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200802115847679.png)



![image-20200802115920071](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200802115920071.png)



从网络上下载图片，通过一个下载器方法实现

~~~JAVA
package com.ren.lesson01;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;

public class TestThread02 extends Thread {

    private String url;  //网络图片保存地址
    private String name;  //保存的文件名

    public TestThread02(String url,String name){
        this.url = url;
        this.name = name;
    }

    //下载图片线程的执行体
    @Override
    public void run() {
        WebDownloader webDownloader = new WebDownloader();
        webDownloader.Downloader(url,name);
        System.out.println("下载的文件名为："+name);
    }

    public static void main(String[] args) {
        TestThread02 t1 = new TestThread02("https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy9QbjRTbTBSc0F1Z1g0WFJ2ODZpYTZRR0NXVjdGOG5taWJmS0hDRmhhZk4wa2IxRXlCbUh4MmliNThqd0hQWnlydUpSd2hpYVNZM0ppYXptY2liRlB5b2R2cU9pYUEvNjQw?x-oss-process=image/1.jpg","1.jpg");
        TestThread02 t2 = new TestThread02("https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy9QbjRTbTBSc0F1Z1g0WFJ2ODZpYTZRR0NXVjdGOG5taWJmS0hDRmhhZk4wa2IxRXlCbUh4MmliNThqd0hQWnlydUpSd2hpYVNZM0ppYXptY2liRlB5b2R2cU9pYUEvNjQw?x-oss-process=image/2.jpg","2.jpg");
        TestThread02 t3 = new TestThread02("https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy9QbjRTbTBSc0F1Z1g0WFJ2ODZpYTZRR0NXVjdGOG5taWJmS0hDRmhhZk4wa2IxRXlCbUh4MmliNThqd0hQWnlydUpSd2hpYVNZM0ppYXptY2liRlB5b2R2cU9pYUEvNjQw?x-oss-process=image/3.jpg","3.jpg");

        //这三个不是按照顺序来执行的，是同时进行，小的先完成
        t1.start();
        t2.start();
        t3.start();
    }

}

//下载器
class WebDownloader{
    public void Downloader(String url,String name){
        try {
            FileUtils.copyURLToFile(new URL(url),new File(name));
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("IO异常,Downloader方法出现异常"+name);
        }
    }
}

~~~

![image-20200802120138301](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200802120138301.png)















小结：![image-20200802121612288](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200802121612288.png)



### 创建线程方式二：实现runnable接口，重写run方法，执行线程需要丢入runnable接口实现类，调用start方法。

~~~JAVA
package com.ren.lesson01;

//创建线程方式二：实现runnable接口，重写run方法，执行线程需要丢入runnable接口实现类，调用start方法。

public class TestThread03 implements Runnable{
    @Override
    public void run() {
        //run方法线程体
        for (int i = 0; i < 200; i++) {
            System.out.println("我去打篮球了---"+i);
        }
    }

    public static void main(String[] args) {
        //创建runnable接口的实现类对象
        TestThread03 testThread03 = new TestThread03();

        //创建线程对象，通过线程对象来开启我们的线程，代理
//        Thread thread = new Thread(testThread03);
//
//        thread.start();

        new Thread(testThread03).start();

        for (int i = 0; i < 1000; i++) {
            System.out.println("我准备出门了---"+i);
        }
    }
}

~~~

结果图：

![image-20200803135117351](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200803135117351.png)



```
package com.ren.lesson01;

//多个线程同时操作同一个对象
//买火车票的例子

//发现问题：多个线程操作同一个资源的情况下，线程不安全，数据紊乱
public class TestThread04 implements Runnable{

    //票数
    private int ticketNums = 10;

    @Override
    public void run() {
        while (true){
            if(ticketNums<=0){
                break;
            }
            //模拟延时
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            //Thread.currentThread().getName()获取当前线程的名称
            System.out.println(Thread.currentThread().getName()+"-->拿到了第"+ticketNums--+"票");
        }
    }

    public static void main(String[] args) {
        TestThread04 ticket = new TestThread04();

        new Thread(ticket,"你").start();
        new Thread(ticket,"我").start();
        new Thread(ticket,"他").start();
    }
}
```

结果图：

![image-20200803141822154](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200803141822154.png)

![image-20200803133716076](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200803133716076.png)





### 线程创建方式三：实现Callable接口

~~~JAVA
package com.ren.lesson02;

import com.ren.lesson01.TestThread02;
import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;
import java.util.concurrent.*;

//线程创建方式三：实现Callable接口
/**
 * callable的好处：
 * 1.可以定义返回值
 * 2.可以抛出异常
 * 不好处：实现方式复杂些
 */

public class TestCallable implements Callable<Boolean> {
    private String url;  //网络图片保存地址
    private String name;  //保存的文件名

    public TestCallable(String url,String name){
        this.url = url;
        this.name = name;
    }

    //下载图片线程的执行体
    @Override
    public Boolean call() {
        WebDownloader webDownloader = new WebDownloader();
        webDownloader.Downloader(url,name);
        System.out.println("下载的文件名为："+name);
        return true;
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        TestCallable t1 = new TestCallable("https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy9QbjRTbTBSc0F1Z1g0WFJ2ODZpYTZRR0NXVjdGOG5taWJmS0hDRmhhZk4wa2IxRXlCbUh4MmliNThqd0hQWnlydUpSd2hpYVNZM0ppYXptY2liRlB5b2R2cU9pYUEvNjQw?x-oss-process=image/1.jpg","1.jpg");
        TestCallable t2 = new TestCallable("https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy9QbjRTbTBSc0F1Z1g0WFJ2ODZpYTZRR0NXVjdGOG5taWJmS0hDRmhhZk4wa2IxRXlCbUh4MmliNThqd0hQWnlydUpSd2hpYVNZM0ppYXptY2liRlB5b2R2cU9pYUEvNjQw?x-oss-process=image/2.jpg","2.jpg");
        TestCallable t3 = new TestCallable("https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy9QbjRTbTBSc0F1Z1g0WFJ2ODZpYTZRR0NXVjdGOG5taWJmS0hDRmhhZk4wa2IxRXlCbUh4MmliNThqd0hQWnlydUpSd2hpYVNZM0ppYXptY2liRlB5b2R2cU9pYUEvNjQw?x-oss-process=image/3.jpg","3.jpg");

        //创建执行服务
        ExecutorService ser = Executors.newFixedThreadPool(3);

        //提交执行(通过服务来提交我们的线程)
        Future<Boolean> r1 = ser.submit(t1);
        Future<Boolean> r2 = ser.submit(t2);
        Future<Boolean> r3 = ser.submit(t3);

        //获取结果（获取的结果就是线程调用了call函数所返回的结果）
        Boolean rs1 = r1.get();
        Boolean rs2 = r1.get();
        Boolean rs3 = r1.get();

        System.out.println(rs1);
        System.out.println(rs2);
        System.out.println(rs3);

        //关闭服务
        ser.shutdownNow();
    }
}

//下载器
class WebDownloader{
    public void Downloader(String url,String name){
        try {
            FileUtils.copyURLToFile(new URL(url),new File(name));
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("IO异常,Downloader方法出现异常"+name);
        }
    }
}
~~~

```
/**
 * callable的好处：
 * 1.可以定义返回值
 * 2.可以抛出异常
 * 不好处：实现方式复杂些
 */
```

结果图：

![image-20200803134911877](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200803134911877.png)



### 静态代理

#### <font color='red'>使用结婚这个例子来阐述，可以将其与线程进行比较</font>

~~~JAVA
package com.ren.lesson02;

/**
 * 静态代理模式总结：
 * 真实对象（目标对象）和代理对象都要实现同一个接口
 * 代理对象要代理真实角色
 * 好处：代理对象可以做很多真实对象做不了的事情
 *       真实对象专注做自己的事情
 */
public class StaticProxy {
    public static void main(String[] args) {

        You you = new You();

        //将其与多线程对比
        /**
         * Thread就相当于WeddingCompany
         * Thread是一个代理，它代理的是他括号里面的真实对象（Runnable接口）
         * HappyMarry是来自于它的接口中的（代理对象中的），start是Thread里面去弄的，它们都有一个共同的方法，run方法
         */
        new Thread(()-> System.out.println("我爱你")).start();

        new WeddingCompany(you).HappyMarry();

    }
}

interface Marry{
    void HappyMarry();
}

//真实角色，你去结婚
class You implements Marry{
    @Override
    public void HappyMarry() {
        System.out.println("我要结婚了！");
    }
}

//代理角色，帮助你结婚
class WeddingCompany implements Marry{

    //代理谁-->真实目标角色
    private Marry target;

    public WeddingCompany(Marry target) {
        this.target = target;
    }

    @Override
    public void HappyMarry() {
        before();
        this.target.HappyMarry();//这就是真实对象
        after();
    }

    private void after() {
        System.out.println("结婚之后，收尾款");
    }

    private void before() {
        System.out.println("结婚之前，布置现场");
    }
}
~~~

结果图：

![image-20200803161329320](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200803161329320.png)



## Lamda表达式

 ### 函数式接口的定义：

#### 任何接口，如果只包含唯一一个抽象方法，那么它就是一个函数式接口



#### 对于函数式接口，我们可以通过lamda表达式来创建该接口的对象



#### 一步步的简化过程

~~~JAVA
package com.ren.lesson02;

/**
 * 推导lamda表达式
 */
public class TestLambda {

    //3.静态内部类
    static class Like2 implements ILike{
        @Override
        public void lamda() {
            System.out.println("i like lamda2");
        }
    }

    public static void main(String[] args) {
        ILike like = new Like();
        like.lamda();

        like = new Like2();
        like.lamda();

        //4.局部内部类
        class Like3 implements ILike{
            @Override
            public void lamda() {
                System.out.println("i like lamda3");
            }
        }

        like = new Like3();
        like.lamda();

        //5.匿名内部类,没有类的名称，必须借助接口或者父类
        like = new ILike() {
            @Override
            public void lamda() {
                System.out.println("i like lamda4");
            }
        };
        like.lamda();

        //3.用lamda简化
        like = ()->{
            System.out.println("i like lamda5");
        };
        like.lamda();

    }
}

//1.定义一个函数式接口
interface ILike{
    void lamda();
}

//2.实现类
class Like implements ILike{
    @Override
    public void lamda() {
        System.out.println("i like lamda");
    }
}
~~~





#### 实例过程

~~~JAVA
package com.ren.lesson02;

public class TestLamda2 {
    public static void main(String[] args) {

        //lamda表达式简化
        ILove love = (int a)-> {
                System.out.println("i love you-->"+a);
            };

        //简化1.去掉参数类型
        love = (a)-> {
            System.out.println("i love you-->"+a);
        };

        //简化2.去掉括号
        love = a-> {
            System.out.println("i love you-->"+a);
        };

        //简化3.去掉花括号
        love = a-> System.out.println("i love you-->"+a);

        /**
         * 总结：
         *      lamda表达式只能有一行代码的情况下才能简化成为一行（也就是去掉花括号），如果有多行，那么就用代码块包裹。
         *      前提是接口为函数式接口
         *      多个参数也可以去掉参数类型，要去掉就都去掉，必须加上括号
         */
        love.love(8);

    }
}

interface ILove{
    void love(int a);
}



~~~



#### 总结：
         *      lamda表达式只能有一行代码的情况下才能简化成为一行（也就是去掉花括号），如果有多行，那么就用代码块包裹。
         *      前提是接口为函数式接口
         *      多个参数也可以去掉参数类型，要去掉就都去掉，必须加上括号


## 线程五大状态

![image-20200805173616017](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805173616017.png)



### 线程方法

![image-20200805173828769](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805173828769.png)

## <font color='red'>不涉及到多个线程操作一个对象可以直接继承Thread，涉及道路就采用继承Runnable接口</font>



### 线程停止

~~~JAVA
package com.ren.lesson03;
/**
 * 测试stop(停止线程)
 *      1.建议使用线程正常停止--->利用次数，不建议死循环
 *      2.建议使用标志位--->设置一个标志位
 *      3.不要使用stop或者destory等过时或者JDK不建议使用的方法
 */
public class TestStop implements Runnable{

    //1.设置一个标识位
    private boolean flag = true;
    @Override
    public void run() {
        int i = 0;
        while(flag){
            System.out.println("run......Thread"+i++);
        }
    }

    //2.设置一个公开的方法停止线程，转换标志位
    public void stop(){
        this.flag = false;
    }

    public static void main(String[] args) {
        TestStop testStop = new TestStop();
        new Thread(testStop).start();

        for (int i = 0; i < 1000; i++) {
            System.out.println("main"+i);
            if(i==900){
                //调用stop方法切换标志位，让线程停止
                testStop.stop();
                System.out.println("该线程停止了");
            }

        }
    }
}

~~~

结果图：

![image-20200805175819346](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805175819346.png)



### 线程休眠

![image-20200805175852121](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805175852121.png)

~~~JAVA
package com.ren.lesson03;

import java.text.SimpleDateFormat;
import java.util.Date;

//模拟倒计时
public class TestSleep2 {

    public static void main(String[] args) {
        //打印当前系统时间
        Date startTime = new Date(System.currentTimeMillis());//获取系统当前时间

        while(true){
            try {
                Thread.sleep(1000);
                System.out.println(new SimpleDateFormat("HH:mm:ss").format(startTime));
                startTime = new Date(System.currentTimeMillis());//更新当前时间
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

    }

    //模拟倒计时
    public static void tenDown() throws InterruptedException {
        int num = 10;
        while(true){
            Thread.sleep(1000);
            System.out.println(num--);
            if(num<=0){
                break;
            }
        }
    }
}

~~~

结果图：

![image-20200805182652600](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805182652600.png)



### 线程礼让

![image-20200805182503229](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805182503229.png)

~~~JAVA
package com.ren.lesson03;

/**
 * 测试礼让线程
 * 礼让不一定成功，看CPU心情
 */
public class TestYield {
    public static void main(String[] args) {
        MyYield myYield = new MyYield();

        new Thread(myYield,"a").start();
        new Thread(myYield,"b").start();
    }
}

class MyYield implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"线程开始执行！");
        Thread.yield();//礼让
        System.out.println(Thread.currentThread().getName()+"线程停止执行！");
    }
}
~~~

结果图：

![image-20200805183502589](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805183502589.png)

<font color='red'>注意：礼让有可能是成功的，也有可能礼让失败！！！</font>



### 线程强制执行_Join

#### Join合并线程，待此线程执行完成后，再执行其它线程，其它线程阻塞。可以想象成插队。

~~~JAVA
package com.ren.lesson03;

//测试join方法，想象成插队
public class TestJoin implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i < 1000; i++) {
            System.out.println("线程VIP来了"+i);
        }
    }

    public static void main(String[] args) throws InterruptedException {
        //启动我们的线程
        TestJoin testJoin = new TestJoin();
        Thread thread = new Thread(testJoin);


        //主线程
        for (int i = 0; i < 500; i++) {
            if(i==200){
                thread.start();
                thread.join();//插队
            }
            System.out.println("main"+i);
        }
    }
}

~~~

结果图：

![image-20200805185207596](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805185207596.png)



### 线程状态观测

![image-20200805195955020](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805195955020.png)

~~~JAVA
package com.ren.lesson03;

//观察测试线程的状态
public class TestState {

    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(()->{
            for (int i = 0; i < 5; i++) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("//////");
        });



        //观察状态
        Thread.State state = thread.getState();
        System.out.println(state);//New

        //观察启动后
        thread.start();//启动线程
        state = thread.getState();
        System.out.println(state);//Run

        //只要线程不终止，就一直输出状态
        while(state != Thread.State.TERMINATED){
            Thread.sleep(100);//1秒运行十次
            state = thread.getState();//线程状态的更新
            System.out.println(state);
        }
    }
}

~~~

结果图：

![image-20200805201053410](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805201053410.png)



### 线程优先级

#### 线程优先级越高，所获取的资源越多

#### 优先级低只是意味着获得调度的概率低，并不是优先级低就不会被调用了，这都是看CPU的调度

~~~JAVA
package com.ren.lesson03;

//测试线程的优先级
public class Testpriority {

    public static void main(String[] args) {
        //主线程默认优先级
        System.out.println(Thread.currentThread().getName()+"-->"+Thread.currentThread().getPriority());

        MyPriority myPriority = new MyPriority();

        Thread t1 = new Thread(myPriority);
        Thread t2 = new Thread(myPriority);
        Thread t3 = new Thread(myPriority);
        Thread t4 = new Thread(myPriority);
        Thread t5 = new Thread(myPriority);
        Thread t6 = new Thread(myPriority);

        //先设置优先级，在启动
        t1.start();

        t2.setPriority(1);
        t2.start();

        t3.setPriority(5);
        t3.start();

        t4.setPriority(Thread.MAX_PRIORITY);
        t4.start();

//        t5.setPriority(11);
//        t5.start();
//
//        t6.setPriority(-1);
//        t6.start();
    }
}

class MyPriority implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"-->"+Thread.currentThread().getPriority());
    }
}
~~~



结果图：

![image-20200805205351413](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805205351413.png)





### 守护线程

![image-20200805205516689](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805205516689.png)

~~~JAVA
package com.ren.lesson03;

//测试守护线程
public class TestDaemon {

    public static void main(String[] args) {
        God god = new God();
        You you = new You();

        Thread thread = new Thread();
        thread.setDaemon(true);//默认是false表示的用户线程，正常的线程都是用户线程

        thread.start();//上帝守护线程启动

        new Thread(you).start();// 你 用户线程启动

    }
}

class God implements Runnable{

    @Override
    public void run() {
        while (true){
            System.out.println("1111111111111111111111111");
        }
    }
}


class You implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 36500; i++) {
            System.out.println("你一生都开心的活着！");
        }
        System.out.println("============goodbye  world==============");
    }
}
~~~

结果图：

![image-20200805211111872](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805211111872.png)





### 线程同步机制

![image-20200805212042020](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805212042020.png)

## 三大不安全案例

### 买票

~~~JAVA
package com.ren.lesson04;

//不安全的买票
//线程不安全，有负数
public class UnsafeBuyTicket {

    public static void main(String[] args) {
        BuyTicket station = new BuyTicket();

        new Thread(station,"A").start();
        new Thread(station,"B").start();
        new Thread(station,"C").start();
    }
}

class BuyTicket implements Runnable{

    //票数
    private int ticketNums = 10;
    //外部停止方式
    boolean flag = true;

    @Override
    public void run() {
        //买票
        while (flag){
            try {
                buy();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    private void buy() throws InterruptedException {
        //判断是否有票
        if(ticketNums<=0){
            flag=false;
            return ;
        }
        //模拟延时  放大问题的发生性
        Thread.sleep(100);
        //买票
        System.out.println(Thread.currentThread().getName()+"拿到"+ticketNums--);
    }
}
~~~

结果图：

![image-20200805231547263](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805231547263.png)



### 取钱

~~~JAVA
package com.ren.lesson04;

//不安全的取钱
//两个人去银行取钱，账户
public class UnsafeBank {
    public static void main(String[] args) {
        Account account = new Account(100,"结婚基金");

        Drawing you = new Drawing(account, 50,"你");
        Drawing girlFriend = new Drawing(account, 100,"girlFriend");

        you.start();
        girlFriend.start();

    }
}

class Account{
    //余额
    int money;
    //卡名
    String name;

    public Account(int money, String name) {
        this.money = money;
        this.name = name;
    }
}

//银行：模拟取款
class Drawing extends Thread{
    //账户
    Account account;
    //取了多少钱
    int drawingMoney;
    //现在手里有多少钱
    int nowMoney;
    public Drawing(Account account,int drawingMoney,String name){
        super(name);
        this.account = account;
        this.drawingMoney = drawingMoney;
    }

    //取钱
    @Override
    public void run() {
        //判断有没有钱
        if(account.money-drawingMoney<0){
            System.out.println(Thread.currentThread().getName()+"钱不够，取不了");
            return;
        }

        //延时，放大问题的发生性
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        //卡内余额 = 余额 - 你取的钱
        account.money = account.money - drawingMoney;
        //你手上的钱
        nowMoney = nowMoney + drawingMoney;

        System.out.println(account.name+"余额为："+account.money);
        //Thread.currentThread().getName()  等价于  this.getName()  因为继承了Thread
        System.out.println(this.getName()+"手里的钱："+nowMoney);
    }
}
~~~

结果图：

![image-20200805231645820](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805231645820.png)



### 不安全的集合

~~~JAVA
package com.ren.lesson04;

import java.util.ArrayList;
import java.util.List;

//线程不安全的集合
public class UnsafeList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(()->{
                list.add(Thread.currentThread().getName());
            }).start();
        }
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(list.size());
    }
}

~~~

结果图：

![image-20200805231754671](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805231754671.png)



## 同步方法

![image-20200805232035320](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805232035320.png)

~~~JAVA
package com.ren.lesson04;

//不安全的买票
//线程不安全，有负数
public class UnsafeBuyTicket {

    public static void main(String[] args) {
        BuyTicket station = new BuyTicket();

        new Thread(station,"A").start();
        new Thread(station,"B").start();
        new Thread(station,"C").start();
    }
}

class BuyTicket implements Runnable{

    //票数
    private int ticketNums = 10;
    //外部停止方式
    boolean flag = true;

    @Override
    public void run() {
        //买票
        while (flag){
            try {
                buy();
                //模拟延时  放大问题的发生性
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    //synchronized  同步方法，锁的是this
    private synchronized void buy() throws InterruptedException {
        //判断是否有票
        if(ticketNums<=0){
            flag=false;
            return ;
        }

        //买票
        System.out.println(Thread.currentThread().getName()+"拿到"+ticketNums--);
    }
}
~~~

### 关键点：

![image-20200805233800543](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805233800543.png)

结果图：

![image-20200805234021295](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805234021295.png)



 ### 同步块

![image-20200805232942804](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805232942804.png)

~~~JAVA
package com.ren.lesson04;

//不安全的取钱
//两个人去银行取钱，账户
public class UnsafeBank {
    public static void main(String[] args) {
        Account account = new Account(1000,"结婚基金");

        Drawing you = new Drawing(account, 50,"你");
        Drawing girlFriend = new Drawing(account, 100,"girlFriend");

        you.start();
        girlFriend.start();

    }
}

class Account{
    //余额
    int money;
    //卡名
    String name;

    public Account(int money, String name) {
        this.money = money;
        this.name = name;
    }
}

//银行：模拟取款
class Drawing extends Thread{
    //账户
    Account account;
    //取了多少钱
    int drawingMoney;
    //现在手里有多少钱
    int nowMoney;
    public Drawing(Account account,int drawingMoney,String name){
        super(name);
        this.account = account;
        this.drawingMoney = drawingMoney;
    }

    //取钱
    //synchronized 默认锁的是this
    @Override
    public void run() {

        //锁的对象就是变化的量，需要增、删、改的对象
        synchronized (account){
            //判断有没有钱
            if(account.money-drawingMoney<0){
                System.out.println(Thread.currentThread().getName()+"钱不够，取不了");
                return;
            }

            //延时，放大问题的发生性
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            //卡内余额 = 余额 - 你取的钱
            account.money = account.money - drawingMoney;
            //你手上的钱
            nowMoney = nowMoney + drawingMoney;

            System.out.println(account.name+"余额为："+account.money);
            //Thread.currentThread().getName()  等价于  this.getName()  因为继承了Thread
            System.out.println(this.getName()+"手里的钱："+nowMoney);
        }
    }
}
~~~

### 关键点：

![image-20200805233923330](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805233923330.png)

结果图：

![image-20200805233956509](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805233956509.png)



## CopyOnWriteArrayList

~~~JAVA
package com.ren.lesson04;

import java.util.concurrent.CopyOnWriteArrayList;

public class TestJUC {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(()->{
                list.add(Thread.currentThread().getName());
            }).start();
        }
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //打印是在主线程，操作list是在子线程，sleep是保证子线程执行完在执行主线程的打印

        System.out.println(list.size());
    }
}

~~~

结果图：

![image-20200805235739598](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200805235739598.png)



## 死锁

![image-20200806000011241](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806000011241.png)



### 死锁避免方法

![image-20200806002045304](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806002045304.png)

~~~JAVA
package com.ren.lesson05;

//死锁：多个线程互相抱着对方需要的资源，形成僵持
public class DeadLock {
    public static void main(String[] args) {
        Makeup g1 = new Makeup(0,"灰姑凉");
        Makeup g2 = new Makeup(1,"白雪公主");

        g1.start();
        g2.start();
    }
}

//口红
class Lipstick{

}

//镜子
class Mirror{

}

class Makeup extends Thread{

    //需要的资源只有一份，用static来保证只有一份
    static Lipstick lipstick = new Lipstick();
    static Mirror mirror = new Mirror();

    //选择
    int choice;
    //使用化妆品的人
    String girlName;

    public Makeup(int choice,String girlName){
        this.choice = choice;
        this.girlName = girlName;
    }

    @Override
    public void run() {
        try {
            makeup();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    //化妆，互相持有对方的锁，就是需要拿到对方的资源
    private void makeup() throws InterruptedException {
        if(choice == 0){
            /**
             * 这个地方解决的方法就是不让其一个人抱着两个人的资源，将其写到外面来
             */
            synchronized (lipstick){//获得口红的锁
                System.out.println(this.girlName+"获得口红的锁");
                Thread.sleep(1000);
            }
            synchronized (mirror){//一秒钟后想获得镜子的锁
                System.out.println(this.girlName+"获得镜子的锁");
            }
        }else{
            synchronized (mirror){//获得镜子的锁
                System.out.println(this.girlName+"获得镜子的锁");
                Thread.sleep(2000);
            }
            synchronized (lipstick){//两秒钟后想获得口红的锁
                System.out.println(this.girlName+"获得口红的锁");
            }
        }
    }
}
~~~

结果图：

![image-20200806002206009](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806002206009.png)





## Lock

### synchronized与Lock的对比

![image-20200806095653298](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806095653298.png)

~~~JAVA
package com.ren.lesson05;

import java.util.concurrent.locks.ReentrantLock;

public class TestLock {
    public static void main(String[] args) {
        TestLock2 testLock2 = new TestLock2();

        new Thread(testLock2).start();
        new Thread(testLock2).start();
        new Thread(testLock2).start();
    }
}

class TestLock2 implements Runnable{

    int ticketNums = 10;

    //定义lock锁
    private final ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true){
            try{
                //加锁
                lock.lock();
                if(ticketNums>0){
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(ticketNums--);
                }else{
                    break;
                }
            }finally {
                //解锁
                lock.unlock();
            }

        }
    }
}

~~~

结果图：

![image-20200806095737234](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806095737234.png)



## 生产者消费者问题

![image-20200806111203490](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806111203490.png)

### 管程法

~~~JAVA
package com.ren.lesson05;

//测试：生产者消费者模型-->利用缓冲区解决：管程法

//生产者，  消费者 ， 产品，  缓冲区
public class TestPC {
    public static void main(String[] args) {
        SynContainer container = new SynContainer();

        new Productor(container).start();
        new Consumer(container).start();
    }
}

//生产者
class Productor extends Thread{
    SynContainer container;

    public Productor(SynContainer container){
        this.container = container;
    }

    //生产
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            container.push(new Chicken(i));
            System.out.println("生产了第"+i+"只鸡");
        }
    }
}

//消费者
class Consumer extends Thread{
    SynContainer container;

    public Consumer(SynContainer container){
        this.container = container;
    }

    //消费

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("消费了第-->"+container.pop().id+"只鸡");
        }
    }
}

//产品
class Chicken{
    //产品编号
    int id;

    public Chicken(int id) {
        this.id = id;
    }
}

//缓冲区
class SynContainer{
    //需要一个容器大小
    Chicken[] chickens = new Chicken[10];

    //容器计数器
    int count = 0;

    //生产者放入产品
    public synchronized void push(Chicken chicken){
        //如果容器满了，就需要等待消费者消费产品
        if(count==chickens.length){
            //通知消费者生产，生产者等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //如果没有满，我们就需要丢入产品
        chickens[count] = chicken;
        count++;

        //可以通知消费者消费了
        this.notifyAll();
    }

    //消费者消费产品
    public synchronized Chicken pop(){
        //判断能否消费
        if(count == 0){
            //等待生产者生产，消费者等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //如果可以消费
        count--;
        Chicken chicken = chickens[count];

        //吃完了，通知生产者生产
        this.notifyAll();
        return chicken;
    }
}
~~~

结果图：

![image-20200806111248283](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806111248283.png)



### 信号灯法

~~~JAVA
package com.ren.lesson05;

//测试生产者消费者问题2：信号灯法，标志位解决
public class TestPC2 {
    public static void main(String[] args) {
        TV tv = new TV();

        new Player(tv).start();
        new Watcher(tv).start();
    }
}

//生产者-->演员
class Player extends Thread{
    TV tv;
    public Player(TV tv){
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            if(i%2==0){
                tv.play("快乐大本营播放中！");
            }else{
                tv.play("抖音记录美好生活");
            }
        }
    }
}

//消费者-->观众
class Watcher extends Thread{
    TV tv;
    public Watcher(TV tv){
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            tv.watch();
        }
    }
}

//产品-->节目
class TV{

    //演员表演，观众等待   T
    //观众观看，演员等待   F

    //表演的节目
    String voice;
    //标志位
    boolean flag = true;

    //演员表演
    public synchronized void play(String voice){

        if(!flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println("演员表演了："+voice);
        //通知观众观看
        this.notifyAll();//通知唤醒
        this.voice = voice;
        this.flag = !flag;
    }


    //观众观看
    public synchronized void watch(){
        if(flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("观众观看了："+voice);
        //通知演员表演
        this.notifyAll();
        this.flag = !flag;
    }
}
~~~

结果图：

![image-20200806113805848](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806113805848.png)



## 使用线程池

![image-20200806114037556](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806114037556.png)



![image-20200806114158442](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806114158442.png)



~~~JAVA
package com.ren.lesson05;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

//测试线程池
public class TestPool {
    public static void main(String[] args) {
        //1.创建服务，创建线程池
        //newFixedThreadPool  参数为：线程池大小
        ExecutorService service = Executors.newFixedThreadPool(10);

        //执行runnable的实现类
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());

        //2.关闭连接
        service.shutdown();
    }
}

class MyThread implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
~~~

结果图：

![image-20200806115129795](C:\Users\任作炜\AppData\Roaming\Typora\typora-user-images\image-20200806115129795.png)




