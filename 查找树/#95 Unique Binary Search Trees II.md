[toc]

Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1 ... n.



## 题目解读

&emsp;给定$n$个数，生成所有形式的二叉查找树。

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
    public List<TreeNode> generateTrees(int n) {

    }
}
```

## 程序设计

* 参考官方解题，采用递归生成所有满足要求的组合。

```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        if(n <= 0) {
            return new LinkedList<>();
        }
        return generateTrees(1, n);
    }

    private List<TreeNode> generateTrees(int start, int end) {
        List<TreeNode> res = new LinkedList<>();
        // 递归终止条件
        if(start > end) {
            res.add(null);
            return res;
        }
        for(int i = start; i <= end; i++) {
            List<TreeNode> lefts = generateTrees(start, i - 1);
            List<TreeNode> rights = generateTrees(i + 1, end);
            // 将所有左右组合与头结点组合，加入链表
            for(TreeNode left : lefts) {
                for(TreeNode right : rights) {
                    // 头结点
                    TreeNode head = new TreeNode(i);
                    head.left = left;
                    head.right = right;
                    res.add(head);
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;卡特兰数$G_n$满足递推公式：
$$
h(n) = h(0) * h(n - 1) + h(1) * h(n - 2) + \dots + h(n - 1) * h(0)
$$
可看到和树的组合生成是吻合的。递归$G_n$次生成$G_n$棵树，每生成一棵树需要遍历$n$个点，总的时间复杂度为$O(nG_n)$，由于$G_n \propto \frac{4^n}{n^{\frac{3}{2}}}$，总的时间复杂度为$O(\frac{4^n}{n^{\frac{1}{2}}})$；空间复杂度为$\frac{4^n}{n^{\frac{3}{2}}}$棵树，每棵有$n$个结点，总的空间复杂度为$O(\frac{4^n}{n^{\frac{1}{2}}})$。

执行用时：2ms，在所有java提交中击败了92.45%的用户。

内存消耗：41.2MB，在所有java提交中击败了5.07%的用户。

## 官方解题

&emsp;参考官方解题。