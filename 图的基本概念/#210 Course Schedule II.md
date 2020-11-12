[toc]

There are a total of $n$ courses you have to take, labeled from $0$ to $n-1$.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite **pairs**, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.



**Note**:

* The input prerequisites is a graph represented by **a list of edges**, not adjacency matrices. Read more about how a graph is represented.
* You may assume that there are no duplicate edges in the input prerequisites.



## 题目解读

&emsp;有向无环图的拓扑排序。

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {

    }
}
```

## 程序设计

* 考虑到题目中的拓扑排序序列是倒序的，首先使用深度优先搜索，思路如下：

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // 无效，返回空
        if (numCourses < 0) return new int[]{};
        int[] res = new int[numCourses];
        // 有课程，但是没有关联关系，返回顺序列表
        if (prerequisites == null || prerequisites.length == 0) {
            for (int i = 0; i < numCourses; i++) res[i] = i;
            return res;
        }

        // 构建图
        Node[] graph = new Node[numCourses];
        for (int[] plan : prerequisites) {
            graph[plan[0]] = new Node(plan[1], graph[plan[0]]);
        }
        // 访问标识
        int[] visited = new int[numCourses];
        // 记录深度遍历结点
        List<Integer> list = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (visited[i] != -1) {
                // 有环，返回空
                if (!isDAG(i, graph, visited, list)) return new int[]{};
                // 标记为已访问且无环
                visited[i] = -1;
            } 
        }
        // 拼接
        for (int i = 0; i < numCourses; i++) {
            res[i] = list.get(i);
        }
        return res;
    }

    private boolean isDAG(int start, Node[] graph, int[] visited, List<Integer> record) {
        // 标记为正在访问
        visited[start] = 1;
        Node temp = graph[start];
        // 遍历后继顶点
        while (temp != null) {
            // 已访问无环，则直接遍历下一个
            if (visited[temp.val] == -1) {
                temp = temp.next;
                continue;
            }
            // 有环
            if (visited[temp.val] == 1 || !isDAG(temp.val, graph, visited, record)) {
                return false;
            }
            // 遍历完temp的后继结点，无环，标记为已访问无环
            visited[temp.val] = -1;
            temp = temp.next;
        }
        // 当前结点遍历完，加入
        record.add(start);
        return true;
    }
}

class Node {
    int val;
    Node next;

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

* 使用广度优先搜索思路如下：

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // 无效，返回空
        if (numCourses < 0) return new int[]{};
        int[] res = new int[numCourses];
        // 有课程，但是没有关联关系，返回顺序列表
        if (prerequisites == null || prerequisites.length == 0) {
            for (int i = 0; i < numCourses; i++) {
                res[i] = i;
            }
            return res;
        }

        // 构建图
        Node[] graph = new Node[numCourses];
        // 统计入度
        int[] degree = new int[numCourses];
        for (int[] plan : prerequisites) {
            graph[plan[0]] = new Node(plan[1], graph[plan[0]]);
            degree[plan[1]]++;
        }
        // 初始化队列
        Queue<Integer> queue = new LinkedList<>();
        for(int i = 0; i < numCourses; i++) {
            if (degree[i] == 0) queue.add(i);
        }
        
        // 记录索引
        int idx = numCourses - 1;
        // 拓扑排序
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            res[idx--] = cur;
            // 遍历后继入度为0的结点
            Node temp = graph[cur];
            while (temp != null) {
                if (--degree[temp.val] == 0) queue.add(temp.val); 
                temp = temp.next;
            }
        }
  		// 若队列中元素不足结点数个，说明有环
        return idx == -1 ? res : new int[]{};
    }
}

class Node {
    int val;
    Node next;

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

## 性能分析

&emsp;深度优先搜索时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：5ms，在所有java提交中击败了95.19%的用户。

内存消耗：42.7MB，在所有java提交中击败了80.00%的用户。

&emsp;广度优先搜索时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：3ms，在所有java提交中击败了99.89%的用户。

内存消耗：42.5MB，在所有java提交中击败了80.45%的用户。

## 官方解题

&emsp;同上。