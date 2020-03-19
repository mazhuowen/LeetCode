[toc]

There are `N` rooms and you start in room `0`. Each room has a distinct number in `0, 1, 2, ..., N-1`, and each room may have some keys to access the next room. 

Formally, each room `i` has a list of keys `rooms[i]`, and each key `rooms[i][j]` is an integer in `[0, 1, ..., N-1]` where `N = rooms.length`. A key `rooms[i][j] = v` opens the room with number `v`.

Initially, all the rooms start locked (except for room `0`). 

You can walk back and forth between rooms freely.

Return `true` if and only if you can enter every room.

**Note:**

* $1 \le \text{rooms.length} \le 1000$
* $0 \le \text{rooms[i].length} \le 1000$
* The number of keys in all rooms combined is at most `3000`.



## 题目解读

&emsp;有`N`个房间，开始时位于`0`号房间。每个房间有不同的号码`0，1，2，...，N-1`，并且房间里可能有一些钥匙能进入下一个房间。初始除`0`号房间外的其余所有房间都被锁住。判断能否进入每一个房间。

```java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {

    }
}
```

## 程序设计

* 首先需要理解题意，每个房间包含钥匙，对应一条边，走完每个房间就是遍历完所有的边，实际上是个给定起始点的遍历问题。

> 不能理解为拓扑排序。

```java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        if (rooms == null || rooms.size() == 0) return true;

        int n = rooms.size();
        boolean[] visited = new boolean[n];
        dfs(0, rooms, visited);
        // 检查是否全部遍历到
        for (boolean flag : visited) {
            if (!flag) return false;
        }
        return true;
    }

    private void dfs(int start, List<List<Integer>> graph, boolean[] visited) {
        // 已遍历，返回
        if (visited[start]) return;
		// 标记为已遍历
        visited[start] = true;
        for (int cur : graph.get(start)) {
            dfs(cur, graph, visited);
        }
    }
}
```

## 性能分析

&emsp;时间性能为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时 ：1ms，在所有java提交中击败了99.20%的用户。

内存消耗：40.7MB，在所有java提交中击败了89.12%的用户。

## 官方解题

&emsp;官方提供了深度优先搜索的栈的实现方式。