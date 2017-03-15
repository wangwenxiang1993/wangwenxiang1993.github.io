---
layout: post
title: ThreadLocal源码解析
date: 2017/03/09
tags: [note, index]
---

## ThreadLocal是什么
##### This class provides thread-local variables.  These variables differ from their normal counterparts in that each thread that accesses one (via its <tt>get</tt> or <tt>set</tt> method) has its own, independently initialized copy of the variable.  <tt>ThreadLocal</tt> instances are typically private static fields in classes that wish to associate state with a thread (e.g., a user ID or Transaction ID).
<!--more-->

### 这是ThreadLocal源码中一段介绍，翻译过来的大概意思是：
##### ThreadLocal类用来提供线程内部的局部变量。这种变量在多线程环境下访问(通过get或set方法访问)时能保证各个线程里的变量相对独立于其他线程内的变量。ThreadLocal实例通常来说都是private static类型的，用于关联线程和线程的上下文。

##### 这种解释和网络上所说的解决多线程的并发问题不太一样，当你把一个变量复制成了多份，我觉得这种方式并不叫解决多线程访问资源时的共享问题。本质上来说资源已经变成了多份，没有共享之说，当然这也是一种解决方式，但是如果资源不必线程共享，又何必要用ThreadLocal去实现呢。

#### 总结来说：ThreadLocal的作用是提供线程内的局部变量，这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度。
##### 举一个例子，一个请求需要按照1、2、3这个顺序执行3个函数。但是其中有一个参数是需要函数1提供给函数3使用的，但是这个参数对于函数2来说并没有作用（实际情况可能比这要复杂的多）。同时我们又不希望污染函数2，那么如何将该参数通过函数1传递到函数3呢？`ThreadLocal`是一个比较合理的解决方法。

## ThreadLocal代码演示

```JAVA
/**
 * Created BY wangwenxiang on 2017/3/15.
 */
public class ThreadLocalTest {
    private static ThreadLocal<Integer> integerThreadLocal = new ThreadLocal<Integer>();

    /**
     * 初始化并输出每个线程的ThreadLocal
     * Runnable
     * name
     */
    private static Thread[] initThread(){
        Thread[] threads = new Thread[10];
        for (int i=0;i<10;i++){
            final int finalI = i;
            threads[i] = new Thread(new Runnable() {
                public void run() {
                    integerThreadLocal.set(finalI);
                    System.out.println(Thread.currentThread().getName()+": "+integerThreadLocal.get());
                }
            },"thread-" + i);
        }
        return threads;
    }


    public static void main(String[] args) {
        Thread[] threads = initThread();
        for (Thread thread : threads){
            thread.start();
        }
    }
}
```

##### `ThreadLocal`的操作非常简单，基本上没有太多的过程。

##### 执行结果：
```
thread-0: 0
thread-1: 1
thread-2: 2
thread-3: 3
thread-4: 4
thread-5: 5
thread-6: 6
thread-7: 7
thread-8: 8
thread-9: 9
```

##### 可以看出虽然只创建了一个`ThreadLocal`对象，但是每个线程的初始化之后，得出来的结果并不一样。各个线程之间的值是相互独立的。

## ThreadLocal源码
### 构造函数
```java
    /**
     * Creates a thread local variable.
     */
    public ThreadLocal() {
    }
```
##### 这是`ThreadLocal`唯一的构造函数，什么也没做



