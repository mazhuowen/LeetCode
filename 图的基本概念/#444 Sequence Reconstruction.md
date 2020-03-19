[toc]

Check whether the original sequence `org` can be uniquely reconstructed from the sequences in `seqs`. The `org` sequence is a permutation of the integers from $1$ to $n$, with $1 \le n \le 10^4$. Reconstruction means building a shortest common supersequence of the sequences in `seqs` (i.e., a shortest sequence so that all sequences in `seqs` are subsequences of it). Determine whether there is only one sequence that can be reconstructed from `seqs` and it is the `org` sequence.



## 题目解读

&emsp;给定一组有依赖关系的序列和数组，检查这组序列拓扑排序是否是唯一的，且就是给定数组的形式。

> 该题边角条件非常多，很多都是出乎意料的，甚至有悖于常理，需要面面俱到考虑验证。首先题目规定结点`1～N`编号，但给出的边却有超过`N`的数，甚至是负数；其次给出的序列可能是空或只有一个点，稍不注意会数组越界；再次不同于常规的拓扑排序，比如结点数为1，依赖关系为空，则输出序列1也是正确答案，本题要求序列中的点必须在给出的边中出现，故需要额外的记录哪些点在边中出现。

```java
class Solution {
    public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {

    }
}
```

## 程序设计

* 对于一个有向图，当无环时存在拓扑排序；而唯一确定则表示拓扑排序每一层只有一个结点，且整个图是连通的，这意味这在拓扑排序的过程中，每次入度为0的结点只有一个。
* 除了上述整体思路，还要考虑无关的边角条件，如给定的数组元素少于结点、给定的数组元素多于结点等情况。

```java
class Solution {
    public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {
        // 没有依赖关系
        if (seqs == null || org == null) throw new IllegalArgumentException("invalid param");

        // 图结构
        Map<Integer, Set<Integer>> graph = new HashMap<>();
        // 统计入度
        Map<Integer, Integer> degree = new HashMap<>();
        for (List<Integer> seq : seqs) {
            // 特殊情况，输入存在为空的情况
            if (seq.isEmpty()) continue;
            // 避免漏掉第一个点
            degree.putIfAbsent(seq.get(0), 0);
            int from = -1;
            for (int to : seq) {
                if (from != -1) {
                    graph.computeIfAbsent(from, k -> new HashSet<>());
                    // 防止重复加入
                    if (!graph.get(from).contains(to)) {
                        degree.put(to, degree.getOrDefault(to, 0) + 1);
                    }
                    graph.get(from).add(to);
                }
                from = to;
            }
        }
        // 初始化队列
        Queue<Integer> queue = new LinkedList<>();
        for (Map.Entry<Integer, Integer> entry : degree.entrySet()) {
            if (entry.getValue() == 0) queue.add(entry.getKey());
        }
        // org遍历索引
        int idx = 0;
        while (!queue.isEmpty()) {
            // 无法唯一决定依赖
            if (queue.size() != 1) return false;
            // 判断是否与org相符
            int cur = queue.poll();
            if (idx >= org.length || cur != org[idx++]) return false;

            if (graph.get(cur) == null) continue;
            for (int next : graph.get(cur)) {
                // cur出发的边入度减少
                degree.put(next, degree.get(next) - 1);
                // 入度为0，加入
                if (degree.get(next) == 0) queue.add(next);
            }
        }
        if (idx < org.length) return false;
        // 存在环
        for (int deg : degree.values()) {
            if (deg > 0) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：81ms，在所有java提交中击败了14.71%的用户。

内存消耗：54.1MB，在所有java提交中击败了35.71%的用户。

## 官方解题

&emsp;暂无，密切关注。社区思路大致相同。注意到题目中说结点是`1～N`，且数组中不存在重复点，故没必要使用集合类构建图，将上述代码中的集合类使用数组加实现，如下：

```java
class Solution {
    public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {
        // 没有依赖关系
        if (seqs == null || org == null) throw new IllegalArgumentException("invalid param");

        int verNum = org.length;
        // 图
        Node[] graph = new Node[verNum + 1];
        // 入度
        int[] degree = new int[verNum + 1];
        // 标记结点是否在边中出现（由于本题要求序列出现的结点必须在给出的序列中出现过，故需要记录在边中出现过的结点）
        boolean[] flag = new boolean[verNum + 1];
        for (List<Integer> seq : seqs) {
            // 输入存在为空的情况
            if (seq.isEmpty()) continue;
            int from = -1;
            for (int to : seq) {
                // 结点索引为负数或大于规定的结点数目
                if (to > verNum || to <= 0) {
                    return false;
                }
                if (from != -1) {
                    // 存在重复边，不加入图，但是重复边不影响结果，故注释掉
                    //Node temp = graph[from];
                    //while (temp != null) {
                    //      if (temp.ver == to) break;
                    //      temp = temp.next;
                    //}
                    //if (temp != null && temp.ver == to) continue;

                    graph[from] = new Node(to, graph[from]);
                    degree[to]++;
                }
                flag[to] = true;
                from = to;
            }
        }
        // 初始化队列
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 1; i <= verNum; i++) {
            if (flag[i] && degree[i] == 0) queue.add(i);
        }
        // org遍历索引
        int idx = 0;
        while (!queue.isEmpty()) {
            // 无法唯一决定依赖
            if (queue.size() != 1) return false;
            // 判断是否与org相符
            int cur = queue.poll();
            // org短于序列或值不相等
            if (idx >= org.length || cur != org[idx++]) return false;

            Node temp = graph[cur];
            while (temp != null) {
                // 入度为0，加入
                if (--degree[temp.ver] == 0) queue.add(temp.ver);
                temp = temp.next;
            }
        }
        // org长于序列
        if (idx < org.length) return false;
        // 存在环或孤点
        for (int i = 1; i <= verNum; i++) {
            if (flag[i] && degree[i] > 0) return false;
        }
        return true;
    }
}

class Node {
    int ver;
    Node next;

    Node(int ver, Node next) {
        this.ver = ver;
        this.next = next;
    }
}
```

&emsp;不使用集合类，时间性能有提升。

执行用时：11ms，在所有java提交中击败了98.53%的用户。

内存消耗：44.3MB，在所有java提交中击败了50.00%的用户。