---
layout: post
category : Java
tags : [Java]
title: Java多线程总结
wordpress_id: 1022
wordpress_url: http://www.im47.cn/?p=1022
date: 2011-10-18 13:20:03.000000000 +08:00
excerpt: 要理解线程首先需要了解一些基本的东西，我们现在所使用的大多数操作系统都属于多任务，分时操作系统。正是由于这种操作系统的出现才有了多线程这个概念。我们使用的windows,linux就属于此列。<br>什么是分时操作系统呢，通俗一点与就是可以同一时间执行多个程序的操作系统，在自己的电脑上面，你是不是一边听歌，一边聊天还一边看网页呢？但实际上，并不上cpu在同时执行这些程序，cpu只是将时间切割为时间片，然后将时间片分配给这些程序，获得时间片的程序开始执行，不等执行完毕，下个程序又获得时间片开始执行，这样多个程序轮流执行一段时间，由于现在cpu的高速计算能力，给人的感觉就像是多个程序在同时执行一样。
---
要理解线程首先需要了解一些基本的东西，我们现在所使用的大多数操作系统都属于多任务，分时操作系统。正是由于这种操作系统的出现才有了多线程这个概念。我们使用的windows,linux就属于此列。

什么是分时操作系统呢，通俗一点与就是可以同一时间执行多个程序的操作系统，在自己的电脑上面，你是不是一边听歌，一边聊天还一边看网页呢？但实际上，并不上cpu在同时执行这些程序，cpu只是将时间切割为时间片，然后将时间片分配给这些程序，获得时间片的程序开始执行，不等执行完毕，下个程序又获得时间片开始执行，这样多个程序轮流执行一段时间，由于现在cpu的高速计算能力，给人的感觉就像是多个程序在同时执行一样。

一般可以在同一时间内执行多个程序的操作系统都有进程的概念.一个进程就是一个执行中的程序,而每一个进程都有自己独立的一块内存空间,一组系统资源.在进程概念中,每一个进程的内部数据和状态都是完全独立的.因此可以想像创建并执行一个进程的系统开消是比较大的，所以线程出现了。

在java中，程序通过流控制来执行程序流,程序中单个顺序的流控制称为线程,多线程则指的是在单个程序中可以同时运行多个不同的线程,执行不同的任务.多线程意味着一个程序的多行语句可以看上去几乎在同一时间内同时运行.（你可以将前面一句话的程序换成进程，进程是程序的一次执行过程,是系统运行程序的基本单位）

线程与进程相似,是一段完成某个特定功能的代码,是程序中单个顺序的流控制;但与进程不同的是,同类的多个线程是共享一块内存空间和一组系统资源,而线程本身的数据通常只有微处理器的寄存器数据,以及一个供程序执行时使用的堆栈.所以系统在产生一个线程,或者在各个线程之间切换时,负担要比进程小的多,正因如此,线程也被称为轻负荷进程(light-weight process).一个进程中可以包含多个线程.

多任务是指在一个系统中可以同时运行多个程序,即有多个独立运行的任务,每个任务对应一个进程，同进程一样,一个线程也有从创建,运行到消亡的过程,称为线程的生命周期.用线程的状态(state)表明线程处在生命周期的哪个阶段.线程有**创建**, **可运行**, **运行中**, **阻塞**, **死亡**五种状态.通过线程的控制与调度可使线程在这几种状态间转化每个程序至少自动拥有一个线程,称为主线程.当程序加载到内存时,启动主线程.

## 线程的运行机制以及调度模型

java中多线程就是一个类或一个程序执行或管理多个线程执行任务的能力，每个线程可以独立于其他线程而独立运行，当然也可以和其他线程协同运行，一个类控制着它的所有线程，可以决定哪个线程得到优先级，哪个线程可以访问其他类的资源，哪个线程开始执行，哪个保持休眠状态。

下面是线程的机制图：

![/assets/images/2011/10/threads.jpg](/assets/images/2011/10/threads.jpg)

线程的状态表示线程正在进行的活动以及在此时间段内所能完成的任务.线程有创建,可运行,运行中,阻塞,死亡五中状态.一个具有生命的线程,总是处于这五种状态之一：

1. 创建状态
> 使用new运算符创建一个线程后,该线程仅仅是一个空对象,系统没有分配资源,称该线程处于创建状态(new thread)

2. 可运行状态
> 使用start()方法启动一个线程后,系统为该线程分配了除CPU外的所需资源,使该线程处于可运行状态(Runnable)

3. 运行中状态
> Java运行系统通过调度选中一个Runnable的线程,使其占有CPU并转为运行中状态(Running).此时,系统真正执行线程的run()方法.

4. 阻塞状态
> 一个正在运行的线程因某种原因不能继续运行时,进入阻塞状态(Blocked)

5. 死亡状态
> 线程结束后是死亡状态(Dead)

同一时刻如果有多个线程处于可运行状态,则他们需要排队等待CPU资源.此时每个线程自动获得一个线程的优先级(priority),优先级的高低反映线程的重要或紧急程度.可运行状态的线程按优先级排队,线程调度依据优先级基础上的"先到先服务"原则.

线程调度管理器负责线程排队和CPU在线程间的分配,并由线程调度算法进行调度.当线程调度管理器选种某个线程时,该线程获得CPU资源而进入运行状态.

线程调度是先占式调度,即如果在当前线程执行过程中一个更高优先级的线程进入可运行状态,则这个线程立即被调度执行.先占式调度分为:独占式和分时方式.

独占方式下,当前执行线程将一直执行下去,直 到执行完毕或由于某种原因主动放弃CPU,或CPU被一个更高优先级的线程抢占分时方式下,当前运行线程获得一个时间片,时间到时,即使没有执行完也要让出CPU,进入可运行状态,等待下一个时间片的调度.系统选中其他可运行状态的线程执行

分时方式的系统使每个线程工作若干步,实现多线程同时运行

另外请注意下面的线程调度规则（如果有不理解，不急，往下看）：

1. 如果两个或是两个以上的线程都修改一个对象，那么把执行修改的方法定义为被同步的（Synchronized）,如果对象更新影响到只读方法，那么只读方法也应该定义为同步的

2. 如果一个线程必须等待一个对象状态发生变化，那么它应该在对象内部等待，而不是在外部等待，它可以调用一个被同步的方法，并让这个方法调用wait()

3. 每当一个方法改变某个对象的状态的时候，它应该调用notifyAll()方法，这给等待队列的线程提供机会来看一看执行环境是否已发生改变

4. 记住wait(), notify(), notifyAll()方法属于Object类，而不是Thread类，仔细检查看是否每次执行wait()方法都有相应的notify()或notifyAll()方法，且它们作用与相同的对象 在java中每个类都有一个主线程，要执行一个程序，那么这个类当中一定要有main方法，这个main方法也就是java class中的主线程。你可以自己创建线程，有两种方法，一是继承Thread类，或是实现Runnable接口。一般情况下，最好避免继承，因为java中是单继承，如果你选用继承，那么你的类就失去了弹性，当然也不能全然否定继承Thread,该方法编写简单,可以直接操作线程,适用于单重继承情况。至于选用那一种，具体情况具体分析。

###### eg.继承Thread

    public class MyThread_1 extends Thread {
        public void run() {
            //some code
        }
    }

当使用继承创建线程，这样启动线程：

    new MyThread_1().start()
    
###### eg.实现Runnable接口

    public class MyThread_2 implements Runnable {
        public void run() {
            //some code
        }
    }

当使用实现接口创建线程，这样启动线程：

    new Thread(new MyThread_2()).start()

注意，其实是创建一个线程实例，并以实现了Runnable接口的类为参数传入这个实例，当执行这个线程的时候，MyThread_2中run里面的代码将被执行。
下面是完成的例子：

    public class MyThread implements Runnable { 
        public void run() {
            System.out.println("My Name is "+Thread.currentThread().getName());
        }
        public static void main(String[] args) {
            new Thread(new MyThread()).start();
        }
    }

执行后将打印出：

    My Name is Thread-0

你也可以创建多个线程，像下面这样

    new Thread(new MyThread()).start();
    new Thread(new MyThread()).start();
    new Thread(new MyThread()).start();

那么会打印出：

    My Name is Thread-0
    My Name is Thread-1
    My Name is Thread-2

看了上面的结果，你可能会认为线程的执行顺序是依次执行的，但是那只是一般情况，千万不要用以为是线程的执行机制；影响线程执行顺序的因素有几点：首先看看前面提到的优先级别

    public class MyThread implements Runnable { 
        public void run() {
            System.out.println("My Name is "+Thread.currentThread().getName());
        }
        public static void main(String[] args) {
            Thread t1=new Thread(new MyThread());
            Thread t2=new Thread(new MyThread());
            Thread t3=new Thread(new MyThread());
            t2.setPriority(Thread.MAX_PRIORITY);//赋予最高优先级
            t1.start();
            t2.start();
            t3.start();
        }
    }

再看看结果：

    My Name is Thread-1
    My Name is Thread-0
    My Name is Thread-2

线程的优先级分为10级，分别用1到10的整数代表，默认情况是5。上面的t2.setPriority(Thread.MAX_PRIORITY)等价与t2.setPriority(10）

然后是线程程序本身的设计，比如使用sleep,yield,join，wait等方法（详情请看JDKDocument)

    public class MyThread implements Runnable {
        public void run() {
            try {
                int sleepTime=(int)(Math.random()*100);//产生随机数字，
                Thread.currentThread().sleep(sleepTime);//让其休眠一定时间，时间又上面sleepTime决定
                //public static void sleep(long millis)throw InterruptedException （API）
                System.out.println(Thread.currentThread().getName()+" 睡了 "+sleepTime);
            }catch(InterruptedException ie) {//由于线程在休眠可能被中断，所以调用sleep方法的时候需要捕捉异常
                ie.printStackTrace();
            }
        }
        public static void main(String[] args) {
            Thread t1=new Thread(new MyThread());
            Thread t2=new Thread(new MyThread());
            Thread t3=new Thread(new MyThread());
            t1.start();
            t2.start();
            t3.start();
        }
    }

执行后观察其输出：

    Thread-0 睡了 11
    Thread-2 睡了 48
    Thread-1 睡了 69

上面的执行结果是随机的，再执行很可能出现不同的结果。由于上面我在run中添加了休眠语句，当线程休眠的时候就会让出cpu，cpu将会选择执行处于runnable状态中的其他线程，当然也可能出现这种情况，休眠的Thread立即进入了runnable状态，cpu再次执行它。

## 线程组概念

线程是可以被组织的，java中存在线程组的概念，每个线程都是一个线程组的成员,线程组把多个线程集成为一个对象,通过线程组可以同时对其中的多个线程进行操作,如启动一个线程组的所有线程等.Java的线程组由java.lang包中的Thread——Group类实现.

ThreadGroup类用来管理一组线程,包括:线程的数目,线程间的关系,线程正在执行的操作,以及线程将要启动或终止时间等.线程组还可以包含线程组.在Java的应用程序中,最高层的线程组是名位main的线程组,在main中还可以加入线程或线程组,在mian的子线程组中也可以加入线程和线程组,形成线程组和线程之间的树状继承关系。像上面创建的线程都是属于main这个线程组的。

借用上面的例子，main里面可以这样写：

    public static void main(String[] args) {
        /***************************************
        ThreadGroup(String name)
        ThreadGroup(ThreadGroup parent, String name)
        ***********************************/
        ThreadGroup group1=new ThreadGroup("group1");
        ThreadGroup group2=new ThreadGroup(group1,"group2");
        Thread t1=new Thread(group2,new MyThread());
        Thread t2=new Thread(group2,new MyThread());
        Thread t3=new Thread(group2,new MyThread());
        t1.start();
        t2.start();
        t3.start();
    }

线程组的嵌套，t1,t2,t3被加入group2,group2加入group1。

另外一个比较多就是关于线程同步方面的，试想这样一种情况，你有一笔存款在银行，你在一家银行为你的账户存款，而你的妻子在另一家银行从这个账户提款，现在你有1000块在你的账户里面。你存入了1000，但是由于另一方也在对这笔存款进行操作，人家开始执行的时候只看到账户里面原来的1000元，当你的妻子提款1000元后，你妻子所在的银行就认为你的账户里面没有钱了，而你所在的银行却认为你还有2000元。

看看下面的例子：

    class BlankSaving {//储蓄账户
        private static int money=10000;
        public void add(int i) {
            money=money+i;
            System.out.println("Husband 向银行存入了 [￥"+i+"]");
        }
        public void get(int i) {
            money=money-i;
            System.out.println("Wife 向银行取走了 [￥"+i+"]");
            if(money&lt;0)
            System.out.println("余额不足！");
        }
        public int showMoney() {
            return money;
        }
    } 

    class Operater implements Runnable {
        String name;
        BlankSaving bs;
        public Operater(BlankSaving b,String s) {
            name=s;
            bs=b;        
        }
        public static void oper(String name,BlankSaving bs) {
            if(name.equals("husband")) {
                try {
                    for(int i=0;i&lt;10;i++) {
                        Thread.currentThread().sleep((int)(Math.random()*300));
                        bs.add(1000);
                    }
                }catch(InterruptedException e){}
            }else {
                try {                
                    for(int i=0;i&lt;10;i++) {
                        Thread.currentThread().sleep((int)(Math.random()*300));
                        bs.get(1000);
                    }
                }catch(InterruptedException e){}
            }
        }
        public void run() {
        oper(name,bs);
        }
    }

    public class BankTest {
        public static void main(String[] args)throws InterruptedException {
            BlankSaving bs=new BlankSaving();
            Operater o1=new Operater(bs,"husband");
            Operater o2=new Operater(bs,"wife");
            Thread t1=new Thread(o1);
            Thread t2=new Thread(o2);
            t1.start();
            t2.start();
            Thread.currentThread().sleep(500);
        }
    }

下面是其中一次的执行结果：

    ---------first--------------
    Husband 向银行存入了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Husband 向银行存入了 [￥1000]

看到了吗，这可不是正确的需求，在husband还没有结束操作的时候，wife就插了进来，这样很可能导致意外的结果。解决办法很简单，就是将对数据进行操作方法声明为synchronized,当方法被该关键字声明后，也就意味着，如果这个数据被加锁，只有一个对象得到这个数据的锁的时候该对象才能对这个数据进行操作。也就是当你存款的时候，这笔账户在其他地方是不能进行操作的，只有你存款完毕，银行管理人员将账户解锁，其他人才能对这个账户进行操作。

修改public static void oper(String name,BlankSaving bs)为public static void oper(String name,BlankSaving bs)，再看看结果:

    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Husband 向银行存入了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]
    Wife 向银行取走了 [￥1000]

当丈夫完成操作后，妻子才开始执行操作，这样的话，对共享对象的操作就不会有问题了。

## wait and notify

你可以利用这两个方法很好的控制线程的执行流程，当线程调用wait方法后，线程将被挂起，直到被另一线程唤醒（notify）或则是如果wait方法指定有时间得话，在没有被唤醒的情况下，指定时间时间过后也将自动被唤醒。但是要注意一定，被唤醒并不是指马上执行，而是从组塞状态变为可运行状态，其是否运行还要看cpu的调度。

事例代码：

    class MyThread_1 extends Thread {
        Object lock;
        public MyThread_1(Object o) {
            lock=o;
        }
        public void run() {
            try {
                synchronized(lock) {
                    System.out.println("Enter Thread_1 and wait");
                    lock.wait();
                    System.out.println("be notified");
                }
            }catch(InterruptedException e){}
        }
    }
        
    class MyThread_2 extends Thread {
        Object lock;
        public MyThread_2(Object o) {
            lock=o;
        }
        public void run() {
            synchronized(lock) {
                System.out.println("Enter Thread_2 and notify");
                lock.notify();
            }
        }
    }
    
    public class MyThread {
        public static void main(String[] args) {
            int[] in=new int[0];//notice
            MyThread_1 t1=new MyThread_1(in);
            MyThread_2 t2=new MyThread_2(in);
            t1.start();
            t2.start();
        }
    }

执行结果如下：

    Enter Thread_1 and wait
    Enter Thread_2 and notify
    Thread_1 be notified

可能你注意到了在使用wait and notify方法得时候我使用了synchronized块来包装这两个方法，这是由于调用这两个方法的时候线程必须获得锁，也就是上面代码中的lock[]，如果你不用synchronized包装这两个方法的得话，又或则锁不一是同一把，比如在MyThread_2中synchronized(lock)改为synchronized(this),那么执行这个程序的时候将会抛出java.lang.IllegalMonitorStateException执行期异常。另外wait and notify方法是Object中的，并不在Thread这个类中。最后你可能注意到了这点：int[] in=new int[0];为什么不是创建new Object而是一个0长度的数组，那是因为在java中创建一个0长度的数组来充当锁更加高效。

Thread作为java中一重要组成部分，当然还有很多地方需要更深刻的认识，上面只是对Thread的一些常识和易错问题做了一个简要的总结，若要真正的掌握java的线程，还需要自己多做总结。