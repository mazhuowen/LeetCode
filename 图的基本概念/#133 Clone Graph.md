[toc]

Given a reference of a node in a **connected** undirected graph.

Return a **deep copy** (clone) of the graph.

Each node in the graph contains a val (`int`) and a list (`List[Node]`) of its neighbors.

```java
class Node {
    public int val;
    public List<Node> neighbors;
}
```


Test case format:

For simplicity sake, each node's value is the same as the node's index (1-indexed). For example, the first node with `val = 1`, the second node with `val = 2`, and so on. The graph is represented in the test case using an adjacency list.

**Adjacency list** is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with `val = 1`. You must return the **copy of the given node** as a reference to the cloned graph.

Constraints:

* $1 \le \text{Node.val} \le 100$
* `Node.val` is unique for each node.
* Number of Nodes will not exceed 100.
* There is no repeated edges and no self-loops in the graph.
* The Graph is connected and all nodes can be visited starting from the given node.



## 题目解读

&emsp;给定无向连通图中一个节点的引用，返回该图的深拷贝。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/
class Solution {
    public Node cloneGraph(Node node) {
        
    }
}
```

## 程序设计

* 最直接的想法是深度优先搜索遍历，每遍历一个结点便复制并记录。

```java
class Solution {
    public Node cloneGraph(Node node) {
        if(node == null) return null;
        // 使用字典记录是否访问过，及保存映射关系
        Map<Node, Node> visited = new HashMap<>();
        return cloneGraph(node, visited);
    }

    private Node cloneGraph(Node node, Map<Node, Node> visited) {
        // 已访问，直接返回
        if(visited.get(node) != null) {
            return visited.get(node);
        }
        Node copy = new Node(node.val);
        // 标记为已访问
        visited.put(node, copy);
        // 深度优先搜索创建邻接结点
        for (Node neighbor : node.neighbors) {
            copy.neighbors.add(cloneGraph(neighbor, visited));
        }
        return copy;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：36ms，在所有java提交中击败了52.42%的用户。

内存消耗：38.7MB，在所有java提交中击败了5.17%的用户。

## 官方解题

&emsp;官方解题还提供了广度优先搜索的思路。考虑到调用栈的深度，使用BFS进行图的遍历比DFS更好。

```java
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) return null;
        Map<Node, Node> record = new HashMap<>();
        Queue<Node> queue = new LinkedList<>();
        queue.add(node);
        record.put(node, new Node(node.val));
        // 广度优先遍历
        while(!queue.isEmpty()) {
            Node cur = queue.poll();
            Node copy = record.get(cur);
            for(Node visit : cur.neighbors) {
                // 已访问
                if (record.get(visit) != null) {
                    copy.neighbors.add(record.get(visit));
                } 
                // 未访问，入队
                else {
                    Node temp = new Node(visit.val);
                    copy.neighbors.add(temp);
                    record.put(visit, temp);
                    queue.add(visit);
                }
            } 
        }
        return record.get(node);
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：36ms，在所有java提交中击败了52.42%的用户。

内存消耗：38.6MB，在所有java提交中击败了5.17%的用户。