# Deadlock

------

##1 Description
死锁就是两个或者多个进程，互相请求对方占有的资源。死锁的存在需要满足四要素：


> * **互斥条件:** 一个资源每次只能被一个进程使用
> * **请求与保持条件:** 一个进程因请求资源而阻塞时，对已获得的资源保持不放

> * **不剥夺条件:** 进程已获得的资源，在末使用完之前，不能强行剥夺
> * **循环等待条件:** 若干进程之间形成一种头尾相接的循环等待资源关系



------

## 2 Experiment Result
![](https://raw.githubusercontent.com/JLHins/src/master/Deadlock.png)
#####结果说明：循环运行到第23次时发生死锁

---
## 3 结果分析
### 3.1 A、B类的定义如下
```java
class A{
    synchronized void methodA(B b){
        b.last();
    }
    synchronized void last(){
        System.out.println("Inside A.last()");
    }
}


class B{
    synchronized void methodB(A a){
        a.last();
    }
    synchronized void last(){
        System.out.println("Inside B.last()");
    }
}
```
>* 关键字  synchronized:
>当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。
当一个线程访问object的一个synchronized同步代码块或同步方法时，其他线程对object中所有其它synchronized同步代码块或同步方法的访问将被阻塞。
###3.2 主类Deadlock定义
```java
public class Deadlock implements Runnable{
    A a=new A();
    B b=new B();
    int cnt=0;
    Deadlock(){
        Thread t = new Thread(this);
        t.start();
        while(cnt++<=145000);
        a.methodA(b);
    }
    public void run(){
        b.methodB(a);
    }
    public static void main(String args[]){
        new Deadlock();
    }
}
```
####分析：
>利用同步关键字来实现死锁：Deadlock对象首先被初始化，紧接着新建线程t，t执行的任务是调用B类对象b的methodB（）；while语句来控制主线程调用A类对象a的methodA方法的时间。由于synchronized关键字的特性使然，在主线程调用了A类方法之后且未执行结束期间，t不能调用A类方法；同理t线程调用B类方法后未结束前，主类调用B方法会被阻塞。调整cnt值，使得在t线程调用了b.methodB()后且调用a.last()前的这段时间内主线程调用a.methodA()，这样两个线程都无法调用last函数，发生死锁。
要得到cnt的合适值，只需记录函数调用之间的cnt差值即可观察得出。
