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

&emsp;给定数字第一个数字表示后继数字出现的次数，设计迭代器。

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

* 数据结构中引入当前数字和剩余计数，如果不足则迭代到下个数字，否则扣除计数返回当前数字。
* 注意计数的清零。

```java
class RLEIterator {
    // 当前值及其计数
    int idx;
    int count;
    int[] A;

    public RLEIterator(int[] A) {
        if (A == null || A.length % 2 == 1) throw new IllegalArgumentException("invalid param");
        this.A = A;
        this.idx = -1;
        this.count = 0;
    }
    
    public int next(int n) {
        if (n < 0) throw new IllegalArgumentException("invalid param");

        while (n > count && idx + 1 < A.length) {
            n -= count;
            // 消耗尽上一个序列，进入下一个序列
            count = A[idx + 1];
            idx += 2;
        }

        // 迭代结束
        if (n > count) {
            // 注意计数清零
            count = -1;
            return -1;
        }

        // 更新计数并返回
        count -= n;
        return A[idx];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了94.69%的用户。

内存消耗：39.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上，实现细节不同。