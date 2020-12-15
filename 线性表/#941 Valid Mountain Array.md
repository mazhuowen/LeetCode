[toc]

Given an array of integers `arr`, return `true` if and only if it is a valid mountain array.

Recall that arr is a mountain array if and only if:

* $\text{arr.length} \ge 3$
* There exists some $i$ with $0 < i < \text{arr.length - 1}$ such that:
  * $\text{arr[0]} < \text{arr[1]} < \dots < \text{arr[i - 1]} < \text{arr[i]}$
  * $\text{arr[i]} > \text{arr[i + 1]} > \dots > \text{arr[arr.length - 1]}$

 <img src="..\images\#941.png" style="zoom:67%;" />



**Example 1**:

```
Input: arr = [2,1]
Output: false
```

**Example 2**:

```
Input: arr = [3,5,5]
Output: false
```

**Example 3**:

```
Input: arr = [0,3,2,1]
Output: true
```



**Constraints**:

* $1 \le \text{arr.length} \le 10^4$
* $0 \le \text{arr[i]} \le 10^4$



## 题目解读

&emsp;判断是否是山形数组。

```java
class Solution {
    public boolean validMountainArray(int[] arr) {

    }
}
```

## 程序设计

* 遍历判断。

```java
class Solution {
    public boolean validMountainArray(int[] arr) {
        if (arr == null || arr.length < 3) return false;
        int idx = 1;
        while (idx < arr.length && arr[idx] > arr[idx - 1]) idx++;
        // 递减、递增或存在平地，返回
        if (idx == arr.length || idx == 1 && arr[idx] < arr[idx - 1] || arr[idx] == arr[idx - 1]) return false;
        idx++;
        while (idx < arr.length && arr[idx] < arr[idx - 1]) idx++;
        return idx == arr.length;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1 ms, 在所有 Java 提交中击败了99.98%的用户

内存消耗：39.6 MB, 在所有 Java 提交中击败了44.36%的用户

## 官方解题

&emsp;同上。
