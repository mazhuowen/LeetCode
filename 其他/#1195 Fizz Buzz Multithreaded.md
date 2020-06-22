[toc]

Write a program that outputs the string representation of numbers from $1$ to $n$, however:

* If the number is divisible by 3, output "fizz".
* If the number is divisible by 5, output "buzz".
* If the number is divisible by both 3 and 5, output "fizzbuzz".

For example, for $n = 15$, we output: `1, 2, fizz, 4, buzz, fizz, 7, 8, fizz, buzz, 11, fizz, 13, 14, fizzbuzz`.

```java
Suppose you are given the following code:

class FizzBuzz {
  public FizzBuzz(int n) { ... }               // constructor
  public void fizz(printFizz) { ... }          // only output "fizz"
  public void buzz(printBuzz) { ... }          // only output "buzz"
  public void fizzbuzz(printFizzBuzz) { ... }  // only output "fizzbuzz"
  public void number(printNumber) { ... }      // only output the numbers
}
```

Implement a multithreaded version of `FizzBuzz` with **four** threads. The same instance of `FizzBuzz` will be passed to four different threads:

* Thread A will call `fizz()` to check for divisibility of 3 and outputs `fizz`.
* Thread B will call `buzz()` to check for divisibility of 5 and outputs `buzz`.
* Thread C will call `fizzbuzz()` to check for divisibility of 3 and 5 and outputs `fizzbuzz`.
* Thread D will call `number()` which should only output the numbers.



## 题目解读

&emsp;设计多线程对数字的读写。

```java
class FizzBuzz {
    private int n;

    public FizzBuzz(int n) {
        this.n = n;
    }

    // printFizz.run() outputs "fizz".
    public void fizz(Runnable printFizz) throws InterruptedException {
        
    }

    // printBuzz.run() outputs "buzz".
    public void buzz(Runnable printBuzz) throws InterruptedException {
        
    }

    // printFizzBuzz.run() outputs "fizzbuzz".
    public void fizzbuzz(Runnable printFizzBuzz) throws InterruptedException {
        
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void number(IntConsumer printNumber) throws InterruptedException {
        
    }
}
```

## 程序设计

* 采用独占锁通信机制，互相打印。
* 需注意`while (count <= n && (count % 3 != 0 || count % 5 == 0))`循环中的条件`count <= n`，不加则会出现部分线程完成打印退出，剩余线程由于数字不满足条件，同时没有线程唤醒，导致死循环。

```java
class FizzBuzz {
    private int count;
    private int n;
    Lock lock = new ReentrantLock();
    Condition fizz = lock.newCondition();
    Condition buzz = lock.newCondition();
    Condition fizzbuzz = lock.newCondition();
    Condition num = lock.newCondition();

    public FizzBuzz(int n) {
        this.count = 1;
        this.n = n;
    }

    // printFizz.run() outputs "fizz".
    public void fizz(Runnable printFizz) throws InterruptedException {
        lock.lock();

        try {
            while (count <= n) {

                while (count <= n && (count % 3 != 0 || count % 5 == 0)) fizz.await();

                if (count <= n) printFizz.run();
                count++;

                buzz.signalAll();
                fizzbuzz.signalAll();
                num.signalAll();
            }
        } finally {
            lock.unlock();
        }
    }

    // printBuzz.run() outputs "buzz".
    public void buzz(Runnable printBuzz) throws InterruptedException {
        lock.lock();

        try {
            while (count <= n) {

                while (count <= n && (count % 5 != 0 || count % 3 == 0)) buzz.await();

                if (count <= n) printBuzz.run();
                count++;

                fizz.signalAll();
                fizzbuzz.signalAll();
                num.signalAll();
            }
        } finally {
            lock.unlock();
        }
    }

    // printFizzBuzz.run() outputs "fizzbuzz".
    public void fizzbuzz(Runnable printFizzBuzz) throws InterruptedException {
        lock.lock();

        try {
            while (count <= n) {

                while (count <= n && (count % 3 != 0 || count % 5 != 0)) fizzbuzz.await();

                if (count <= n) printFizzBuzz.run();
                count++;

                fizz.signalAll();
                buzz.signalAll();
                num.signalAll();
            }
        } finally {
            lock.unlock();
        }
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void number(IntConsumer printNumber) throws InterruptedException {
        lock.lock();

        try {
            while (count <= n) {

                while (count <= n && (count % 3 == 0 || count % 5 == 0)) num.await();

                if (count <= n) printNumber.accept(count++);

                fizz.signalAll();
                buzz.signalAll();
                fizzbuzz.signalAll();
            }
        } finally {
            lock.unlock();
        }
    }
}
```

> 测试代码如下：
>
> ```java
> public class Test {
>     public static void main(String[] args) {
>         FizzBuzz f = new FizzBuzz(15);
> 
>         Thread t1 = new Thread(() -> {
>             try {
>                 f.fizz(() -> System.out.println("fizz"));
>             } catch (InterruptedException e) {
>                 e.printStackTrace();
>             }
>         });
>         
>         Thread t2 = new Thread(() -> {
>             try {
>                 f.buzz(() -> System.out.println("buzz"));
>             } catch (InterruptedException e) {
>                 e.printStackTrace();
>             }
>         });
>         
>         Thread t3 = new Thread(() -> {
>             try {
>                 f.fizzbuzz(() -> System.out.println("fizzbuzz"));
>             } catch (InterruptedException e) {
>                 e.printStackTrace();
>             }
>         });
>         
>         Thread t4 = new Thread(() -> {
>             try {
>                 f.number(System.out::println);
>             } catch (InterruptedException e) {
>                 e.printStackTrace();
>             }
>         });
> 
>         t1.start();
>         t2.start();
>         t3.start();
>         t4.start();
>     }
> }
> ```

## 性能分析

执行用时：8ms，在所有java提交中击败了99.19%的用户。

内存消耗：39.3MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。上述设计过于复杂，本质上某一时刻只能有一个打印，可使用同步锁来实现。

```java
class FizzBuzz {
    private int n;
    private int i;
    private Object lock = new Object();

    public FizzBuzz(int n) {
        this.n = n;
        this.i = 1;
    }

    // printFizz.run() outputs "fizz".
    public void fizz(Runnable printFizz) throws InterruptedException {
        while (i <= n) {
            synchronized(lock) {
                while (i <= n && i % 3 == 0 && i % 5 != 0) {
                    printFizz.run();
                    i++; 
                }
            }
        }
        
    }

    // printBuzz.run() outputs "buzz".
    public void buzz(Runnable printBuzz) throws InterruptedException {
        while (i <= n) {
            synchronized(lock) {
                while (i <= n && i % 3 != 0 && i % 5 == 0) {
                    printBuzz.run();
                    i++;
                } 
            }
        }
        
    }

    // printFizzBuzz.run() outputs "fizzbuzz".
    public void fizzbuzz(Runnable printFizzBuzz) throws InterruptedException {
        while (i <= n) {
            synchronized(lock) {
                while (i <= n && i % 3 == 0 && i % 5 == 0) {
                    printFizzBuzz.run();
                    i++;
                }
            }
        }
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void number(IntConsumer printNumber) throws InterruptedException {
        while (i <= n) {
             synchronized(lock) {
                while (i <= n && i % 3 != 0 && i % 5 != 0) {
                    printNumber.accept(i);
                    i++; 
                }
            }
        }
    }
}
```