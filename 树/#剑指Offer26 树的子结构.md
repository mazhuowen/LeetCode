[toc]

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:

```
	 3
	/ \
   4   5
  / \
 1   2
```

给定的树 B：

```
   4 
  /
 1
```

返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。



**示例 1**：

```
输入：A = [1,2,3], B = [3,1]
输出：false
```



**示例 2**：

```
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```



**限制**：

* $0 \le \text{节点个数} \le 10000$



## 题目解读

&emsp;判断`B`是否是`A`的子树结构。

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
    public boolean isSubStructure(TreeNode A, TreeNode B) {

    }
}
```

## 程序设计

* 递归。

```java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if (A == null || B == null) return false;
        return check(A, B) || isSubStructure(A.left, B) || isSubStructure(A.right, B); 
    }

    private boolean check(TreeNode A, TreeNode B) {
        if (B == null) return true;
        if (A == null) return false;
        return A.val == B.val && check(A.left, B.left) && check(A.right, B.right);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN)$，空间复杂度为$O(M)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：39.5 MB, 在所有 Java 提交中击败了99.86%的用户。

## 官方解题

&emsp;暂无，密切关注。
