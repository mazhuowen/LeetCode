[toc]

Given an array of integers `arr` and an integer `d`. In one step you can jump from index `i` to index:

* `i + x` where: $i + x < \text{arr.length}$ and $0 < x \le d$.
* `i - x` where: $i - x \ge 0$ and $0 < x \le d$.

In addition, you can only jump from index `i` to index `j` if $\text{arr[i]} > \text{arr[j]}$ and $\text{arr[i]} > \text{arr[k]}$ for all indices `k` between `i` and `j` (More formally $\min(i, j) < k < \max(i, j)$).

You can choose any index of the array and start jumping. Return the maximum number of indices you can visit.

Notice that you can not jump outside of the array at any time.

<img src="../images/#1340.jpeg" style="zoom: 40%;" />



**Constraints:**

- $1 \le \text{arr.length} \le 1000$
- $1 \le \text{arr[i]} \le 10^5$
- $1 \le d \le \text{arr.length}$



## 题目解读

&emsp;求可跳达的最大数目。

```java
class Solution {
    public int maxJumps(int[] arr, int d) {

    }
}
```

## 程序设计

* 最基本的思路是选择起始点回溯尝试，但由于存在重复计算，时间复杂度为$O(d^N)$，会超时。

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
            // 当前可访问索引为当前位置加后续尝试索引数
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

* 引入额外数据结构记录每个位置的可达最大索引数。

```java
class Solution {
    int[] record;

    public int maxJumps(int[] arr, int d) {
        if (arr == null || arr.length == 0 || d < 1) return 0;

        this.record = new int[arr.length];
        int maxStep = 0;
        for (int i = 0; i < arr.length; i++) {
            maxStep = Math.max(maxStep, maxJumps(arr, i, d));
        }
        return maxStep;
    }

    private int maxJumps(int[] arr, int start, int d) {
        if (record[start] != 0) return record[start];
        int maxStep = 1;
        for (int i = start + 1; i < arr.length && i <= start + d; i++) {
            // 存在不低于起始的点，后续无法跳过到达
            if (arr[i] >= arr[start]) break;
            // 当前可访问索引为当前位置加后续尝试索引数
            maxStep = Math.max(maxStep, maxJumps(arr, i, d) + 1);
        }

        for (int i = start - 1; i >= 0 && i >= start - d; i--) {
            if (arr[i] >= arr[start]) break;
            maxStep = Math.max(maxStep, maxJumps(arr, i, d) + 1);
        }
        record[start] = maxStep;
        return maxStep;
    }
}
```

## 性能分析

&emsp;引入额外空间回溯法，时间复杂度为$O(dN)$，空间复杂度为$O(N)$。

执行用时：12ms，在所有java提交中击败了93.81%的用户。

内存消耗：39.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方还提到了动态规划的思路，需要将数组索引按照数值高低排序，然后从低到高进行。