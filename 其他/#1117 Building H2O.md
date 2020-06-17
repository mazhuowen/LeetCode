[toc]

There are two kinds of threads, `oxygen` and `hydrogen`. Your goal is to group these threads to form water molecules. There is a barrier where each thread has to wait until a complete molecule can be formed. Hydrogen and oxygen threads will be given `releaseHydrogen` and `releaseOxygen` methods respectively, which will allow them to pass the barrier. These threads should pass the barrier in groups of three, and they must be able to immediately bond with each other to form a water molecule. You must guarantee that all the threads from one molecule bond before any other threads from the next molecule do.

In other words:

* If an oxygen thread arrives at the barrier when no hydrogen threads are present, it has to wait for two hydrogen threads.
* If a hydrogen thread arrives at the barrier when no other threads are present, it has to wait for an oxygen thread and another hydrogen thread.

We don’t have to worry about matching the threads up explicitly; that is, the threads do not necessarily know which other threads they are paired up with. The key is just that threads pass the barrier in complete sets; thus, if we examine the sequence of threads that bond and divide them into groups of three, each group should contain one oxygen and two hydrogen threads.

Write synchronization code for oxygen and hydrogen molecules that enforces these constraints.



**Constraints**:

* Total length of input string will be $3n$, where $1 \le n \le 20$.
* Total number of `H` will be $2n$ in the input string.
* Total number of `O` will be $n$ in the input string.



## 题目解读

&emsp;设计协调多个线程，使得每次打印两个`H`一个`O`。

```java
class H2O {

    public H2O() {
        
    }

    public void hydrogen(Runnable releaseHydrogen) throws InterruptedException {
		
        // releaseHydrogen.run() outputs "H". Do not change or remove this line.
        releaseHydrogen.run();
    }

    public void oxygen(Runnable releaseOxygen) throws InterruptedException {
        
        // releaseOxygen.run() outputs "O". Do not change or remove this line.
		releaseOxygen.run();
    }
}
```

## 程序设计

* 使用信号量，为了节省判断，`O`的信号量设为$2$。

```java
class H2O {
    Semaphore H2;
    Semaphore O;

    public H2O() {
        this.H2 = new Semaphore(0);
        this.O = new Semaphore(2);
    }

    public void hydrogen(Runnable releaseHydrogen) throws InterruptedException {
        H2.acquire();
        // releaseHydrogen.run() outputs "H". Do not change or remove this line.
        releaseHydrogen.run();
        O.release();
    }

    public void oxygen(Runnable releaseOxygen) throws InterruptedException {
        O.acquire(2);
        // releaseOxygen.run() outputs "O". Do not change or remove this line.
		releaseOxygen.run();
        H2.release(2);
    }
}
```

## 性能分析

执行用时：19ms，在所有java提交中击败了51.49%的用户。

内存消耗：41.3MB，在所有java提交中击败了14.29%的用户。

## 官方解题

&emsp;暂无，密切关注。