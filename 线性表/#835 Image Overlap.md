[toc]

You are given two images `img1` and `img2` both of size $n \times n$, represented as binary, square matrices of the same size. (A binary matrix has only 0s and 1s as values.)

We translate one image however we choose (sliding it left, right, up, or down any number of units), and place it on top of the other image.  After, the overlap of this translation is the number of positions that have a $1$ in both images.

(Note also that a translation does not include any kind of rotation.)

What is the largest possible overlap?

 

**Example 1**:

<img src="../images/#835_exp1.jpg" style="zoom:80%;" />

```
Input: img1 = [[1,1,0],[0,1,0],[0,1,0]], img2 = [[0,0,0],[0,1,1],[0,0,1]]
Output: 3
Explanation: We slide img1 to right by 1 unit and down by 1 unit.
The number of positions that have a 1 in both images is 3. (Shown in red)
```

**Example 2**:

```
Input: img1 = [[1]], img2 = [[1]]
Output: 1
```

**Example 3**:

```
Input: img1 = [[0]], img2 = [[0]]
Output: 0
```



**Constraints**:

* $n == \text{img1.length}$
* $n == \text{img1[i].length}$
* $n == \text{img2.length}$
* $n == \text{img2[i].length}$
* $1 \le n \le 30$
* `img1[i][j]` is $0$ or $1$.
* `img2[i][j]` is $0$ or $1$.



## 题目解读

&emsp;给定两个只包含$0$和$1$的正方形，求调整一个正方形（上下左右移动）并覆盖到另一个正方形后$1$的最多的数目。

```java
class Solution {
    public int largestOverlap(int[][] img1, int[][] img2) {

    }
}
```

## 程序设计

* 参考官方思路，与其模拟偏移遍历计数，不如直接遍历并计算两个位置的偏移，然后计数。

```java
class Solution {
    public int largestOverlap(int[][] img1, int[][] img2) {
        int n = img1.length;
        int[][] offset = new int[2 * n + 1][2 * n + 1];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (img1[i][j] == 0) continue;
                for (int k = 0; k < n; k++) {
                    for (int l = 0; l < n; l++) {
                        if (img2[k][l] == 0) continue;
                        // 在相应偏移处计数
                        offset[n + i - k][n + j - l]++;
                    }
                }
            }
        }

        int res = 0;
        // 统计最大值
        for (int i = 0; i < 2 * n + 1; i++) {
            for (int j = 0; j < 2 * n + 1; j++) {
                res = Math.max(res, offset[i][j]);
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^4)$，空间复杂度为$O(N^2)$。

执行用时：6 ms, 在所有 Java 提交中击败了98.82%的用户。

内存消耗：37.9 MB, 在所有 Java 提交中击败了61.18%的用户。

## 官方解题

&emsp;上述思路参考官方解题。
