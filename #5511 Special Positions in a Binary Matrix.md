[toc]

Given a `rows x cols` matrix `mat`, where `mat[i][j]` is either $0$ or $1$, return the number of special positions in `mat`.

A position `(i,j)` is called **special** if `mat[i][j] == 1` and all other elements in row $i$ and column $j$ are $0$ (rows and columns are **0-indexed**).



**Constraints**:

* $\text{rows} == \text{mat.length}$
* $\text{cols} == \text{mat[i].length}$
* $1 \le \text{rows, cols} \le 100$
* `mat[i][j]` is $0$ or $1$.



## 题目解读

&emsp;统计满足行列只有一个元素的数目。

```java
class Solution {
    public int numSpecial(int[][] mat) {
        
    }
}
```

## 程序设计

* 首先统计行列中$1$的数目，然后再次遍历判断。

```java
class Solution {
    public int numSpecial(int[][] mat) {
        int n = mat.length, m = mat[0].length;
        int[] row = new int[n], col = new int[m];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (mat[i][j] == 1) {
                    row[i]++;
                    col[j]++;
                }
            }
        }
        
        int res = 0;
        point: for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (mat[i][j] == 1) {
                    if (row[i] == 1 && col[j] == 1) {
                        res++;
                        continue point;
                    }
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。



## 官方解题

&emsp;