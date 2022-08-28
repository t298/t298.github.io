---
layout:     post   				    # 使用的布局（不需要改）
title: java如何实现多线程？			# 标题 
date:       2021-04-21			# 时间
author:     t298						# 作者
header-img: img/wallhaven-4grwgd.png 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - java
    - 多线程
---

java实现多线程差不多有4种方法：

1. 继承Thread类，重写run方法。

2. 实现Runnable接口，重写run方法。

3. 匿名内部类实现多线程。

4. 通过线程池创建线程。

5. 定时器实现多线程。

6. 使用Spring来实现多线程。

   

方法一：

```java
public class hy extends Thread {
    //重写Thread的run()
    @Override
    public void run() {
        System.out.println(getName()+"  is running...");
    }
    public static void main(String[] args) {
        hy hy1 = new hy();
        hy hy2 = new hy();

        hy1.start();
        hy2.start();

    }
}
```



方法二：使用了面向接口，将任务与线程进行分离，有利于解偶。

```java
public class hy implements Runnable {
    //重写Runnable的run()
    @Override
    public void run() {
        System.out.println("Runnable  is running...");
    }
    public static void main(String[] args) {
        Thread thread1 = new Thread(new hy());
        Thread thread2 = new Thread(new hy());

        thread1.start();
        thread2.start();

    }
}
```

方法三：

```java
public class hy {
    public static void main(String[] args) {
        new Thread(){
            @Override
            public void run() {
                System.out.println("匿名内部类创建的线程1");
            };
        }.start();
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("匿名内部类创建的线程2");
            }
        }).start();
    }
   
```

方法四：

```
public class hy {
    public static void main(String[] args) {
        //创建带有5个线程的线程池
        ExecutorService threadpool = Executors.newFixedThreadPool(5);
        for(int i = 0;i < 10;i++){
            threadpool.execute(new Runnable() {
                @Override
                public void run() {
                    System.out.println(Thread.currentThread().getName()+"  is running");
                }
            });
        }
        threadpool.shutdown();

    }
    }
```

方法五：

```java
public class hy{
    public static void main(String[] args) {
        Timer timer = new Timer();
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println("执行任务");
            }
            //延迟0秒，每次间隔1000ms执行一次
        },0,1000);
    }
}
```

方法六：

只需要在配置类中添加@EnableAsync就可以使用多线程。在希望执行的并发方法中使用@Async就可以定义一个线程任务。

