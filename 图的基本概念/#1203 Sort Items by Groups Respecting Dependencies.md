[toc]

There are `n` items each belonging to zero or one of `m` groups where `group[i]` is the group that the `i`-th item belongs to and it's equal to `-1` if the `i`-th item belongs to no group. The items and the groups are zero indexed. A group can have no item belonging to it.

Return a sorted list of the items such that:

- The items that belong to the same group are next to each other in the sorted list.
- There are some relations between these items where `beforeItems[i]` is a list containing all the items that should come before the `i`-th item in the sorted array (to the left of the `i`-th item).

Return any solution if there is more than one solution and return an **empty list** if there is no solution.



**Constraints:**

- $1 \le m \le n \le 3*10^4$
- $\text{group.length} == \text{beforeItems.length} == n$
- $-1 \le \text{group[i]} \le m-1$
- $0 \le \text{beforeItems[i].length} \le n-1$
- $0 \le \text{beforeItems[i][j]} \le n-1$
- $i != \text{beforeItems[i][j]}$
- $\text{beforeItems[i]}$does not contain duplicates elements.



## 题目解读

&emsp;给出项目个组的对应关系和项目的依赖关系，排序项目，要求需要依赖其它项目的项目在后面，相互间没有依赖关系的按照归属的组排列。

```java
class Solution {
    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {

    }
}
```

## 程序设计

* 首先想到的是通过广度优先搜索实现的单层的对项目的拓扑排序，每一层整体输出，输出时根据项目归属做排序。但是这个想法不能满足题意。比如**`a->b`**，**`c->d`**，**`a`**和**`b`**一组，**`c`**和**`d`**一组，如果只用一个拓扑排序的话，序列是**`acbd`**，显然一个更好的排列是**`abcd`**，注意到这个差别，可以先对组之间的依赖关系排序，然后在组内再对项目依赖排序。

* 由于题目要求序列顺序，采用深度优先搜索较好。为了实现上述思路，给定了项目到组的映射，还需要维护组和项目集合的映射，然后对组图深度优先遍历，每遍历一个组就对其对应的项目集合拓扑排序。

* 对于不满足条件的情况，无非是项目依赖关系存在环，我们需要在组图和项目图中检测是否存在环，存在则直接返回。

```java
class Solution {
    // 组依赖图及访问标识（1正在访问，0未访问，-1已访问且无环）
    private Node[] grpGraph;
    private int[] grpVisited;
    // 项目依赖图及访问标识
    private Node[] prdGraph;
    private int[] prdVisited;

    // 项目与组映射关系
    private int[] prdToGrp;
    // 组与项目映射关系
    private Map<Integer, Set<Integer>> grpToPrd;
    // 全局变量拼接，节省时间
    int[] res;
    int idx = 0;

    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        if (m < 1 || n < 1 || n < m || n != group.length || n != beforeItems.size()) throw new IllegalArgumentException("invalid param");

        prdToGrp = group;
        grpToPrd = new HashMap<>();
        // 构建项目依赖图和组依赖图
        grpGraph = new Node[m + 1]; // 多一个存放-1，无组情况
        prdGraph = new Node[n];
        grpVisited = new int[m + 1];
        prdVisited = new int[n];

        // 遍历每个项目
        for (int i = 0; i < n; i++) {
            // 当前项目的组，如果没有组则分配组的下标为m
            int curGrp = group[i] == -1 ? m : group[i];
            if (grpToPrd.get(curGrp) == null) grpToPrd.put(curGrp, new HashSet<>());
            grpToPrd.get(curGrp).add(i);

            // 当前项目依赖的项目
            for (Integer item : beforeItems.get(i)) {
                prdGraph[i] = new Node(item, prdGraph[i]);

                int itemGrp = group[item] == -1 ? m : group[item];
                // 维护组依赖，杜绝自环情况
                if (curGrp != itemGrp) {
                    // 存在重复边，不影响拓扑排序
                    grpGraph[curGrp] = new Node(itemGrp, grpGraph[curGrp]);
                }
            }
        }

        // 根据组依赖拓扑排序，深度搜索
        res = new int[n];
        for (int i = 0; i <= m; i++) {
            // 存在环，返回空数组
            if (grpVisited[i] == 0 && !dfsParent(i)) {
                return new int[]{};
            }
        }
        
        return res;
    }

    private boolean dfsParent(int start) {
        // 正在遍历或已经遍历过，返回（1为有环，-1则不必遍历）
        if (grpVisited[start] != 0) return grpVisited[start] == -1;
        // 标记为正在遍历
        grpVisited[start] = 1;
        // 拓扑排序当前组
        Node temp = grpGraph[start];
        while (temp != null) {
            // 后继存在环，则返回失败
            if(!dfsParent(temp.ver)) return false;
            temp = temp.next;
        }
        Set<Integer> childVers = grpToPrd.getOrDefault(start, new HashSet<>());
        for (Integer cur : childVers) {
            // 组内项目循环依赖
            if (prdVisited[cur] == 0 && !dfsChild(cur)) return false;
        }
        // 遍历结束，设置标识
        grpVisited[start] = -1;
        return true;
    }

    private boolean dfsChild(int start) {
        // 组内存在环则是1，已遍历则是-1
        if (prdVisited[start] != 0) return prdVisited[start] == -1;

        // 标记为正在访问
        prdVisited[start] = 1; 
        Node temp = prdGraph[start];
        while (temp != null) {
            // 遍历与start同一分组的后继结点，如果后续路径不满足条件存在环，返回
            if (prdToGrp[start] == prdToGrp[temp.ver] && !dfsChild(temp.ver)) {
                return false;
            } 
            temp = temp.next;
        }
        // 子路径已遍历完
        res[idx++] = start;
        prdVisited[start] = -1; 
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

## 性能分析

&emsp;由于一个项目只能归属一个组，本质上是稀疏图，时间复杂度为$O(M + N)$，空间复杂度为$O(M + N)$。

执行用时：38ms，在所有java提交中击败了98.00%的用户。

内存消耗：54.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有通过广度优先搜索的方式实现两层遍历，使用广度优先搜索的难度在于每组内子图入度的统计。