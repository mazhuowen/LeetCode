[toc]

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses-1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite **pairs**, is it possible for you to finish all courses?



**Constraints**:

* The input prerequisites is a graph represented by **a list of edges**, not adjacency matrices. Read more about how a graph is represented.
* You may assume that there are no duplicate edges in the input prerequisites.
* $1 \le \text{numCourses} \le 10^5$



## 题目解读

&emsp;判断有向图是否存在环路。

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {

    }
}
```

## 程序设计

* 首先想到的是常规的判断有向图环路的逻辑，即从每一个定点出发深度优先遍历。但是由于时间复杂度为$O(V^2)$，会超时。

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (prerequisites == null || prerequisites.length == 0) return true;

        // 构建图
        int[][] graph = new int[numCourses][numCourses];
        for (int[] plan : prerequisites) {
            graph[plan[0]][plan[1]] = 1;
        }
        // 判断是否存在环
        boolean[] visited = new boolean[numCourses];
        for (int i = 0; i < numCourses; i++) {
            if(!isDAG(i, graph, visited)) {
                return false;
            }
            // 遍历完成，以i出发不存在环路，重新置为未访问
            visited[i] = false;
        }
        return true;
    }

    private boolean isDAG(int start, int[][] graph, boolean[] visited) {
        visited[start] = true;
        for (int i = 0; i < graph.length; i++) {
            if (graph[start][i] == 1) {
                // 路径上存在已访问的结点或后续路径存在环
                if (visited[i] || !isDAG(i, graph, visited)) return false;
                // 遍历完成，以i出发不存在环路，重新置为未访问
                visited[i] = false;
            }
        }
        return true;
    }
}
```

* 考虑到结点规模太大，导致以邻接矩阵为代表的算法时间超出限制，以邻接表结构代替：

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (prerequisites == null || prerequisites.length == 0) return true;

        // 构建图
        Node[] graph = new Node[numCourses];
        for (int[] plan : prerequisites) {
            // 头插法
            graph[plan[0]] = new Node(plan[1], graph[plan[0]]);
        }
        // 判断是否存在环
        boolean[] visited = new boolean[numCourses];
        for (int i = 0; i < numCourses; i++) {
            if(!isDAG(i, graph, visited)) {
                return false;
            }
            // 遍历完成，以i出发不存在环路，重新置为未访问
            visited[i] = false;
        }
        return true;
    }

    private boolean isDAG(int start, Node[] graph, boolean[] visited) {
        visited[start] = true;
        Node temp = graph[start];
        while (temp != null) {
            // 存在环路
            if(visited[temp.val] || !isDAG(temp.val, graph, visited)) return false;
            // 遍历结束，置为未访问
            visited[temp.val] = false;
            temp = temp.next;
        }
        return true;
    }
}

class Node {
    int val;
    Node next;

    Node(int val) {
        this.val = val;
    }

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

* 继续优化上述算法，上述算法每次从一个顶点从头遍历，考虑到之前已经遍历过的一段路径不存在环，则下次遍历遇到这段路径仍然不可能存在环，这样我们需要记录已经遍历过的无环路径、本次遍历路径、未遍历路径，改造得：

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (prerequisites == null || prerequisites.length == 0) return true;

        // 构建图
        Node[] graph = new Node[numCourses];
        for (int[] plan : prerequisites) {
            // 头插法
            graph[plan[0]] = new Node(plan[1], graph[plan[0]]);
        }
        // 判断是否存在环
        int[] visited = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            // 未遍历则遍历，如果存在环，则不符合要求
            if(visited[i] != -1 && !isDAG(i, graph, visited)) {
                return false;
            }
            // 遍历完成，以i出发不存在环路，置为访问且无环
            visited[i] = -1;
        }
        return true;
    }

    private boolean isDAG(int start, Node[] graph, int[] visited) {
        // 置为正在遍历
        visited[start] = 1;
        Node temp = graph[start];
        while (temp != null) {
            // 之前已经遍历过，无环
            if (visited[temp.val] == -1) {
                temp = temp.next;
                continue;
            }
            // 存在环路
            if(visited[temp.val] == 1 || !isDAG(temp.val, graph, visited)) return false;
            // 遍历结束，置为已访问且无环
            visited[temp.val] = -1;
            temp = temp.next;
        }
        return true;
    }
}

class Node {
    int val;
    Node next;

    Node(int val) {
        this.val = val;
    }

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

## 性能分析

&emsp;原始的方法时间复杂度为$O(VE)$，空间复杂度为$O(V + E)$。

执行用时：28ms，在所有java提交中击败了52.47%的用户。

内存消耗：41.2MB，在所有java提交中击败了89.99%的用户。

&emsp;改进后的方法时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：41MB，在所有java提交中击败了90.26%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有思路利用拓扑排序入度统计，如果存在环，则有结点入度永远不为$0$，利用这个性质拓扑排序，如果所有结点都已经排序完，则说明无环，否则有环。

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (prerequisites == null || prerequisites.length == 0) return true;

        // 构建图
        Node[] graph = new Node[numCourses];
        // 记录入度
        int[] degree = new int[numCourses];
        for (int[] plan : prerequisites) {
            // 头插法
            graph[plan[0]] = new Node(plan[1], graph[plan[0]]);
            degree[plan[1]]++;
        }
        // 初始化队列
        Queue<Integer> queue = new LinkedList<>();
        for(int i = 0; i < numCourses; i++) {
            if (degree[i] == 0) queue.add(i);
        }
        while (!queue.isEmpty()) {
            int idx  = queue.poll();
            // 结点数减一
            numCourses--;
            // 判断边的入度
            Node temp = graph[idx];
            while (temp != null) {
                // 为0则加入队列
                if (--degree[temp.val] == 0) queue.add(temp.val);
                temp = temp.next;
            }
        }
        return numCourses == 0;
    }
}

class Node {
    int val;
    Node next;

    Node(int val) {
        this.val = val;
    }

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：3ms，在所有java提交中击败了99.27%的用户。

内存消耗：40.6MB，在所有java提交中击败了90.26%的用户。