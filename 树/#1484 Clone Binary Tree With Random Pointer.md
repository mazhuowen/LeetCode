[toc]

A binary tree is given such that each node contains an additional random pointer which could point to any node in the tree or null.

Return a deep copy of the tree.

The tree is represented in the same input/output way as normal binary trees where each node is represented as a pair of `[val, random_index]` where:

* `val`: an integer representing `Node.val`
* `random_index`: the index of the node (in the input) where the random pointer points to, or `null` if it does not point to any node.

You will be given the tree in class `Node` and you should return the cloned tree in class `NodeCopy`.

 

**Example 1**:

<img src="..\images\#1484_exp1.png" style="zoom: 50%;" />

```
Input: root = [[1,null],null,[4,3],[7,0]]
Output: [[1,null],null,[4,3],[7,0]]
Explanation: The original binary tree is [1,null,4,7].
The random pointer of node one is null, so it is represented as [1, null].
The random pointer of node 4 is node 7, so it is represented as [4, 3] where 3 is the index of node 7 in the tree array.
The random pointer of node 7 is node 1, so it is represented as [7, 0] where 0 is the index of node 1 in the tree array
```

**Example 2**:

<img src="..\images\#1484_exp2.png" style="zoom: 50%;" />

```
Input: root = [[1,4],null,[1,0],null,[1,5],[1,5]]
Output: [[1,4],null,[1,0],null,[1,5],[1,5]]
Explanation: The random pointer of a node can be the node itself.
```

**Example 3**:

<img src="..\images\#1484_exp3.png" style="zoom: 50%;" />

```
Input: root = [[1,6],[2,5],[3,4],[4,3],[5,2],[6,1],[7,0]]
Output: [[1,6],[2,5],[3,4],[4,3],[5,2],[6,1],[7,0]]
```

**Example 4**:

```
Input: root = []
Output: []
```

**Example 5**:

```
Input: root = [[1,null],null,[2,null],null,[1,null]]
Output: [[1,null],null,[2,null],null,[1,null]]
```



**Constraints**:

* The number of nodes in the tree is in the range $[0, 1000]$.
* Each node's value is between $[1, 10^6]$.



## 题目解读

&emsp;返回深度拷贝树，树存在随机节点。

```java
/**
 * Definition for a binary tree node.
 * public class Node {
 *     int val;
 *     Node left;
 *     Node right;
 *     Node random;
 *     Node() {}
 *     Node(int val) { this.val = val; }
 *     Node(int val, Node left, Node right, Node random) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *         this.random = random;
 *     }
 * }
 */
class Solution {
    public NodeCopy copyRandomBinaryTree(Node root) {
        
    }
}
```

## 程序设计

* 采用字典记录节点与复制节点的关系，递归复制并维护关系。

```java
class Solution {
    public NodeCopy copyRandomBinaryTree(Node root) {
        // 记录节点对应关系
        Map<Node, NodeCopy> relation = new HashMap<>();
        return preOrder(root, relation);
    }

    private NodeCopy preOrder(Node root, Map<Node, NodeCopy> relation) {
        if (root == null) return null;
        // 未创建，则创建
        if (!relation.containsKey(root)) {
            // 必须先构建函数设置到字典，再维护子节点关系，避免子节点random节点指向当前节点造成死循环
            relation.put(root, new NodeCopy(root.val));
            relation.get(root).left = preOrder(root.left, relation);
            relation.get(root).right = preOrder(root.right, relation);
            relation.get(root).random = preOrder(root.random, relation);
        }
        return relation.get(root);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9 ms, 在所有 Java 提交中击败了38.68%的用户

内存消耗：39.8 MB, 在所有 Java 提交中击败了56.73%的用户。

## 官方解题

&emsp;暂无，密切关注。