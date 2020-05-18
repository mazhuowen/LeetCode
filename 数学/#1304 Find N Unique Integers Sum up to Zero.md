[toc]

Given an integer `n`, return **any** array containing `n` **unique** integers such that they add up to `0`.



**Constraints:**

- $1 \le n \le 1000$



## 题目解读

&emsp;创建长度为`n`的数组，其中数字各不相同，其和为0。

```java
class Solution {
    public int[] sumZero(int n) {

    }
}
```

## 程序设计

* 最简单的思路，如果是奇数长度中间为0，两侧分别从1开始互为相反数。


```java
class Solution {
    public int[] sumZero(int n) {
        if (n <= 0) throw new IllegalArgumentException("invalid param");
        int[] res = new int[n];

        int base = 1;
        int idx = n / 2 - 1;
        while (idx >= 0) {
            // 两侧互为相反数
            res[idx] = -base;
            res[n - idx - 1] = base;
            idx--;
            base++;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.8MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;官方还提供了其它思路，都比较基础。