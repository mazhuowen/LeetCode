[toc]

Five silent philosophers sit at a round table with bowls of spaghetti. Forks are placed between each pair of adjacent philosophers.

Each philosopher must alternately think and eat. However, a philosopher can only eat spaghetti when they have both left and right forks. Each fork can be held by only one philosopher and so a philosopher can use the fork only if it is not being used by another philosopher. After an individual philosopher finishes eating, they need to put down both forks so that the forks become available to others. A philosopher can take the fork on their right or the one on their left as they become available, but cannot start eating before getting both forks.

Eating is not limited by the remaining amounts of spaghetti or stomach space; an infinite supply and an infinite demand are assumed.

Design a discipline of behaviour (a concurrent algorithm) such that no philosopher will starve; i.e., each can forever continue to alternate between eating and thinking, assuming that no philosopher can know when others may want to eat or think.

<img src="../images/#1226.png" style="zoom:50%;" />

The problem statement and the image above are taken from wikipedia.org

 

The philosophers' ids are numbered from $0$ to $4$ in a **clockwise** order. Implement the function `void wantsToEat(philosopher, pickLeftFork, pickRightFork, eat, putLeftFork, putRightFork)` where:

* `philosopher` is the id of the philosopher who wants to eat.
* `pickLeftFork` and `pickRightFork` are functions you can call to pick the corresponding forks of that philosopher.
* `eat` is a function you can call to let the philosopher eat once he has picked both forks.
* `putLeftFork` and `putRightFork` are functions you can call to put down the corresponding forks of that philosopher.
* The philosophers are assumed to be thinking as long as they are not asking to eat (the function is not being called with their number).

Five threads, each representing a philosopher, will simultaneously use one object of your class to simulate the process. The function may be called for the same philosopher more than once, even before the last call ends.

 

**Constraints**:

$1 \le n \le 60$



## 题目解读

&emsp;经典的哲学家就餐问题。

```java
class DiningPhilosophers {

    public DiningPhilosophers() {
        
    }

    // call the run() method of any runnable to execute its code
    public void wantsToEat(int philosopher,
                           Runnable pickLeftFork,
                           Runnable pickRightFork,
                           Runnable eat,
                           Runnable putLeftFork,
                           Runnable putRightFork) throws InterruptedException {
        
    }
}
```

## 程序设计

* 最基本的思路是每次只限制一个人拿起两个叉子，这样只需要全局锁即可，但效率比较低；参考社区思路，尝试拿起叉子的人不能超过四个，可使用信号量来维护关系。

```java
class DiningPhilosophers {
    Semaphore count;
    Lock[] locks;

    public DiningPhilosophers() {
        // 最多只有四个人尝试拿起叉子
        this.count =  new Semaphore(4);
        // 五个叉子锁
        this.locks = new Lock[]{new ReentrantLock(),
                new ReentrantLock(),
                new ReentrantLock(),
                new ReentrantLock(),
                new ReentrantLock()};
    }

    // call the run() method of any runnable to execute its code
    public void wantsToEat(int philosopher,
                           Runnable pickLeftFork,
                           Runnable pickRightFork,
                           Runnable eat,
                           Runnable putLeftFork,
                           Runnable putRightFork) throws InterruptedException {

        count.acquire();
        locks[philosopher].lock();
        locks[(philosopher + 1) % 5].lock();
        try {
            pickLeftFork.run();
            pickRightFork.run();

            eat.run();

            putLeftFork.run();
            putRightFork.run();
        } finally {
            locks[philosopher].unlock();
            locks[(philosopher + 1) % 5].unlock();
            count.release();
        }
    }
}
```

## 性能分析

执行用时：13ms，在所有java提交中击败了99.34%的用户。

内存消耗：40.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有一种思路就是打乱获取叉子锁的顺序，一部分人先获取左边的叉子，另一部分人先获取右边的叉子。

```java
class DiningPhilosophers {

    volatile Semaphore[] forks = new Semaphore[]{
        new Semaphore(1)
        , new Semaphore(1)
        , new Semaphore(1)
        , new Semaphore(1)
        , new Semaphore(1)
    };

    public DiningPhilosophers() {
        
    }

    // call the run() method of any runnable to execute its code
    public void wantsToEat(int philosopher,
                           Runnable pickLeftFork,
                           Runnable pickRightFork,
                           Runnable eat,
                           Runnable putLeftFork,
                           Runnable putRightFork) throws InterruptedException {

        int leftForkNo = philosopher;
        int rightForkNo = (philosopher + 1) % 5;

        if (philosopher % 2 == 0) {
            forks[leftForkNo].acquire();
            forks[rightForkNo].acquire();
        } else {
            forks[rightForkNo].acquire();
            forks[leftForkNo].acquire();
        }

        pickLeftFork.run();
        pickRightFork.run();

        eat.run();

        putLeftFork.run();
        putRightFork.run();
        
        forks[leftForkNo].release();
        forks[rightForkNo].release();
    }
}
```

