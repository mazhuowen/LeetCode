[toc]

You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.

Given an integer array `flowerbed` containing `0`'s and `1`'s, where `0` means empty and `1` means not empty, and an integer $n$, return if $n$ new flowers can be planted in the `flowerbed` without violating the no-adjacent-flowers rule.

 

**Example 1**:

```
Input: flowerbed = [1,0,0,0,1], n = 1
Output: true
```

**Example 2**:

```
Input: flowerbed = [1,0,0,0,1], n = 2
Output: false
```



**Constraints**:

* $1 \le \text{flowerbed.length} \le 2 * 10^4$
* `flowerbed[i]` is $0$ or $1$.
* There are no two adjacent flowers in `flowerbed`.
* $0 \le n \le \text{flowerbed.length}$



## 题目解读

&emsp;判断是否可种$n$个花，且每个花不相邻。

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {

    }
}
```

## 程序设计

* 逐个遍历，需注意首尾特殊情况。

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        if (n == 0) return true;
		
        // 首空两格的情况
        if (flowerbed[0] == 0 && (flowerbed.length == 1 || flowerbed[1] == 0)) {
            flowerbed[0] = 1;
            n--;
        } 
        for (int i = 2; i < flowerbed.length && n > 0; i++) {
            if (flowerbed[i] == 0 && flowerbed[i - 1] == 0 && flowerbed[i - 2] == 0) {
                flowerbed[i - 1] = 1;
                n--;
            }
        }

        // 尾空两格的情况
        if (flowerbed.length >= 2 && flowerbed[flowerbed.length - 1] == 0 && flowerbed[flowerbed.length - 2] == 0) n--;
        return n <= 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：39.8 MB, 在所有 Java 提交中击败了83.45%的用户。

## 官方解题

&emsp;官方思路类似，优化判断条件。

