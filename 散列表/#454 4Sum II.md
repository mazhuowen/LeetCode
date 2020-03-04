[toc]

Given four lists `A`, `B`, `C`, `D` of integer values, compute how many tuples $(i, j, k, l)$ there are such that $A[i] + B[j] + C[k] + D[l]$ is zero.

To make problem a bit easier, all `A`, `B`, `C`, `D` have same length of $N$ where $0 \le N \le 500$. All integers are in the range of $-2^{28}$ to $2^{28} - 1$ and the result is guaranteed to be at most $2^{31} - 1$.



## 题目解读

&emsp;给定四个数组，找出四个数组中的组合使得四数之和为0。

```java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        
    }
}
```

## 程序设计

* 最简单的思路是两两相加组合成字典，统计和的计数，然后遍历两个字典比较计算。

```java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        // 由于长度一致，只检测第一个
        if(A == null || A.length == 0) {
            return 0;
        }
        int n = A.length;
        Map<Integer, Integer> sum1 = new HashMap<>();
        Map<Integer, Integer> sum2 = new HashMap<>();
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < n; j++) {
                // 维护两个字典
                sum1.put(A[i] + B[j], sum1.getOrDefault(A[i] + B[j], 0) + 1);
                sum2.put(C[i] + D[j], sum2.getOrDefault(C[i] + D[j], 0) + 1);
            }
        }
        int count = 0;
        // 比那里查找
        for(Map.Entry<Integer, Integer> entry : sum1.entrySet()) {
            if(sum2.get(-entry.getKey()) != null) {
                // 计数相乘
                count += entry.getValue() * sum2.get(-entry.getKey());
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：143ms，在所有java提交中击败了26.06%的用户。

内存消耗：84.2MB，在所有java提交中击败了5.14%的用户。

## 官方解题

&emsp;暂无，密切关注。社区思路大同小异，采用哈希表记录和和计数。