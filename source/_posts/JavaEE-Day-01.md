---
title: JavaEE-Day-01
categories: 学习
tags: JavaSe
---

### 封装

私有

get/set

### 继承

子类继承父类，会获得父类所有方法

### super

调用父类方法

## 多线程

### 多线程的创建和使用

- 继承Thread类创建

```java
public class dem01 {
    public static void main(String[] args) {
        test t1=new test();
        test t2=new test();
        t1.start();                           //start启动
        t2.start();


    }
}
class test extends Thread{                      //继承Thread类

    @Override
    public void run() {                         //重写run方法
        for (int i=1;i<1000;i++){
            System.out.println(i);
        }
    }
}

```

实例--卖票

```java
public class demo3 {
    public static void main(String[] args) {
        mp mp=new mp();
        mp mp1=new mp();
        mp mp2=new mp();
        mp.setName("1");
        mp1.setName("2");
        mp2.setName("3");
        mp.start();
        mp1.start();
        mp2.start();

    }
    static class mp extends Thread{
        private static int num=100;                     //static !!全局变量
        @Override
        public void run() {
            while (true){
                if (num>0){
                    System.out.println(getName()+"号窗口正在卖第"+num+"张票");
                    num--;
                }else {
                    break;
                }
            }
        }
    }
}
```

- 实现runnable接口创建

```java
public class demo4 {
    public static void main(String[] args) {

        my my =new my();
        Thread thread = new Thread(my);
        thread.start();

    }
}
class my implements Runnable{
    @Override
    public void run() {
        for (int i=1;i<100;i++){
            System.out.println(i);
        }
    }
}
```

实例--卖票

```java
public class demo5 {
    public static void main(String[] args) {
        mp mp=new mp();
        Thread thread = new Thread(mp);
        Thread thread1 = new Thread(mp);
        Thread thread2 = new Thread(mp);
        thread.setName("1");
        thread1.setName("2");
        thread2.setName("3");
        thread.start();
        thread1.start();
        thread2.start();

    }
}
class mp implements Runnable{
    static int num=100;
    @Override
    public void run() {
        while (true){
            if (num>0){
                System.out.println(Thread.currentThread().getName()+"窗口正在卖"+num+"张票");
                num--;
            }else {
                break;
            }
        }
    }
}
```



- Thread常用的方法

```java
getName()           //获取线程名称              Thread.currentThread().getName()
setName()           //设置线程名称              Thread.setName("线程1");
join()              //堵塞线程                  Thread.join()
boolean isAlive()   //判断线程是否存活           Thread.isAlive()
sleep()             //线程睡眠                  sleep()毫秒

```

### 线程的优先级

```java
Thread.getPriority()                          //获取线程的优先级
Thread.setPriority()                          //设置线程的优先级
    
//MAX_PRIORITY   MIN_PRIORITY   NORM_PRIORITY(默认)
```

### 线程安全

- 同步锁

```java
public class demo5 {
    public static void main(String[] args) {
        mp mp=new mp();
        Thread thread = new Thread(mp);
        Thread thread1 = new Thread(mp);
        Thread thread2 = new Thread(mp);
        thread.setName("1");
        thread1.setName("2");
        thread2.setName("3");
        thread.start();
        thread1.start();
        thread2.start();

    }
}
class mp implements Runnable{
    static int num=100;
    Object object =new Object();                       //对象
    @Override
    public void run() {
        while (true){
            synchronized (object) {                   //锁
                if (num > 0) {
                    System.out.println(Thread.currentThread().getName() + "窗口正在卖" + num + "张票");
                    num--;
                } else {
                    break;
                }
            }
        }
    }
}

```

- 同步方法--实现解决runnable接口安全问题

```java
public class dem06 {
    public static void main(String[] args) {
        paip paip =new paip();
        Thread thread = new Thread(paip);
        Thread thread1 = new Thread(paip);
        Thread thread2 = new Thread(paip);
        thread.setName("1");
        thread1.setName("2");
        thread2.setName("3");
        thread.start();
        thread1.start();
        thread2.start();

    }

}
class paip implements Runnable{
    private int p =100;

    @Override
    public void run() {
        while (true) {
            sgow();
        }
    }
    private synchronized void sgow(){
        if (p > 0) {
            System.out.println(p);
            p--;
        }
    }
}
```

- 同步方法--实现解决继承Thrend类安全问题

```java
public class demo3 {
    public static void main(String[] args) {
        mp mp=new mp();
        mp mp1=new mp();
        mp mp2=new mp();
        mp.setName("1");
        mp1.setName("2");
        mp2.setName("3");
        mp.start();
        mp1.start();
        mp2.start();

    }
    static class mp extends Thread {
        private static int num = 100;
        private static Object o =new Object();

        @Override
        public void run() {
            while (true) {
                show();
            }
        }
        private synchronized static void show(){
            if (num > 0) {
                System.out.println(Thread.currentThread().getName() + "号窗口正在卖第" + num + "张票");
                num--;
            }
        }
    }
}

```

