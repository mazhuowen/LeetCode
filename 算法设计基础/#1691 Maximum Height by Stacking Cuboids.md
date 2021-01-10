[toc]

Given $n$ `cuboids` where the dimensions of the `ith` cuboid is `cuboids[i] = [widthi, lengthi, heighti]` (**0-indexed**). Choose a **subset** of `cuboids` and place them on each other.

You can place cuboid $i$ on cuboid $j$ if $\text{width}_i \le \text{width}_j$ and $\text{length}_i \le \text{length}_j$ and $\text{height}_i \le \text{height}_j$. You can rearrange any cuboid's dimensions by rotating it to put it on another cuboid.

Return the **maximum height** of the stacked `cuboids`.

 

**Example 1**:

<img src="..\images\#1691_exp1.jpg" style="zoom: 50%;" />

```
Input: cuboids = [[50,45,20],[95,37,53],[45,23,12]]
Output: 190
Explanation:
Cuboid 1 is placed on the bottom with the 53x37 side facing down with height 95.
Cuboid 0 is placed next with the 45x20 side facing down with height 50.
Cuboid 2 is placed next with the 23x12 side facing down with height 45.
The total height is 95 + 50 + 45 = 190.
```

**Example 2**:

```
Input: cuboids = [[38,25,45],[76,35,3]]
Output: 76
Explanation:
You can't place any of the cuboids on the other.
We choose cuboid 1 and rotate it so that the 35x3 side is facing down and its height is 76.
```

**Example 3**:

```
Input: cuboids = [[7,11,17],[7,17,11],[11,7,17],[11,17,7],[17,7,11],[17,11,7]]
Output: 102
Explanation:
After rearranging the cuboids, you can see that all cuboids have the same dimension.
You can place the 11x7 side down on all cuboids so their heights are 17.
The maximum height of stacked cuboids is 6 * 17 = 102.
```



**Constraints**:

* $n == \text{cuboids.length}$
* $1 \le n \le 100$
* $1 \le \text{width}_i, \text{length}_i, \text{height}_i \le 100$



## 题目解读

&emsp;给定箱子，每个箱子可旋转，求叠加的最高高度。

```java
class Solution {
    public int maxHeight(int[][] cuboids) {
        
    }
}
```

## 程序设计

* 如果不能旋转箱子，则问题就是最长递增子序列的变形，可排序一个维度，然后动态规划即可；
* 考虑箱子的旋转，每个箱子旋转后有六种组合，可将这六种组合都加入到候选箱子；然后根据长宽高排序，动态规划数组`dp(i)`表示将箱子$i$放入底部的最大叠加高度，对于$0 \le j < i$，如果箱子$j$满足叠加条件，则尝试叠加并更新高度；
* 上述思路的核心在于对每个箱子的旋转状态分配索引，使得$\text{idx}/6$相等，避免遍历到的$j$就是$i$的一种旋转形态；其次根据长宽高排序后也会避免箱子$j$叠加在$i$上，但是$j$上又叠加了$i$的另一种旋转形态，因为对于一个箱子$i$旋转后排序，如果旋转形态在$i$前面，则必然高度要大于$i$，这样这个旋转形态必然不能叠加在$j$上；这两个步骤保证了我们可以按照一般情况处理得到正确结果。

```java
class Solution {
    public int maxHeight(int[][] cuboids) {
        int n = cuboids.length;
        // 将所有情况放入数组
        int[][] newCub = new int[6 * n][3];
        Integer[] index = new Integer[6 * n];
        for (int i = 0; i < n; i++) {
            newCub[6 * i] = cuboids[i];
            newCub[6 * i + 1] = new int[]{cuboids[i][1], cuboids[i][0], cuboids[i][2]};
            newCub[6 * i + 2] = new int[]{cuboids[i][1], cuboids[i][2], cuboids[i][0]};
            newCub[6 * i + 3] = new int[]{cuboids[i][2], cuboids[i][1], cuboids[i][0]};
            newCub[6 * i + 4] = new int[]{cuboids[i][0], cuboids[i][2], cuboids[i][1]};
            newCub[6 * i + 5] = new int[]{cuboids[i][2], cuboids[i][0], cuboids[i][1]};
        }
        
        for (int i = 0; i < 6 * n; i++) index[i] = i;
        Arrays.sort(index, (a, b) -> newCub[a][0] == newCub[b][0] ? newCub[a][1] == newCub[b][1] ? newCub[a][2] - newCub[b][2] : newCub[a][1] - newCub[b][1] : newCub[a][0] - newCub[b][0]);
        
        int max = 0;
        // 动态规划数组表示将当前箱子放在最底层的最大高度
        int[] dp = new int[6 * n];
        for (int i = 0; i < 6 * n; i++) {
            // 初始化，只有当前一个箱子
            dp[i] = newCub[index[i]][2];
            // 叠加前面的箱子到当前箱子上面
            for (int j = 0; j < i; j++) {
                // 同一个箱子，或长宽高不符合要求
                if (index[i] / 6 == index[j] / 6 || newCub[index[j]][0] > newCub[index[i]][0]
                    || newCub[index[j]][1] > newCub[index[i]][1] || newCub[index[j]][2] > newCub[index[i]][2]) continue;
                
                dp[i] = Math.max(dp[i], dp[j] + newCub[index[i]][2]);
            }
            max = Math.max(max, dp[i]);
        }
        
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：82 ms, 在所有 Java 提交中击败了9.97%的用户。

内存消耗：37.6 MB, 在所有 Java 提交中击败了97.26%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用巧妙思路避免列出所有的立方体形式。假设两个立方体为$(a_1, b_1, c_1)$和$(a_2, b_2, c_2)$，且$a_1 > a_2$，$b_1 > b_2$，$c_1 > c_2$，则两个立方体可在$ab$面、$ac$面、$bc$面拼接，由于题目要求最高，则可选择高度最高的边，另一面作为拼接面。

```java
class Solution {
    public int maxHeight(int[][] cuboids) {
        // 排序立方体边长
        for (int[] arr : cuboids) Arrays.sort(arr);
        // 排序立方体大小
        Arrays.sort(cuboids, (a, b) -> a[0] == b[0] ? a[1] == b[1] ? a[2] - b[2] : a[1] - b[1] : a[0] - b[0]);

        int res = 0;
        // 以i作为底的最大高度
        int[] dp = new int[cuboids.length];
        for (int i = 0; i < cuboids.length; i++) {
            dp[i] = cuboids[i][2];
            for (int j = 0; j < i; j++) {
                if (cuboids[j][0] <= cuboids[i][0] && cuboids[j][1] <= cuboids[i][1] && cuboids[j][2] <= cuboids[i][2]) dp[i] = Math.max(dp[i], dp[j] + cuboids[i][2]);
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：8 ms, 在所有 Java 提交中击败了44.39%的用户。

内存消耗：37.7 MB, 在所有 Java 提交中击败了89.28%的用户。