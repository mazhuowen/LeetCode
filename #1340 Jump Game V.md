[toc]

Given an array of integers `arr` and an integer `d`. In one step you can jump from index `i` to index:

* `i + x` where: $i + x < \text{arr.length}$ and $0 < x \le d$.
* `i - x` where: $i - x \ge 0$ and $0 < x \le d$.

In addition, you can only jump from index `i` to index `j` if $\text{arr[i]} > \text{arr[j]}$ and $\text{arr[i]} > \text{arr[k]}$ for all indices `k` between `i` and `j` (More formally $\min(i, j) < k < \max(i, j)$).

You can choose any index of the array and start jumping. Return the maximum number of indices you can visit.

Notice that you can not jump outside of the array at any time.

<img src="../images/#1340.jpeg" style="zoom: 50%;" />



**Constraints:**

- $1 \le \text{arr.length} \le 1000$
- $1 \le \text{arr[i]} \le 10^5$
- $1 \le d \le \text{arr.length}$



## 题目解读

&emsp;

```java
class Solution {
    public int maxJumps(int[] arr, int d) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public int maxJumps(int[] arr, int d) {
        if (arr == null || arr.length == 0 || d < 1) return 0;

        int maxStep = 0;
        for (int i = 0; i < arr.length; i++) {
            maxStep = Math.max(maxStep, maxJumps(arr, i, d));
        }
        return maxStep;
    }

    private int maxJumps(int[] arr, int start, int d) {
        int maxStep = 1;
        for (int i = start + 1; i < arr.length && i <= start + d; i++) {
            // 存在不低于起始的点，后续无法跳过到达
            if (arr[i] >= arr[start]) break;
            maxStep = Math.max(maxStep, maxJumps(arr, i, d) + 1);
        }

        for (int i = start - 1; i >= 0 && i >= start - d; i--) {
            if (arr[i] >= arr[start]) break;
            maxStep = Math.max(maxStep, maxJumps(arr, i, d) + 1);
        }
        return maxStep;
    }
}
```

## 性能分析

&emsp;



## 官方解题

&emsp;