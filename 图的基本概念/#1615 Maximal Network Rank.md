[toc]

There is an infrastructure of $n$ cities with some number of `roads` connecting these cities. Each `roads[i] = [ai, bi]` indicates that there is a bidirectional road between cities `ai` and `bi`.

The **network rank** of **two different cities** is defined as the total number of **directly** connected roads to **either** city. If a road is directly connected to both cities, it is only counted **once**.

The **maximal network rank** of the infrastructure is the **maximum network rank** of all pairs of different cities.

Given the integer $n$ and the array `roads`, return the **maximal network rank** of the entire infrastructure.



**Constraints**:

* $2 \le n \le 100$
* $0 \le \text{roads.length} \le n * (n - 1) / 2$
* $\text{roads[i].length} == 2$
* $0 \le \text{ai, bi} \le n-1$
* $\text{ai} \ne \text{bi}$
* Each pair of cities has **at most one** road connecting them.



## 题目解读

&emsp;定义城市间的网络秩为与两座城市直接相连的道路总数，如果存在一条道路连接两个城市，只记作一条。求最大的网络秩。

```java
class Solution {
    public int maximalNetworkRank(int n, int[][] roads) {
        
    }
}
```

## 程序设计

* 最基本的思路就是统计每个城市的秩，然后遍历计算城市两两之间的网络秩，如果存在一条边连接了这两个城市，则从计算结果中减去一。

```java
class Solution {
    public int maximalNetworkRank(int n, int[][] roads) {
        // 每个城市的秩
        int[] counter = new int[n];
        // 记录每个城市相连的城市
        Map<Integer, Set<Integer>> graph = new HashMap<>();
        for (int i = 0; i < n; i++) graph.put(i, new HashSet<>());
        for (int[] road : roads) {
            int ver1 = road[0], ver2 = road[1];
            counter[ver1]++;
            counter[ver2]++;
            graph.get(ver1).add(ver2);
            graph.get(ver2).add(ver1);
        }

        int max = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j) continue;
                max = Math.max(max, counter[i] + counter[j] - (graph.get(i).contains(j) ? 1 : 0));
            }
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：25ms，在所有java提交中击败了54.29%的用户。

内存消耗：39.2MB，在所有java提交中击败了89.42%的用户。

## 官方解题

&emsp;暂无，密切关注。社区进一步细化思路，显而易见最大网络秩存在以下几种情况：

* 只存在一个最大值，若次大值都存在直连的边，结果为最大值加次大值减去一；若次大值中不存在直连的边，则结果为最大值加次大值；
* 存在多个最大值，假设有$x$个城市，由于若这$x$个城市互连接，则边的数目为$\frac{x(x - 1)}{2}$，如果大于总的边数，则说明必然有城市不相连，故结果为两个最大值之和；否则需要双循环遍历，由于$\frac{x(x - 1)}{2} \le V$，故时间复杂度不超过$O(V)$。

```java
class Solution {
    public int maximalNetworkRank(int n, int[][] roads) {
        int[] counter = new int[n];
        Map<Integer, Set<Integer>> graph = new HashMap<>();
        for (int i = 0; i < n; i++) graph.put(i, new HashSet<>());
        for (int[] road : roads) {
            int ver1 = road[0], ver2 = road[1];
            graph.get(ver1).add(ver2);
            graph.get(ver2).add(ver1);
            counter[ver1]++;
            counter[ver2]++;
        }

        // 统计最大值和次大值
        int max = -1, sec = -1;
        for (int num : counter) {
            if (num > max) {
                sec = max;
                max = num;
            } else if (num > sec) sec = num;
        }
        List<Integer> maxList = new LinkedList<>(), secList = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (counter[i] == max) maxList.add(i);
            else if (counter[i] == sec) secList.add(i);
        }

        // 遍历次小值
        if (maxList.size() == 1) {
            int i = maxList.get(0);
            for (int j : secList) if (!graph.get(i).contains(j)) return max + sec;
            return max + sec - 1;
        } 
        // 遍历最大值
        else {
            int size = maxList.size();
            if (size * (size - 1) / 2 > roads.length) return 2 * max;
            for (int i : maxList) {
                for (int j : maxList) {
                    if (i != j && !graph.get(i).contains(j)) return 2 * max;
                }
            }
            return 2 * max - 1;
        }
    }
}
```

&emsp;时间复杂度为$O(\max(V,E))$，空间复杂度为$O(E)$。

执行用时：21ms，在所有java提交中击败了58.24%的用户。

内存消耗：39.5MB，在所有java提交中击败了60.11%的用户。