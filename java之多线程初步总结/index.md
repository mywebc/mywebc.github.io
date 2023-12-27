# Java之多线程初步总结

> 在java中，代码是阻塞同步执行的，为了更好的利用CPU资源，多线程的使用是十分有必要的，与之相关的，不得不提的还有一个进程，所谓进程即是代码在数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位。而线程是进程的一个执行路径，一个进程中至少有一个线程，进程中的多个线程共享进程资源；

## 并发与并行
* 并行：多个cpu实例或者多台机器同时执行一段处理逻辑，是真正的同时。
* 并发：通过cpu调度算法，让用户看上去同时执行，实际上从cpu操作层面不是真正的同时。

## 线程创建
* 在 Java 中使用 Thread 类代表线程，所有的线程对象都必须是 Thread 类或者其子类的实例，Java 中创建线程主要有以下三种方式：

### 继承 Thread 类
```java
public class CreateThreadByExtendsThread extends Thread {

  @Override
  public void run() {
    IntStream.rangeClosed(1, 10).forEach(i -> System.out.println(Thread.currentThread().getName() + " " + i));
  }
  public static void main(String[] args) {
    CreateThreadByExtendsThread threadOne = new CreateThreadByExtendsThread();
    threadOne.start();
  }

}
```
### 实现 Runnable 接口

```java
public class RunableTest implements Runnable {
    @Override
    public void run() {
        while (true) {
            System.out.println("good time");
        }
    }
    public static void main(String[] args) {
        RunableTest runableTest1 = new RunableTest();
        new Thread(runableTest1).start();
    }
}
```

### 实现 Callable 接口

```java
public class CallTest implements Callable {
    @Override
    public Object call() throws Exception {
        return "hello world";
    }
 
    public static void main(String[] args){
        FutureTask<String> futureTask = new FutureTask<String>(new CallTest());
        new Thread(futureTask).start();
        try {
            String result = futureTask.get();
            System.out.println(result);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```

## 多线程的优点
* 提高CPU的利用率
* 对于IO密集型极其有用（网络IO，文件IO）
* CPU性能100%的提升
* 程序设计更简单


## 多线程带来的问题
* 大量的线程,会影响性能,因为操作系统需要在它们之间切换
* 更多的线程需要更多的内存空间
* 线程中止需要考虑对程序运行的影响
* 通常块模型数据是在多个线程间共享的,需要防止线程死锁情况的发生


