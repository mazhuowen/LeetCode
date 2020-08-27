[toc]

In some array `arr`, the values were in arithmetic progression: the values `arr[i+1] - arr[i]` are all equal for every $0 \le i < \text{arr.length} - 1$.

Then, a value from `arr` was removed that **was not the first or last value in the array**.

Return the removed value.



**Constraints**:

* $3 \le \text{arr.length} \le 1000$
* $0 \le \text{arr[i]} \le 10^5$



## 题目解读

&emsp;等差数列中缺少一个数值，求该数值。

```java
class Solution {
    public int missingNumber(int[] arr) {

    }
}
```

## 程序设计

* 可使用二分查找。由于除了头尾，等差数列确实一个数字，可根据头尾计算出等差；当$0 \sim mid$不缺少值，则`arr[mid] - arr[0] = diff * mid`，否则表示缺失值，缩小范围查找。
* 对于常规二分查找需要改变形式，首先此处`left`和`right`代表缺失值的左右数字，最后是两个数字，不是一个数字，收缩不能使用`left = left + 1`或`right = right - 1`，其次`while`循环需要使用`left + 1 < right`。

```java
class Solution {
    public int missingNumber(int[] arr) {
        if (arr == null || arr.length < 3) throw new IllegalArgumentException("invalid param");
        int n = arr.length;
        // 等差
        int diff = (arr[n - 1] - arr[0]) / n;
        int left = 0, right = n - 1;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            // 确实值在left~mid间（不等于是因为统一等差为正数和负数的条件）
            if (arr[mid] - arr[0] != diff * mid) right = mid;
            else left = mid;
        }
        return (arr[left] + arr[right]) / 2;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了62.50%的用户。

## 官方解题

&emsp;暂无，密切关注。