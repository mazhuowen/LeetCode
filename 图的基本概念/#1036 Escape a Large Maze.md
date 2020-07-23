[toc]

In a 1 million by 1 million grid, the coordinates of each grid square are `(x, y)` with $0 \le x, y < 10^6$.

We start at the `source` square and want to reach the `target` square.  Each move, we can walk to a 4-directionally adjacent square in the grid that isn't in the given list of `blocked` squares.

Return `true` if and only if it is possible to reach the target square through a sequence of moves.



Note:

* $0 \le \text{blocked.length} \le 200$
* $\text{blocked[i].length} == 2$
* $0 \le \text{blocked[i][j]} < 10^6$
* $\text{source.length} == \text{target.length} == 2$
* $0 \le \text{source[i][j], target[i][j]} < 10^6$
* $\text{source} \ne \text{target}$



## 题目解读

&emsp;给定非常大的迷宫中的障碍物，判断是否可从起始点到达目标点。

```java
class Solution {
    public boolean isEscapePossible(int[][] blocked, int[] source, int[] target) {

    }
}
```

## 程序设计

* 当障碍物将起始点或目标点分割开时，无法到达。由于平面是无限的，使用原始的深度优先搜索或广度优先搜索必然超时；考虑到障碍物最多只有$200$个，若障碍物包围起始点或目标点，则可从该点出发遍历，若被障碍物包围，必然最后广度优先搜索队列为空。
* 由于不知道是起始点还是目标点会被障碍物包围，故可从两个点同时广度优先搜索，若其中有一个点在包围圈内，必然会在$200$层广度优先内结束。
* 存在两个点都在包围圈里的情况，还需分别记录两端广度优先搜索的已遍历的点，若有交集则可达。

```java
class Solution {
    // 广度优先搜索限制步数
    private final static int LIMIT = 201;
    // 列的最大数字
    private final static long COL = 1_000_000L;

    int[] delta = new int[]{0, 1, 0, -1, 0};

    public boolean isEscapePossible(int[][] blocked, int[] source, int[] target) {
        if (blocked == null || blocked.length == 0) return true;
		// 记录障碍物
        Set<Long> blocks = new HashSet<>();
        for (int[] block : blocked) blocks.add(block[0] * COL + block[1]);
        
        // 记录起始端遍历点
        Set<Long> sVisited = new HashSet<>();
        Queue<int[]> start = new LinkedList<>();
        start.add(source);
        // 记录目标点端遍历点
        Set<Long> eVisited = new HashSet<>();
        Queue<int[]> end = new LinkedList<>();
        end.add(target);

        int step = 0;
        // 达到限定步数跳出循环（说明没有包围圈，可达）或某一端为空（说明有包围圈，不可达）
        while (step < LIMIT && !start.isEmpty() && !end.isEmpty()) {
            // 起始端
            int size = start.size();
            for (int i = 0; i < size; i++) {
                int[] cur = start.poll();
                // 起始端与结束端相交
                if (eVisited.contains(cur[0] * COL + cur[1])) return true;
                for (int j = 0; j < 4; j++) {
                    int newX = cur[0] + delta[j], newY = cur[1] + delta[j + 1];
                    if (newX < 0 || newX >= 1_000_000 || newY < 0 || newY >= 1_000_000 || sVisited.contains(newX * COL + newY) || blocks.contains(newX * COL + newY)) continue;
                    sVisited.add(newX * COL + newY);
                    start.add(new int[]{newX, newY});
                }
            }
            // 结束端
            size = end.size();
            for (int i = 0; i < size; i++) {
                int[] cur = end.poll();
                // 起始端与结束端相交
                if (sVisited.contains(cur[0] * COL + cur[1])) return true;
                for (int j = 0; j < 4; j++) {
                    int newX = cur[0] + delta[j], newY = cur[1] + delta[j + 1];
                    if (newX < 0 || newX >= 1_000_000 || newY < 0 || newY >= 1_000_000 || eVisited.contains(newX * COL + newY) || blocks.contains(newX * COL + newY)) continue;
                    eVisited.add(newX * COL + newY);
                    end.add(new int[]{newX, newY});
                }
            }
            step++;
        }
        // 判断
        return step == LIMIT;
    }
}
```

## 性能分析

执行用时：608ms，在所有java提交中击败了18.83%的用户。

内存消耗：57.2MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较高的方法采用深度优先搜索的方式。

```java
class Solution {
    // 广度优先搜索限制步数
    private final static int LIMIT = 201;
    // 列的最大数字
    private final static long COL = 1_000_000L;

    int[] delta = new int[]{0, 1, 0, -1, 0};

    public boolean isEscapePossible(int[][] blocked, int[] source, int[] target) {
        if (blocked == null || blocked.length == 0) return true;

        Set<Long> blocks = new HashSet<>();
        for (int[] block : blocked) blocks.add(block[0] * COL + block[1]);

        int s = dfs(source, source, target, blocks, new HashSet<>());
        if (s == 2) return true;
        else if (s == 0) return false;
        else return dfs(target, target, source, blocks, new HashSet<>()) != 0;
    }

    // 在包围圈内返回0；不在包围圈内返回1；直接可达终点，返回2
    private int dfs(int[] cur, int[] source, int[] target, Set<Long> blocks, Set<Long> visited) {
        // 可达
        if (cur[0] == target[0] && cur[1] == target[1]) return 2;
        // 表明source没有包围圈需要检测target是否有包围圈
        if (Math.abs(cur[0] - source[0]) + Math.abs(cur[1] - source[1]) > 200) return 1;

        for (int i = 0; i < 4; i++) {
            int newX = cur[0] + delta[i], newY = cur[1] + delta[i + 1];
            if (newX < 0 || newX >= 1_000_000 || newY < 0 || newY >= 1_000_000) continue;
            long flag = newX * COL + newY;
            if (blocks.contains(flag) || visited.contains(flag)) continue;
            visited.add(flag);
            int res = dfs(new int[]{newX, newY}, source, target, blocks, visited);
            if (res != 0) return res; 
        }
        return 0;
    }
}
```

执行用时：17ms，在所有java提交中击败了93.51%的用户。

内存消耗：46MB，在所有java提交中击败了25.00%的用户。