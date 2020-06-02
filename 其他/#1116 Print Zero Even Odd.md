[toc]

Suppose you are given the following code:

```java
class ZeroEvenOdd {
  public ZeroEvenOdd(int n) { ... }      // constructor
  public void zero(printNumber) { ... }  // only output 0's
  public void even(printNumber) { ... }  // only output even numbers
  public void odd(printNumber) { ... }   // only output odd numbers
}
```

The same instance of `ZeroEvenOdd` will be passed to three different threads:

* Thread A will call `zero()` which should only output 0's.
* Thread B will call `even()` which should only ouput even numbers.
* Thread C will call `odd()` which should only output odd numbers.

Each of the threads is given a `printNumber` method to output an integer. Modify the given program to output the series `010203040506...` where the length of the series must be 2n.



## 题目解读

&emsp;线程交替打印数字。

```java
class ZeroEvenOdd {
    private int n;
    
    public ZeroEvenOdd(int n) {
        this.n = n;
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void zero(IntConsumer printNumber) throws InterruptedException {
        
    }

    public void even(IntConsumer printNumber) throws InterruptedException {
        
    }

    public void odd(IntConsumer printNumber) throws InterruptedException {
        
    }
}
```

## 程序设计

* 最基本的思路使用独占索及条件队列。

```java
class ZeroEvenOdd {
    private int n;

    // 是否该打印0
    private boolean flag = true;
    // 是否打印奇数、偶数
    private boolean oddOrEven = true;
    // 独占锁
    private ReentrantLock lock = new ReentrantLock();
    private Condition zero = lock.newCondition();
    private Condition odd = lock.newCondition();
    private Condition even = lock.newCondition();
    
    public ZeroEvenOdd(int n) {
        this.n = n;
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void zero(IntConsumer printNumber) throws InterruptedException {
        if (lock.tryLock()) {
            try {
                for (int i = 0; i < n; i++) {
                    // 未轮到打印0，休眠
                    while (!flag) zero.await();
                    
                    printNumber.accept(0);
                    flag = !flag;
                    // 根据奇偶数唤醒
                    if (oddOrEven) odd.signalAll();
                    else even.signalAll();
                }
            } finally {
                lock.unlock();
            }
        }
    }

    public void even(IntConsumer printNumber) throws InterruptedException {
        if (lock.tryLock()) {
            try {
                for (int i = 2; i <= n; i += 2) {
                    while (flag || oddOrEven) even.await();

                    printNumber.accept(i);
                    oddOrEven = !oddOrEven;
                    flag = !flag;
                    // 唤醒打印0
                    zero.signalAll();
                }
            } finally {
                lock.unlock();
            }
        }
    }

    public void odd(IntConsumer printNumber) throws InterruptedException {
        if (lock.tryLock()) {
            try {
                for (int i = 1; i <= n; i += 2) {
                    while (flag || !oddOrEven) odd.await();

                    printNumber.accept(i);
                    oddOrEven = !oddOrEven;
                    flag = !flag;
                    // 唤醒打印0
                    zero.signalAll();
                }
            } finally {
                lock.unlock();
            }
        }
    }
}
```

## 性能分析

执行用时：8ms，在所有java提交中击败了99.86%的用户。

内存消耗：39.1MB，在所有java提交中击败了5.41%的用户。

## 官方解题

&emsp;暂无，密切关注。社区思路采用信号量，实现更为简洁优雅。

```java
class ZeroEvenOdd {
    private int n;

    Semaphore zero = new Semaphore(1);
    Semaphore odd = new Semaphore(0);
    Semaphore even = new Semaphore(0);

    public ZeroEvenOdd(int n) {
        this.n = n;
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void zero(IntConsumer printNumber) throws InterruptedException {
        for (int i = 1; i <= n; i++) {
            zero.acquire();
            printNumber.accept(0);
            if (i % 2 == 1) odd.release();
            else even.release();
        }
    }

    public void even(IntConsumer printNumber) throws InterruptedException {
        for (int i = 2; i <= n; i += 2) {
            even.acquire();
            printNumber.accept(i);
            zero.release();
        }
    }

    public void odd(IntConsumer printNumber) throws InterruptedException {
        for (int i = 1; i <= n; i += 2) {
            odd.acquire();
            printNumber.accept(i);
            zero.release();
        }
    }
}
```

执行用时：8ms，在所有java提交中击败了99.86%的用户。

内存消耗：39.3MB，在所有java提交中击败了5.41%的用户。