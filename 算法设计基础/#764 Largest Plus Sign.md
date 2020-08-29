[toc]

In a 2D `grid` from `(0, 0)` to `(N-1, N-1)`, every cell contains a $1$, except those cells in the given list `mines` which are $0$. What is the largest axis-aligned plus sign of `1`s contained in the grid? Return the order of the plus sign. If there is none, return $0$.

An "axis-aligned plus sign of `1`s of order **k**" has some center `grid[x][y] = 1` along with $4$ arms of length $k-1$ going up, down, left, and right, and made of `1`s. This is demonstrated in the diagrams below. Note that there could be `0`s or `1`s beyond the arms of the plus sign, only the relevant area of the plus sign is checked for `1`s.

**Examples of Axis-Aligned Plus Signs of Order k**:

```
Order 1:
000
010
000

Order 2:
00000
00100
01110
00100
00000

Order 3:
0000000
0001000
0001000
0111110
0001000
0001000
0000000
```



**Note**:

* $N$ will be an integer in the range $[1, 500]$.
* `mines` will have length at most $5000$.
* `mines[i]` will be length $2$ and consist of integers in the range $[0, N-1]$.
* (Additionally, programs submitted in C, C++, or C# will be judged with a slightly smaller time limit.)



## 题目解读

&emsp;给定正方形中为$0$的坐标，返回可由$1$构成的最大加号的半径。

```java
class Solution {
    public int orderOfLargestPlusSign(int N, int[][] mines) {

    }
}
```

## 程序设计

* 最基本的思路是统计每个中心点的四个方向距离，则最大的加号半径就是最短值。

```java
class Solution {
    public int orderOfLargestPlusSign(int N, int[][] mines) {
        if (mines == null || mines.length == 0) return (N + 1) / 2;

        // 障碍点
        Set<Integer> set = new HashSet<>();
        for (int[] array : mines) set.add(array[0] * N + array[1]);

        // dp(i,j)表示坐标为(i,j)的加号最大半径（由四个方向的最短距离决定）
        int[][] dp = new int[N][N];
        int res = 0;
        // 统计左右
        for (int r = 0; r < N; r++) {
            int count = 1;
            for (int c = 0; c < N; c++) {
                if (set.contains(r * N + c)) {
                    count = 1;
                    continue;
                }
                dp[r][c] = count++;
            }
            count = 1;
            for (int c = N - 1; c >= 0; c--) {
                if (set.contains(r * N + c)) {
                    count = 1;
                    continue;
                }
                dp[r][c] = Math.min(dp[r][c], count++);
            }
        }
        // 统计上下
        for (int c = 0; c < N; c++) {
            int count = 1;
            for (int r = 0; r < N; r++) {
                if (set.contains(r * N + c)) {
                    count = 1;
                    continue;
                }
                dp[r][c] = Math.min(dp[r][c], count++);
            }
            count = 1;
            for (int r = N - 1; r >= 0; r--) {
                if (set.contains(r * N + c)) {
                    count = 1;
                    continue;
                }
                dp[r][c] = Math.min(dp[r][c], count++);
                res = Math.max(res, dp[r][c]);
            }
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：204ms，在所有java提交中击败了35.17%的用户。

内存消耗：47.4MB，在所有java提交中击败了70.91%的用户。

## 官方解题

&emsp;同上。社区思路合并四个方向的循环为一个。

```java
public class Solution {
    public int orderOfLargestPlusSign(int N, int[][] mines) {
        int[][] dp = new int[N][N];
        // 初始化
        for(int i = 0; i < N; i++) Arrays.fill(dp[i], N);
        // 标记障碍物
        for(int[] m : mines) dp[m[0]][m[1]] = 0;
        
        for (int i = 0; i < N; i++) {
            // 初始化上下左右四个方向的距离为0，初始化两端（左右或上下）为j、k
            for (int j = 0, k = N - 1, l = 0, r = 0, u = 0, d = 0; j < N; j++, k--) {
                // i为行，j为左侧距离
                dp[i][j] = Math.min(dp[i][j], l = dp[i][j] == 0 ? 0 : ++l);
                // i为行，k为右侧距离
                dp[i][k] = Math.min(dp[i][k], r = dp[i][k] == 0 ? 0 : ++r);
                // i为列，j为上方距离
                dp[j][i] = Math.min(dp[j][i], u = dp[j][i] == 0 ? 0 : ++u);
                // i为列，j为下方距离
                dp[k][i] = Math.min(dp[k][i], d = dp[k][i] == 0 ? 0 : ++d);
            }
        }
        
        int res = 0;
        for(int i = 0; i < N; i++){
            for(int j = 0; j < N; j++){
                res = Math.max(res, dp[i][j]);
            }
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：30ms，在所有java提交中击败了97.93%的用户。

内存消耗：42.3MB，在所有java提交中击败了94.55%的用户。