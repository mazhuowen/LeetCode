[toc]

小王来到了游乐园，她玩的第一个项目是模拟推销员。有一个二维平面地图，其中散布着 $N$ 个推销点，编号 $0$ 到 $N-1$，不存在三点共线的情况。每两点之间有一条直线相连。游戏没有规定起点和终点，但限定了每次转角的方向。首先，小王需要先选择两个点分别作为起点和终点，然后从起点开始访问剩余 $N-2$ 个点恰好一次并回到终点。访问的顺序需要满足一串给定的长度为 $N-2$ 由 `L` 和 `R` 组成的字符串 `direction`，表示从起点出发之后在每个顶点上转角的方向。根据这个提示，小王希望你能够帮她找到一个可行的遍历顺序，输出顺序下标（若有多个方案，输出任意一种）。可以证明这样的遍历顺序一定是存在的。

<img src="..\images\#lcp15_1.png" style="zoom: 50%;" />

（上图：A->B->C 右转； 下图：D->E->F 左转）



**示例 1**：

```
输入：points = [[1,1],[1,4],[3,2],[2,1]], direction = "LL"

输入：[0,2,1,3]

解释：[0,2,1,3] 是符合"LL"的方案之一。在 [0,2,1,3] 方案中，0->2->1 是左转方向， 2->1->3 也是左转方向 
```

<img src="..\images\#lcp15_exp1.gif" style="zoom:67%;" />

**示例 2**：

```
输入：points = [[1,3],[2,4],[3,3],[2,1]], direction = "LR"

输入：[0,3,1,2]

解释：[0,3,1,2] 是符合"LR"的方案之一。在 [0,3,1,2] 方案中，0->3->1 是左转方向， 3->1->2 是右转方向
```



**限制**：

* $3 \le \text{points.length} \le 1000$ 且 $\text{points[i].length} == 2$
* $1 \le \text{points[i][0],points[i][1]} \le 10000$
* $\text{direction.length} == \text{points.length} - 2$
* `direction` 只包含 `"L"`,`"R"`



## 题目解读

&emsp;给定点的数组及指令，求可得到该指令连线的点的顺序。

```java
class Solution {
    public int[] visitOrder(int[][] points, String direction) {

    }
}
```

## 程序设计

* 参考官方思路，使用贪婪法选择，如果指令是`L`，则选择线段使得剩余的点都在左侧，反之亦然；
* 问题转化为怎么判断一个线段在另一个线段左侧或右侧；可使用向量叉积来判断。

<img src="..\images\lcp15_2.png" style="zoom: 67%;" />

```java
class Solution {
    public int[] visitOrder(int[][] points, String direction) {
        // 转换方便处理
        int n = points.length;
        Point[] record = new Point[n];
        for (int i = 0; i < n; i++) record[i] = new Point(points[i], i);
        
        // 当前线段的起始点与结束点
        Point start = null, next = null;
        // 将起始点设为最左端点
        for (Point p : record) if (start == null || p.x < start.x) start = p;
        int[] res = new int[n];
        // 起始点设为最左侧点，并删除该点
        res[0] = start.idx;
        record[start.idx] = record[--n];

        for (int i = 0; i < direction.length(); i++) {
            char c = direction.charAt(i);
            int idx = -1;
            // 更左侧小于零，更右侧大于零
            int flag = c == 'L' ? 1 : -1;
            for (int j = 0; j < n; j++) {
                if (next == null || flag * cross(start.getVec(next), start.getVec(record[j])) < 0) {
                    next = record[j];
                    idx = j;
                }
            }

            // 加入结果并删除选中的点
            res[i + 1] = next.idx;
            record[idx] = record[--n];
            // 设置下一次出发点
            start = next;
            next = null;
        }
        res[points.length - 1] = record[0].idx;
        return res;
    }

    private int cross(int[] vec1, int[] vec2) {
        return vec1[0] * vec2[1] - vec1[1] * vec2[0];
    }
}

class Point {
    int x, y;
    int idx;

    Point(int[] point, int idx) {
        this.x = point[0];
        this.y = point[1];
        this.idx = idx;
    }

    public int[] getVec(Point that) {
        return new int[]{that.x - this.x, that.y - this.y};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：28 ms, 在所有 Java 提交中击败了75.00%的用户

内存消耗：39.2 MB, 在所有 Java 提交中击败了54.17%的用户

## 官方解题

&emsp;上述思路参考官方解题。