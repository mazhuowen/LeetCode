[toc]

Return any binary tree that matches the given preorder and postorder traversals.

Values in the traversals `pre` and `post` are distinct positive integers.

Note:

* $1 \le \text{pre.length} == \text{post.length} \le 30$
* `pre[]` and `post[]` are both permutations of `1, 2, ..., pre.length`.
* It is guaranteed an answer exists. If there exists multiple answers, you can return any of them.



## 题目解读

&emsp;给定前序序列和后序序列，给出一棵二叉树。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode constructFromPrePost(int[] pre, int[] post) {

    }
}
```

## 程序设计

* 前序和后序序列决定不了一棵树，对于只有一个子树的结点，无法决定是右子树还是左子树，由于题目只要给出符合要求的树即可，可以任意设定。此处设定为左子树。

```java
class Solution {
    public TreeNode constructFromPrePost(int[] pre, int[] post) {
        if(pre == null || pre.length == 0) {
            return null;
        }
        return constructFromPrePost(pre, 0, pre.length - 1, post, 0 , post.length - 1);
    }

    private TreeNode constructFromPrePost(int[] pre, int s1, int e1, int[] post, int s2, int e2) {
        // 根节点值不一样，抛异常
        if(pre[s1] != post[e2]) throw new IllegalArgumentException("invalid param");
        TreeNode root = new TreeNode(pre[s1]);

        // 叶节点，返回
        if(s1 == e1) {
            return root;
        }
        int l = --e2, r = ++s1;
        // 只有一个子节点，此时不能唯一确定，题目要求返回任意一个，此处设定为左子树
        if(pre[s1] == post[e2]) {
            root.left = constructFromPrePost(pre, s1, e1, post, s2, e2);
            return root;
        }
        // 在后序序列中查找左子树根节点
        for(; l >= s2; l--) {
            if(post[l] == pre[s1]) break;
        }
        // 在前序序列中查找右子树根节点
        for(; r <= e1; r++) {
            if(pre[r] == post[e2]) break;
        }

        root.left = constructFromPrePost(pre, s1, r - 1, post, s2, l);
        root.right = constructFromPrePost(pre, r, e1, post, l + 1, e2);
        return root;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39MB，在所有java提交中击败了36.19%的用户。

## 官方解题

&emsp;官方使用递归解决，但实现不同，官方以前序序列为主，每次遍历后缀序列寻找右结点，这样每次都要遍历整个数组，时间复杂度为$O(N^2)$。