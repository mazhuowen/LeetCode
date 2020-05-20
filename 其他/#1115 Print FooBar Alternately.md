[toc]

Suppose you are given the following code:

```java
class FooBar {
  public void foo() {
    for (int i = 0; i < n; i++) {
      print("foo");
    }
  }

  public void bar() {
    for (int i = 0; i < n; i++) {
      print("bar");
    }
  }
}
```

The same instance of `FooBar` will be passed to two different threads. Thread A will call `foo()` while thread B will call `bar()`. Modify the given program to output "foobar" $n$ times.



## 题目解读

&emsp;两个线程各自调用方法在`for`循环中轮流打印。

```java
class FooBar {
    private int n;

    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            
        	// printFoo.run() outputs "foo". Do not change or remove this line.
        	printFoo.run();
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            
            // printBar.run() outputs "bar". Do not change or remove this line.
        	printBar.run();
        }
    }
}
```

## 程序设计

* 采用`ReentrantLock`锁内部通信。

```java
class FooBar {
    private int n;
    private int flag = 0;
    ReentrantLock lock = new ReentrantLock(true);
    Condition foo = lock.newCondition();
    Condition bar = lock.newCondition();

    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            lock.lock();
            // 标识不是0，则当前线程进入休眠
            if (flag != 0) foo.await();
        	// printFoo.run() outputs "foo". Do not change or remove this line.
        	printFoo.run();
            flag = 1;
            // 唤醒另一个线程
            bar.signal();
        	lock.unlock();
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            lock.lock();
            if (flag != 1) bar.await();
            // printBar.run() outputs "bar". Do not change or remove this line.
        	printBar.run();
            flag = 0;
            foo.signal();
        	lock.unlock();
        }
    }
}
```

## 性能分析

执行用时：21ms，在所有java提交中击败了99.86%的用户。

内存消耗：39.9MB，在所有java提交中击败了7.41%的用户。

## 官方解题

&emsp;暂无，密切关注。