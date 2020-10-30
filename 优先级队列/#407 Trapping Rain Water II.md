[toc]

Given an $m \times n$ matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.



**Note**:

Both m and n are less than 110. The height of each unit cell is greater than 0 and is less than 20,000.



Example:

Given the following $3 \times 6$ height map:
`[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]`

Return 4.

<img src="../images/#407_1.png" style="zoom:70%;" /><img src="../images/#407_2.png" style="zoom:70%;" />

## 题目解读

&emsp;给定矩阵，长宽索引表示图中的柱形的位置，矩阵中的值表示对应位置柱形的高度，需得出积水量。

```java
class Solution {
    public int trapRainWater(int[][] heightMap) {
        
    }
}
```

## 程序设计

* 参考社区常见思路，将围墙入队，迭代计算低于最低围墙的柱形积水面积，并更新围墙，直到全部访问。

```java
class Solution {
    public int trapRainWater(int[][] heightMap) {
        int m, n;
        if((m = heightMap.length) < 3 || (n = heightMap[0].length)< 3) {
            return 0;
        }
        int areaSum = 0;
        // 记录是否访问过
        boolean[][] visit = new boolean[m][n];
        // 最小堆记录围墙
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        // 入队围墙
        // 最外两行
        for(int col = 0; col < n; col++) {
            queue.add(new int[]{0, col, heightMap[0][col]});
            queue.add(new int[]{m - 1, col, heightMap[m - 1][col]});
            visit[0][col] = true;
            visit[m - 1][col] = true;
        }
        // 最外两列
        for(int row = 1; row < m - 1; row++) {
            queue.add(new int[]{row, 0, heightMap[row][0]});
            queue.add(new int[]{row, n - 1, heightMap[row][n - 1]});
            visit[row][0] = true;
            visit[row][n - 1] = true;
        }
        // 结点前后左右
        int[][] boudary = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        while(!queue.isEmpty()) {
            int[] lowest = queue.poll();
            int x = lowest[0], y = lowest[1], height = lowest[2];
            // 当前围墙前后左右的结点判断
            for(int[] opera : boudary) {
                int newX = x + opera[0], newY = y + opera[1];
                // 未访问则判断，若当前元素高于最低围墙，新的围墙入队，否则计算积水并入队，高度为旧的围墙高度
                if(newX > 0 && newX < m - 1 && newY > 0 && newY < n - 1 && !visit[newX][newY]) {
                    // 当前元素低于围墙，计算积水
                    areaSum += Math.max(0, height - heightMap[newX][newY]);
                    // 入队最新的围墙
                    queue.add(new int[]{newX, newY, Math.max(height, heightMap[newX][newY])});
                    visit[newX][newY] = true;
                }
            }
        }
        return areaSum;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM\log_2NM)$，空间复杂度为$O(NM)$。

执行用时：21ms，在所有java提交中击败了47.06%的用户。

内存消耗：47.9MB，在所有java提交中击败了10.07%的用户。

## 官方解题

&emsp;暂无，密切关注。