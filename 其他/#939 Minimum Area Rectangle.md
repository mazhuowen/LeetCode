[toc]

Given a set of points in the xy-plane, determine the minimum area of a rectangle formed from these points, with sides parallel to the `x` and `y` axes.

If there isn't any rectangle, return 0.



Note:

* $1 \le \text{points.length} \le 500$
* $0 \le \text{points[i][0]} \le 40000$
* $0 \le \text{points[i][1]} \le 40000$
* All points are distinct.



## 题目解读

&emsp;计算给定坐标所能组合的最小矩形面积。

```java
class Solution {
    public int minAreaRect(int[][] points) {

    }
}
```

## 程序设计

* 由于只需计算最小矩形面积，故可以记录和当前边等长的最近的边所在行。

```java
class Solution {
    public int minAreaRect(int[][] points) {
        if (points == null || points.length < 4) return 0;

        // 将行从低到高，维护每行的点的列索引
        Map<Integer, List<Integer>> rows = new TreeMap<>();
        for (int[] point : points) {
            if (rows.get(point[0]) == null) rows.put(point[0], new ArrayList<>());
            rows.get(point[0]).add(point[1]);
        }

        int minArea = Integer.MAX_VALUE;
        // 记录长度为k的行
        Map<Integer, Integer> record = new HashMap<>();
        for (Integer row : rows.keySet()) {
            List<Integer> cols = rows.get(row);
            // 当前行没有足够的点构成边
            if (cols.size() < 2) continue;

            Collections.sort(cols);
            for (int i = 0; i < cols.size() - 1; i++) {
                for (int j = i + 1; j < cols.size(); j++) {
                    // 边i~j的编号
                    int code = 40001 * cols.get(i) + cols.get(j);

                    // 存在之前行的边，计算面积
                    if (record.containsKey(code)) {
                        minArea = Math.min(minArea, (cols.get(j) - cols.get(i)) * (row - record.get(code)));
                    }
                    // 更新或加入最新的边
                    record.put(code, row);
                }
            }
        }
        return minArea == Integer.MAX_VALUE ? 0 : minArea;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：203ms，在所有java提交中击败了63.19%的用户。

内存消耗：48.2MB，在所有java提交中击败了66.67%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了枚举对角线的思路。

```java
class Solution {
    public int minAreaRect(int[][] points) {
        if (points == null || points.length < 4) return 0;
        Set<Integer> set = new HashSet<>();
        for (int[] point : points) set.add(40001 * point[0] + point[1]);

        int minArea = Integer.MAX_VALUE;
        // 遍历对角线顶点，并判断
        for (int i = 0; i < points.length - 1; i++) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = i + 1; j < points.length; j++) {
                int x2 = points[j][0], y2 = points[j][1];
                // 非对角线
                if (x1 == x2 || y1 == y2) continue;

                // 存在另两个顶点，计算面积
                if (set.contains(40001 * x1 + y2) && set.contains(40001 * x2 + y1)) {
                    minArea = Math.min(minArea, Math.abs((x1 - x2) * (y1 - y2)));
                }
            }
        }
        return minArea == Integer.MAX_VALUE ? 0 : minArea;
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：143ms，在所有java提交中击败了84.37%的用户。

内存消耗：40.1MB，在所有java提交中击败了100.00%的用户。