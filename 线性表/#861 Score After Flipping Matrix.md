[toc]

We have a two dimensional matrix `A` where each value is $0$ or $1$.

A move consists of choosing any row or column, and toggling each value in that row or column: changing all $0$s to $1$s, and all $1$s to $0$s.

After making any number of moves, every row of this matrix is interpreted as a binary number, and the score of the matrix is the sum of these numbers.

Return the highest possible score.

 

**Example 1**:

```
Input: [[0,0,1,1],[1,0,1,0],[1,1,0,0]]
Output: 39
Explanation:
Toggled to [[1,1,1,1],[1,0,0,1],[1,1,1,1]].
0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39
```



**Note**:

* $1 \le \text{A.length} \le 20$
* $1 \le \text{A[0].length} \le 20$
* `A[i][j]` is $0$ or $1$.



## 题目解读

&emsp;给定矩阵，可翻转行或列若干次，求最后每行二进制数字之和的最大值。

```java
class Solution {
    public int matrixScore(int[][] A) {
        
    }
}
```

## 程序设计

* 排除复杂的行列翻转规律，从二进制数字最大和的角度考虑，显然每行最左端是$1$时最大，在这个条件下，对于后续的列，如果$0$超过该列所有值的一半，则翻转，得到的值较大。

```java
class Solution {
    public int matrixScore(int[][] A) {
        int m = A.length, n = A[0].length;
        // 记录当前列中0的数目
        int[] counter = new int[n];
        // 翻转左侧是0的行
        for (int i = 0; i < m; i++) {
            boolean change = A[i][0] == 0;
            for (int j = 0; j < n; j++) {
                if (change) A[i][j] ^= 1;
                if (A[i][j] == 0) counter[j]++;
            }
        }

        // 翻转0占大多数的列
        for (int j = 0; j < n; j++) {
            if (counter[j] > m / 2) {
                for (int i = 0; i < m; i++) {
                    A[i][j] ^= 1;
                }
            }
        }
		
        // 统计求和
        int res = 0;
        for (int[] nums : A) {
            int cur = 0;
            for (int num : nums) cur = (cur << 1) + num;
            res += cur;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(N)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：36.4 MB, 在所有 Java 提交中击败了30.92%的用户。

## 官方解题

&emsp;官方精简上述三个步骤，使用两个步骤实现。

```java
class Solution {
    public int matrixScore(int[][] A) {
        int m = A.length, n = A[0].length;
        // 第一列最高位能变为1
        int res = m * (1 << (n - 1));
        // 从第二列开始
        for (int j = 1; j < n; j++) {
            // 当前列1的数目
            int countOne = 0;
            for (int i = 0; i < m; i++) {
                // 首位是1,行不翻转
                if (A[i][0] == 1) countOne += A[i][j];
                // 首位是0,行翻转，0的计数就是1的计数
                else countOne += 1 - A[i][j];
            }

            // 翻转1
            if (2 * countOne < m) countOne = m - countOne;
            res += countOne * (1 << (n - j - 1));
        }
        return res;
    }
}
```


&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：36.5 MB, 在所有 Java 提交中击败了9.89%的用户。