[toc]

为了给刷题的同学一些奖励，力扣团队引入了一个弹簧游戏机。游戏机由 $N$ 个特殊弹簧排成一排，编号为 $0$ 到 $N-1$。初始有一个小球在编号 $0$ 的弹簧处。若小球在编号为 $i$ 的弹簧处，通过按动弹簧，可以选择把小球向右弹射 `jump[i]` 的距离，或者向左弹射到任意左侧弹簧的位置。也就是说，在编号为 $i$ 弹簧处按动弹簧，小球可以弹向 $0$ 到 $i-1$ 中任意弹簧或者 `i+jump[i]` 的弹簧（若 `i+jump[i]>=N` ，则表示小球弹出了机器）。小球位于编号 $0$ 处的弹簧时不能再向左弹。

为了获得奖励，你需要将小球弹出机器。请求出最少需要按动多少次弹簧，可以将小球从编号 $0$ 弹簧弹出整个机器，即向右越过编号 $N-1$ 的弹簧。



**示例 1**：

```
输入：jump = [2, 5, 1, 1, 1, 1]

输出：3

解释：小 Z 最少需要按动 3 次弹簧，小球依次到达的顺序为 0 -> 2 -> 1 -> 6，最终小球弹出了机器。
```



**限制**：

* $1 \le \text{jump.length} <= 10^6$
* $1 \le \text{jump[i]} \le 10000$



## 题目解读

&emsp;求将球弹出的最少步数。

```java
class Solution {
    public int minJump(int[] jump) {

    }
}
```

## 程序设计

* 最基本的思路是广度优先搜索，会超时。

```java
class Solution {
    public int minJump(int[] jump) {
        int n = jump.length;
        // 记录到达的位置和步数
        Map<Integer, Integer> visited = new HashMap<>();

        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        queue.add(new int[]{0, 0});
        visited.put(0, 0);
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            int pos = cur[0], step = cur[1];
            // 向前跳
            int forward = pos + jump[pos];
            if (forward >= n) return step + 1;
            if (visited.getOrDefault(forward, Integer.MAX_VALUE) > step + 1) {
                queue.add(new int[]{forward, step + 1});
                visited.put(forward, step + 1);

                for (int backward = pos + 1; backward < forward; backward++) {
                    if (visited.getOrDefault(backward, Integer.MAX_VALUE) < step + 2) continue;
                    queue.add(new int[]{backward, step + 2});
                    visited.put(backward, step + 2);
                }
            }
        }
        return -1;
    }
}
```

* 改进上述思路，进一步细化回退范围。

```java
class Solution {
    public int minJump(int[] jump) {
        // 之前已遍历位置
        int pre = 0;
        boolean[] visited = new boolean[jump.length];
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0});
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            int pos = cur[0], step = cur[1];
            if (pos + jump[pos] >= jump.length) return step + 1;
            // 前进
            if (!visited[pos + jump[pos]]) {
                visited[pos + jump[pos]] = true;
                queue.add(new int[]{pos + jump[pos], step + 1});
            }
            // 回退
            while (pre < pos) {
                if (!visited[pre]) {
                    visited[pre] = true;
                    queue.add(new int[]{pre, step + 1});
                } 
                pre++;
            }
            // 之前已遍历，更新
            if (pos + 1 > pre) pre = pos + 1;
        }
        return -1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：46 ms, 在所有 Java 提交中击败了41.15%的用户

内存消耗：151.6 MB, 在所有 Java 提交中击败了37.08%的用户。

## 官方解题

&emsp;官方除了上述思路，还采用动态规划。

```java
class Solution {
    public int minJump(int[] jump) {
        // 表示当前位置出界的最少步数
        int[] dp = new int[jump.length];
        // 从后往前更新
        for (int i = jump.length - 1; i >= 0; i--) {
            // 更新前跳
            if (i + jump[i] >= jump.length) dp[i] = 1;
            else dp[i] = dp[i + jump[i]] + 1;
            // 更新回退（注意dp[i] < dp[j]，因为j之后的都可回退到j，这意味这不大于dp[i]，故无需再次遍历）
            for (int j = i + 1; j < jump.length && j < i + jump[i] && dp[i] < dp[j]; j++) {
                dp[j] = dp[i] + 1;
            }
        }
        return dp[0];
    }
}
```

执行用时：14 ms, 在所有 Java 提交中击败了71.19%的用户。

内存消耗：118.1 MB, 在所有 Java 提交中击败了67.08%的用户。