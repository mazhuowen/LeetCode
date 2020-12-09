[toc]

You need to construct a string consists of parenthesis and integers from a binary tree with the preorder traversing way.

The null node needs to be represented by empty parenthesis pair "()". And you need to omit all the empty parenthesis pairs that don't affect the one-to-one mapping relationship between the string and the original binary tree.



**Example 1**:

```
Input: Binary tree: [1,2,3,4]
       1
     /   \
    2     3
   /    
  4     

Output: "1(2(4))(3)"

Explanation: Originallay it needs to be "1(2(4)())(3()())", 
but you need to omit all the unnecessary empty parenthesis pairs. 
And it will be "1(2(4))(3)".
```

**Example 2**:

```
Input: Binary tree: [1,2,3,null,4]
       1
     /   \
    2     3
     \  
      4 

Output: "1(2()(4))(3)"

Explanation: Almost the same as the first example, 
except we can't omit the first parenthesis pair to break the one-to-one mapping relationship between the input and the output.
```



## 题目解读

&emsp;根据二叉树构建字符串，注意左子节点为空而右子节点不为空，左子节点需要打印。

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
    public String tree2str(TreeNode t) {

    }
}
```

## 程序设计

* 前序遍历，注意左子节点为空的特殊情况。

```java
class Solution {
    public String tree2str(TreeNode t) {
        StringBuffer sb = new StringBuffer();
        tree2str(t, sb);
        int len = sb.length();
        // 删除总括号
        return len >= 3 ? sb.toString().substring(1, len - 1) : sb.toString();
    }

    private void tree2str(TreeNode root, StringBuffer sb) {
        if (root == null) return;
        sb.append("(").append(root.val);
        // 判断特殊情况，打印左子节点
        if (root.left == null && root.right != null) sb.append("()");
        tree2str(root.left, sb);
        tree2str(root.right, sb);
        sb.append(")");
    }
}
```

* 不处理最外层括号的思路。

```java
class Solution {
    public String tree2str(TreeNode t) {
        StringBuffer sb = new StringBuffer();
        tree2str(t, sb);
        return sb.toString();
    }

    private void tree2str(TreeNode root, StringBuffer sb) {
        if (root == null) return;
        sb.append(root.val);
        if (root.left != null || root.right != null) {
            sb.append("(");
            tree2str(root.left, sb);
            sb.append(")");
        }
        if (root.right != null) {
            sb.append("(");
            tree2str(root.right, sb);
            sb.append(")");
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2 ms, 在所有 Java 提交中击败了94.84%的用户

内存消耗：38.2 MB, 在所有 Java 提交中击败了95.74%的用户。

## 官方解题

&emsp;官方思路类似。