[toc]

We are given a list of (axis-aligned) `rectangles`.  Each `rectangle[i] = [x1, y1, x2, y2]` , where `(x1, y1)` are the coordinates of the bottom-left corner, and `(x2, y2)` are the coordinates of the top-right corner of the `i`th rectangle.

Find the total area covered by all `rectangles` in the plane.  Since the answer may be too large, **return it modulo $10^9 + 7$**.

<img src="../images/#850.png" style="zoom: 33%;" />

Note:

* $1 \le \text{rectangles.length} \le 200$
* $\text{rectanges[i].length} = 4$
* $0 \le \text{rectangles[i][j]} \le 10^9$
* The total area covered by all rectangles will never exceed $2^{63} - 1$ and thus will fit in a 64-bit signed integer.



## 题目解读

&emsp;给定矩形坐标，计算所有矩形覆盖的总面积。题目说明如果数值较大还需要取余。

```java
class Solution {
    public int rectangleArea(int[][] rectangles) {

    }
}
```

## 程序设计

* 官方提出了利用相交集性质计算但是会报超时；官方还提出了压缩`x`、`y`坐标为连续值，然后遍历矩形将覆盖区域标记为`true`，最后再遍历计算每一个坐标单元的面积，最后相加得到结果。

```java
class Solution {
    public int rectangleArea(int[][] rectangles) {
        int n = rectangles.length;
        // 坐标压缩
        Set<Integer> xAxis = new HashSet<>();
        Set<Integer> yAxis = new HashSet<>();
        for(int[] rectangle : rectangles) {
            xAxis.add(rectangle[0]);
            xAxis.add(rectangle[2]);
            yAxis.add(rectangle[1]);
            yAxis.add(rectangle[3]);
        }
        // 排序坐标
        Integer[] x = xAxis.toArray(new Integer[0]);
        Arrays.sort(x);
        Integer[] y = yAxis.toArray(new Integer[0]);
        Arrays.sort(y);
        int idx = 0;
        // 坐标对应
        Map<Integer, Integer> xMap = new HashMap<>();
        for(int num : x) {
            xMap.put(num, idx++);
        }
        idx = 0;
        Map<Integer, Integer> yMap = new HashMap<>();
        for(int num : y) {
            yMap.put(num, idx++);
        }
        // 遍历标记
        boolean[][] graph = new boolean[x.length][y.length];
        for(int[] rectangle : rectangles) {
            // 将矩形区域标记为true（注意由于是按照坐标单位计算面积，此处不包含后区间，不是<=）
            for(int i = xMap.get(rectangle[0]); i < xMap.get(rectangle[2]); i++) {
                for(int j = yMap.get(rectangle[1]); j < yMap.get(rectangle[3]); j++) {
                    graph[i][j] = true;
                }
            }
        }
        // 计算面积
        long area = 0;
        for(int i = 0; i < graph.length; i++) {
            for(int j = 0; j < graph[0].length; j++) {
                if(graph[i][j])
                    area += ((long)x[i + 1] - x[i]) * (y[j + 1] - y[j]);
            }
        }
        area %= 1_000_000_007;
        return (int)area;
    }
}
```

* 除了从`x`轴观察计算，还可以从`y`轴的角度观察计算，将矩形的顶边和底边加入数组，按照高度排序，然后统计`x`轴的长度，根据高度差计算，通过这样一层层的计算。

```java
class Solution {
    public int rectangleArea(int[][] rectangles) {
        // 矩形数目
        int n = rectangles.length;
        // 存放矩形水平边
        int[][] edges = new int[2 * n][];
        int idx = 0;
        for(int[] rectangle : rectangles) {
            // 分别存放边的高度、底还是高、边的左右坐标
            edges[idx++] = new int[]{rectangle[1], 0, rectangle[0], rectangle[2]};
            edges[idx++] = new int[]{rectangle[3], 1, rectangle[0], rectangle[2]};
        }
        // 按照高度排序
        Arrays.sort(edges, (a, b) -> a[0] - b[0]);
        // 存放x轴坐标用于和高计算，存放开始坐标和结束坐标
        List<int[]> baseX = new ArrayList<>();
        // 存放当前计算参考水平面
        int baseY = 0;
        long area = 0L;
        // 从低到高计算
        for(int[] edge : edges) {
            // 计算x轴的长度
            int lenX = 0;
            // 记录上一次计算的x坐标
            int lastX = 0;
            for(int[] axisX : baseX) {
                // 避免重复计算，取最大的
                lastX = Math.max(lastX, axisX[0]);
                // 长度更新
                lenX += Math.max(axisX[1] - lastX, 0);
                // 更新x坐标
                lastX = Math.max(lastX, axisX[1]);
            }
            // 计算当前高度到基本高度的面积
            area += (long)lenX * (edge[0] - baseY);

            // 底边，将新的x坐标更新入baseX
            if(edge[1] == 0) {
                baseX.add(new int[]{edge[2], edge[3]});
                // 按照起始坐标排序
                Collections.sort(baseX, (a, b) -> a[0] - b[0]);
            }
            // 顶边，意味着这个矩形已计算完成，移除baseX中的坐标
            else {
                for(int i = 0; i < baseX.size(); i++) {
                    if(baseX.get(i)[0] == edge[2] && baseX.get(i)[1] == edge[3]) {
                        baseX.remove(i);
                        break;
                    }
                }
            }
            baseY = edge[0];
        }
        area %= 1_000_000_007;
        return (int)area;
    }
}
```

## 性能分析

&emsp;坐标压缩时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：31ms，在所有java提交中击败了13.73%的用户。

内存消耗：41MB，在所有java提交中击败了8.70%的用户。

&emsp;第二种方法时间复杂度为$O(N^2\log_2N)$，空间复杂度为$O(N)$。

执行用时：10ms，在所有java提交中击败了62.75%的用户。

内存消耗：37.9MB，在所有java提交中击败了8.70%的用户。

## 官方解题

&emsp;除了上述思路，官方提出线段树的最优解法。官方提供了线段树的实现：

```java
class Tree {
    // 区间范围
    int start, end;
    // 左右结点
    Tree left, right;
    // 坐标数组（由于是动态生成子节点，故需要保存叶节点值）
    Integer[] axisX;

    // 计数，当该区间为底边时，大于0；当该区间已计算完成时为0
    int count;
    // 当前区间的x轴长度
    long total;

    Tree(int start, int end, Integer[] axisX) {
        this.start = start;
        this.end = end;
        this.axisX = axisX;
    }

    private int getRangeMid() {
        return start + (end - start) / 2;
    }

    private Tree left() {
        if(left == null) {
            left = new Tree(start, getRangeMid(), axisX);
        }
        return left;
    }

    private Tree right() {
        if(right == null) {
            right = new Tree(getRangeMid(), end, axisX);
        }
        return right;
    }

    public long update(int i, int j, int val) {
        //区间为点不是线段，x轴长度返回0
        if(i >= j) {
            return 0;
        }
        // 找到要更新的区间
        if(start == i && end == j) {
            // 底边加入加一，顶边加入减一
            count += val;
        } else {
            // 创建子区间，最小为[x,x+1]单位区间
            left().update(i, Math.min(getRangeMid(), j), val);
            right().update(Math.max(getRangeMid(), i), j, val);
        }
        // 该区间为底边区间，即未计算完，重新计算该区间的长度（避免被子区间值覆盖）
        if (count > 0) total = axisX[end] - axisX[start];
        // count=0（不可能小于0）1、父区间不是完整的底边区间，为子区间之和； 2、区间已经计算完，区间置为子区间之和；对于叶节点区间就是0
        else total = left().total + right().total;
        // 返回区间可计算x轴长度
        return total;
    }
}
```

> 不同于常规的数组线段树或结点区间线段树，官方的线段树是紧贴题目设计。可以参考动态创建子节点的机制和更新删除逻辑。

```java
class Solution {
    public int rectangleArea(int[][] rectangles) {
        // todo
        int OPEN = 1, CLOSE = -1;
        int[][] edges = new int[rectangles.length * 2][];
        // 记录叶节点
        Set<Integer> Xvals = new HashSet();
        int idx = 0;
        for (int[] rec: rectangles) {
            edges[idx++] = new int[]{rec[1], OPEN, rec[0], rec[2]};
            edges[idx++] = new int[]{rec[3], CLOSE, rec[0], rec[2]};
            Xvals.add(rec[0]);
            Xvals.add(rec[2]);
        }
        // 根据边的高度排序
        Arrays.sort(edges, (a, b) -> a[0] - b[0]);
        // 排序叶节点值
        Integer[] axisX = Xvals.toArray(new Integer[0]);
        Arrays.sort(axisX);
        // 压缩坐标
        Map<Integer, Integer> mapX = new HashMap();
        for (int i = 0; i < axisX.length; ++i)
            mapX.put(axisX[i], i);
        // 创建线段树
        Tree active = new Tree(0, axisX.length - 1, axisX);
        long area = 0;
        // x轴长度
        long lenX = 0;
        int baseY = edges[0][0];

        for (int[] edge: edges) {
            // 计算面积
            area += lenX * (edge[0] - baseY);
            // 获取下一的x轴长度（关键在于底边还是顶边）
            lenX = active.update(mapX.get(edge[2]), mapX.get(edge[3]), edge[1]);
            // 更新基准高度
            baseY = edge[0];
        }

        area %= 1_000_000_007;
        return (int) area;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：8ms，在所有java提交中击败了74.51%的用户。

内存消耗：38.9MB，在所有java提交中击败了8.70%的用户。