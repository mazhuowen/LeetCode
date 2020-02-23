Given an n-ary tree, return the preorder traversal of its nodes' values.

Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).

Constraints:

* The height of the n-ary tree is less than or equal to 1000
* The total number of nodes is between $[0, 10^4]$





## 题目解读

&emsp;N叉树的前序遍历。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
    public List<Integer> preorder(Node root) {
        
    }
}
```

## 程序设计

* 递归形式。

```java
class Solution {
    public List<Integer> preorder(Node root) {
        List<Integer> res = new LinkedList<>();
        preorder(root, res);
        return res;
    }

    private void preorder(Node root, List<Integer> res) {
        if(root == null) {
            return;
        }
        res.add(root.val);
        List<Node> children = root.children;
        // 递归
        for(Node child : children) {
            preorder(child, res);
        }
    }
}
```

* 迭代形式和二叉树前序遍历一致，需注意子节点链表需要从右至左入栈。

## 性能分析

&emsp;时间复杂度为$O(M)$，空间复杂度为$O(\log_NM)$，最坏$O(M)$。

执行用时：1ms，在所有java提交中击败了99.80%的用户。

内存消耗：41.6MB，在所有java提交中击败了5.19%的用户。

## 官方解题

&emsp;同上。