[toc]

You are given an array `rectangles` where `rectangles[i] = [li, wi]` represents the `ith` rectangle of length `li` and width `wi`.

You can cut the `ith` rectangle to form a square with a side length of $k$ if both $k \le l_i$ and $k \le w_i$. For example, if you have a rectangle $[4,6]$, you can cut it to get a square with a side length of at most $4$.

Let `maxLen` be the side length of the **largest** square you can obtain from any of the given rectangles.

Return the **number** of rectangles that can make a square with a side length of `maxLen`.

 

**Example 1**:

```
Input: rectangles = [[5,8],[3,9],[5,12],[16,5]]
Output: 3
Explanation: The largest squares you can get from each rectangle are of lengths [5,3,5,5].
The largest possible square is of length 5, and you can get it out of 3 rectangles.
```

**Example 2**:

```
Input: rectangles = [[2,3],[3,7],[4,3],[3,7]]
Output: 3
```



**Constraints**:

* $1 \le \text{rectangles.length} \le 1000$
* $\text{rectangles[i].length} == 2$
* $1 \le l_i, w_i \le 10^9$
* $l_i \ne w_i$



## 题目解读

&emsp;求可裁剪的最大正方形边长。

```java
class Solution {
    public int countGoodRectangles(int[][] rectangles) {

    }
}
```

## 程序设计

* 遍历判断即可。

```java
class Solution {
    public int countGoodRectangles(int[][] rectangles) {
        if (rectangles == null || rectangles.length == 0) return 0;
        
        int max = 0, num = 0;
        for (int[] rectangle : rectangles) {
            int cur = Math.min(rectangle[0], rectangle[1]);
            if (cur == max) num++;
            else if (cur > max) {
                max = cur;
                num = 1;
            }
        }
        return num;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2 ms, 在所有 Java 提交中击败了99.85%的用户。

内存消耗：39.1 MB, 在所有 Java 提交中击败了17.56%的用户。

## 官方解题

&emsp;暂无，密切关注。
