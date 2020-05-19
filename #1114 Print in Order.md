[toc]

Suppose we have a class:

public class Foo {
  public void first() { print("first"); }
  public void second() { print("second"); }
  public void third() { print("third"); }
}
The same instance of Foo will be passed to three different threads. Thread A will call first(), thread B will call second(), and thread C will call third(). Design a mechanism and modify the program to ensure that second() is executed after first(), and third() is executed after second().



Note:

We do not know how the threads will be scheduled in the operating system, even though the numbers in the input seems to imply the ordering. The input format you see is mainly to ensure our tests' comprehensiveness.



##



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

##

* 

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

##



执行用时 :14 ms, 在所有 Java 提交中击败了45.09%的用户

内存消耗 :39.2 MB, 在所有 Java 提交中击败了6.98%的用户

##