[toc]

You are given $n$ `points` in the plane that are all **distinct**, where `points[i] = [xi, yi]`. A **boomerang** is a tuple of points $(i, j, k)$ such that the distance between $i$ and $j$ equals the distance between $i$ and $k$ (**the order of the tuple matters**).

Return the number of boomerangs.

 

**Example 1**:

```
Input: points = [[0,0],[1,0],[2,0]]
Output: 2
Explanation: The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]].
```

**Example 2**:

```
Input: points = [[1,1],[2,2],[3,3]]
Output: 2
```

**Example 3**:

```
Input: points = [[1,1]]
Output: 0
```



**Constraints**:

* $n == \text{points.length}$
* $1 \le n \le 500$
* $\text{points[i].length} == 2$
* $-10^4 \le x_i, y_i \le 10^4$
* All the points are **unique**.



## 题目解读

&emsp;求点$i$到$j$和$i$到$k$的距离相等的三元组数目，注意三元组索引相等顺序不同仍视为不同。

```java
class Solution {
    public int numberOfBoomerangs(int[][] points) {

    }
}
```

## 程序设计

* 首先遍历$i$点，再遍历$j$和$k$，为了节省时间，可使用哈希表保存之前遍历的结果。

```java
class Solution {
    public int numberOfBoomerangs(int[][] points) {
        Arrays.sort(points, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int res = 0;
        for (int i = 0; i < points.length; i++) {
            Map<Integer, Integer> record = new HashMap<>();
            for (int j = 0; j < points.length; j++) {
                if (i == j) continue;
                int dis = distance(points[i], points[j]);
                // 由于顺序问题，乘以2,表示j在中间和在后面的位置
                if (record.containsKey(dis)) res += 2 * record.get(dis);
                // 计数加一
                record.put(dis, record.getOrDefault(dis, 0) + 1);
            }
        }
        return res;
    }

    private int distance(int[] p1, int[] p2) {
        return (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：153 ms, 在所有 Java 提交中击败了74.53%的用户。

内存消耗：38.7 MB, 在所有 Java 提交中击败了16.67%的用户。

## 官方解题

&emsp;暂无，密切关注。
