[toc]

Given an n-ary tree, return the level order traversal of its nodes' values.

Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).



Constraints:

* The height of the n-ary tree is less than or equal to 1000
* The total number of nodes is between $[0, 10^4]$



## 题目解读

&emsp;给定N叉树，输出层次遍历。

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
    public List<List<Integer>> levelOrder(Node root) {
        
    }
}
```

## 程序设计

* 思路同二叉树。

```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            List<Integer> l = new LinkedList<>();
            int len = queue.size();
            for(int i = 0; i < len; i++) {
                Node cur = queue.poll();
                // 子节点入队
                if(cur.children != null && cur.children.size() >= 1) {
                    for(Node traverse : cur.children) {
                        queue.add(traverse);
                    }
                }
                // 当前结点加入链表
                l.add(cur.val);
            }
            res.add(l);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了56.62%的用户。

内存消耗：50.5MB，在所有java提交中击败了5.08%的用户。

## 官方解题

&emsp;官方除了非递归思路，还有递归思路，同二叉树。