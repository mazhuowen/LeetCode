[toc]

Given an array of integers `A`, a move consists of choosing any `A[i]`, and incrementing it by $1$.

Return the least number of moves to make every value in `A` unique.

 

**Example 1**:

```
Input: [1,2,2]
Output: 1
Explanation:  After 1 move, the array could be [1, 2, 3].
```

**Example 2**:

```
Input: [3,2,1,2,1,7]
Output: 6
Explanation:  After 6 moves, the array could be [3, 4, 1, 2, 5, 7].
It can be shown with 5 or less moves that it is impossible for the array to have all unique values.
```



**Note**:

* $0 \le \text{A.length} \le 40000$
* $0 \le \text{A[i]} < 40000$



## 题目解读

&emsp;给定数组，返回使得数组中每一个数字唯一的最少步数。

```java
class Solution {
    public int minIncrementForUnique(int[] A) {
        
    }
}
```

## 程序设计

* 采用排序，然后遍历判断小于等于之前值，是则需要将当前数字变为之前值加一。

```java
class Solution {
    public int minIncrementForUnique(int[] A) {
        if (A == null || A.length == 0) return 0;
        Arrays.sort(A);

        int res = 0, pre = -1;
        for (int num : A) {
            if (pre == -1 || num > pre) pre = num;
            // 变化的步数
            else res += ++pre - num;
        }
        return res;
    }
}
```

```c#
public class Solution {
    public int MinIncrementForUnique(int[] A) {
        if (A == null || A.Length == 0) return 0;
        Array.Sort(A);

        var (res, pre) = (0, -1);
        foreach (var num in A)
        {
            if (pre == -1 || num > pre)
                pre = num;
            else
                res += ++pre - num;
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：15 ms, 在所有 Java 提交中击败了71.45%的用户。

内存消耗：46.7 MB, 在所有 Java 提交中击败了6.77%的用户。

## 官方解题

&emsp;官方采用计数方法，将重复的值分配在其后计数为$0$的位置上。

```java
class Solution {
    public int minIncrementForUnique(int[] A) {
        int[] count = new int[80000];
        for (int num : A) count[num]++;

        // 最终结果，等待处理的数字
        int res = 0, wait = 0;
        for (int num = 0; num < 80000; num++) {
            // 当前多于一个，将多出的数字计入wait，并从res减去
            if (count[num] >= 2) {
                wait += count[num] - 1;
                res -= num * (count[num] - 1);
            } 
            // 当前值是空的，分配
            else if (wait > 0 && count[num] == 0) {
                wait--;
                res += num;
            }
        }

        return res;
    }
}
```

&emsp;时间复杂度为$O(L)$，空间复杂度为$O(L)$。

执行用时：8 ms, 在所有 Java 提交中击败了87.79%的用户。

内存消耗：43 MB, 在所有 Java 提交中击败了81.68%的用户。