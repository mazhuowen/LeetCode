[toc]

Implementing the class `MajorityChecker`, which has the following API:

* `MajorityChecker(int[] arr)` constructs an instance of `MajorityChecker` with the given array `arr`;

* `int query(int left, int right, int threshold)` has arguments such that:

* * $0 \le \text{left} \le \text{right} < \text{arr.length}$ representing a subarray of `arr`;

  * $2 * \text{threshold} > \text{right} - \text{left} + 1$, ie. the threshold is always a strict majority of the length of the subarray

Each `query(...)` returns the element in `arr[left]`, `arr[left+1]`, ..., `arr[right]` that occurs at least threshold times, or `-1` if no such element exists.

Constraints:

* $1 \le \text{arr.length} \le 20000$
* $1 \le \text{arr[i]} \le 20000$
* For each query, $0 \le \text{left} \le \text{right} < \text{len(arr)}$
* For each query, $2 * \text{threshold} > \text{right} - \text{left} + 1$
* The number of queries is at most $10000$



## 题目解读

&emsp;返回数组区间出现次数大于等于阈值的数，不存在返回-1。题目规定阈值始终比序列长度的一半要大。

```java
class MajorityChecker {

    public MajorityChecker(int[] arr) {

    }
    
    public int query(int left, int right, int threshold) {

    }
}

/**
 * Your MajorityChecker object will be instantiated and called as such:
 * MajorityChecker obj = new MajorityChecker(arr);
 * int param_1 = obj.query(left,right,threshold);
 */
```

## 程序设计



## 性能分析



## 官方解题

