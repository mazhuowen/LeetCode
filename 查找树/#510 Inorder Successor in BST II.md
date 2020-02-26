[toc]

Given a `node` in a binary search tree, find the in-order successor of that node in the BST.

If that node has no in-order successor, return `null`.

The successor of a node is the node with the smallest key greater than `node.val`.

**Follow up:**

Could you solve it without looking up any of the node's values?



Constraints:

* $-10^5 \le \text{Node.val} \le 10^5$
* $1 \le \text{Number of Nodes} \le 10^4$
* All Nodes will have unique values.



## 题目解读

&emsp;给定一个结点，寻找其后继结点。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
};
*/
class Solution {
    public Node inorderSuccessor(Node node) {
        
    }
}
```

## 程序设计

* 存在右子节点，则后继结点在右子节点上，不存在则在祖先路径上查找第一个是其父节点左子节点的祖先结点，不存在则没有后继结点。

```java
class Solution {
    public Node inorderSuccessor(Node node) {
        if(node == null) {
            return null;
        }
        // 存在右子节点
        if(node.right != null) {
            node = node.right;
            while(node.left != null) {
                node = node.left;
            }
            return node;
        }
        // 祖先结点办理
        else {
            while(node.parent != null && node.parent.right == node) {
                node = node.parent;
            }
            // 不存在
            if(node.parent == null) {
                return null;
            } else {
                return node.parent;
            }
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，最坏$O(N)$；空间复杂度为$O(1)$。

执行用时：42ms，在所有java提交中击败了58.27%的用户。

内存消耗：42.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。