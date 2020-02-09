[toc]

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Note: A leaf is a node with no children.



## 题目解读

&emsp;给定二叉树，求解树的深度。

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
    public int maxDepth(TreeNode root) {
        
    }
}
```

## 程序设计

* 最简单的递归实现，当前结点的层数是子节点高度最大值加一。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        return Math.max(left, right) + 1;
    }
}
```

* 非递归实现则深度搜索，记录层数，选择最大的那个。

```java

```



## 性能分析

&emsp;递归实现，时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了5.07%的用户。

## 官方解题

&emsp;官方还提供了非递归实现，相当于在前序遍历的过程中比较深度。需要引入额外的对象`Pair`。

```java
import javafx.util.Pair;
import java.lang.Math;

class Solution {
  public int maxDepth(TreeNode root) {
    Queue<Pair<TreeNode, Integer>> stack = new LinkedList<>();
    if (root != null) {
      stack.add(new Pair(root, 1));
    }

    int depth = 0;
    while (!stack.isEmpty()) {
      Pair<TreeNode, Integer> current = stack.poll();
      root = current.getKey();
      int current_depth = current.getValue();
      if (root != null) {
        depth = Math.max(depth, current_depth);
        stack.add(new Pair(root.left, current_depth + 1));
        stack.add(new Pair(root.right, current_depth + 1));
      }
    }
    return depth;
  }
}
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$。