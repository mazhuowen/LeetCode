[toc]

Given a binary tree `root` and a linked list with `head` as the first node. 

Return True if all the elements in the linked list starting from the `head` correspond to some downward path connected in the binary tree otherwise return False.

In this context downward path means a path that starts at some node and goes downwards.



**Constraints**:

* $1 \le \text{node.val} \le 100$ for each node in the linked list and binary tree.
* The given linked list will contain between $1$ and $100$ nodes.
* The given binary tree will contain between $1$ and $2500$ nodes.



## 题目解读

&emsp;判断路径上是否存在指定的序列。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSubPath(ListNode head, TreeNode root) {
        
    }
}
```

## 程序设计

* 思路为以当前子树开始匹配序列，如果未匹配中，则递归其两个子树继续匹配。

```java
class Solution {
    public boolean isSubPath(ListNode head, TreeNode root) {
        if (root == null) return false;
        return dfs(head, root) || isSubPath(head, root.left) || isSubPath(head, root.right);
    }

    private boolean dfs(ListNode head, TreeNode root) {
        if (head == null) return true;
        if (root == null) return false;
        // 匹配失败
        if (head.val != root.val) return false;
        // 继续匹配
        return dfs(head.next, root.left) || dfs(head.next, root.right);
    }
}
```

## 性能分析

&emsp;最坏情况每个节点开始匹配，匹配到链表尾失败，假设有$N$个树节点，$M$个链表节点，则每个树节点子树都匹配到尾部，即匹配深度为$M$，共$2^M - 1$个节点，此处$2^M - 1$不能多于$N$，从而$N$个节点总的比对次数为$O(N\min(N, 2^M - 1))$；空间复杂度由树的深度决定。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.1MB，在所有java提交中击败了57.14%的用户。

## 官方解题

&emsp;上述思路参考官方。