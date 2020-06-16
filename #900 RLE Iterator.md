[toc]

Write an iterator that iterates through a run-length encoded sequence.

The iterator is initialized by `RLEIterator(int[] A)`, where `A` is a run-length encoding of some sequence.  More specifically, for all even `i`, `A[i]` tells us the number of times that the non-negative integer value `A[i+1]` is repeated in the sequence.

The iterator supports one function: `next(int n)`, which exhausts the next n elements ($n \ge 1$) and returns the last element exhausted in this way.  If there is no element left to exhaust, next returns `-1` instead.

For example, we start with `A = [3,8,0,9,2,5]`, which is a run-length encoding of the sequence `[8,8,8,5,5]`.  This is because the sequence can be read as "three eights, zero nines, two fives".



Note:

* $0 \le \text{A.length} \le 1000$
* `A.length` is an even integer.
* $0 \le \text{A[i]} \le 10^9$
* There are at most `1000` calls to `RLEIterator.next(int n)` per test case.
* Each call to `RLEIterator.next(int n)` will have $1 \le n \le 10^9$.



## 题目解读

&emsp;

```java
class RLEIterator {

    public RLEIterator(int[] A) {

    }
    
    public int next(int n) {

    }
}

/**
 * Your RLEIterator object will be instantiated and called as such:
 * RLEIterator obj = new RLEIterator(A);
 * int param_1 = obj.next(n);
 */
```

## 程序设计

* 

```java

```

## 性能分析

&emsp;



## 官方解题

&emsp;