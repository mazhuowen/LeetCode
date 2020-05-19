[toc]

Suppose we have a class:

```java
public class Foo {
  public void first() { print("first"); }
  public void second() { print("second"); }
  public void third() { print("third"); }
}
```

The same instance of `Foo` will be passed to three different threads. Thread `A` will call `first()`, thread `B` will call `second()`, and thread `C` will call `third()`. Design a mechanism and modify the program to ensure that `second()` is executed after `first()`, and `third()` is executed after `second()`.



Note:

We do not know how the threads will be scheduled in the operating system, even though the numbers in the input seems to imply the ordering. The input format you see is mainly to ensure our tests' comprehensiveness.



## 题目解读

&emsp;三个线程分别调用三个方法，需要保证三个方法调用的有序性。

```java
class Foo {

    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
        
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
    }

    public void third(Runnable printThird) throws InterruptedException {
        
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
    }
}
```

## 程序设计

* 由于三个线程固定调用三个方法，可以使用`volatile`关键字作为状态，三个方法根据关键字判断是否是当前方法调用。

```java
class Foo {
    private volatile int process = 0;

    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
        while (process != 0) continue;
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        process = 1;
    }

    public void second(Runnable printSecond) throws InterruptedException {
        while (process != 1) continue;
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        process = 2;
    }

    public void third(Runnable printThird) throws InterruptedException {
        while (process != 2) continue;
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
        process = 0;
    }
}
```

## 性能分析

执行用时：14ms，在所有java提交中击败了45.09%的用户。

内存消耗：39.2MB，在所有java提交中击败了6.98%的用户。

## 官方解题

&emsp;官方使用原子变量代替`volatile`关键字。