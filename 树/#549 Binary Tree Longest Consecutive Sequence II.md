[toc]



## 题目解读



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
    public int longestConsecutive(TreeNode root) {

    }
}
```

## 程序设计



```java
class Solution {
    private int maxLen;
    
    public int longestConsecutive(TreeNode root) {
        consecutive(root);
        return maxLen;
    }

    // 返回数组第一个值为递增，第二个为递减序列的长度
    private int[] consecutive(TreeNode root) {
        if(root == null) {
            return new int[]{0, 0};
        }
        int[] left = consecutive(root.left);
        int[] right = consecutive(root.right);
        // 子父子组合
        if(root.left != null && root.right != null) {
            // 组成递增
            if(root.val == root.left.val + 1 && root.val == root.right.val - 1) {
                maxLen = Math.max(maxLen, left[1] + 1 + right[0]);
            } 
            // 组成递减
            else if(root.val == root.left.val - 1 && root.val == root.right.val + 1) {
                maxLen = Math.max(maxLen, left[0] + 1 + right[1]);
            }
        }
        int asc = 0, desc = 0;
        if(root.left != null) {
            // 递减
            if(root.val == root.left.val + 1) {
                desc = Math.max(desc, left[1]);
            }
            // 递增
            if(root.val == root.left.val - 1) {
                asc = Math.max(asc, left[0]);
            }
        }
        if(root.right != null) {
            // 递减
            if(root.val == root.right.val + 1) {
                desc = Math.max(desc, right[1]);
            }
            // 递增
            if(root.val == root.right.val - 1) {
                asc = Math.max(asc, right[0]);
            }
        }
        // 更新
        maxLen = Math.max(maxLen, Math.max(++asc, ++desc));
        return new int[]{asc, desc};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.1MB，在所有java提交中击败了9.09%的用户。

## 官方解题

&emsp;思路一致，但形式更简单。上面代码中判断子父子组合、单个链组合花费了大量代码。时间上子父子符合就是当前结点的递增序列长度和递减序列长度之和加一，有了这个思路就可以简化代码。

```java
class Solution {
    private int maxLen;
    public int longestConsecutive(TreeNode root) {
        consecutive(root);
        return maxLen;
    }

    // 返回数组第一个值为递增，第二个为递减序列的长度
    private int[] consecutive(TreeNode root) {
        if(root == null) {
            return new int[]{0, 0};
        }
        // 递增，递减
        int asc = 1, desc = 1;
        if(root.left != null) {
            int[] left = consecutive(root.left);
            if(root.val == root.left.val - 1)
                asc += left[0];
            if(root.val == root.left.val + 1)
                desc += left[1];
        }
        if(root.right != null) {
            int[] right = consecutive(root.right);
            if(root.val == root.right.val - 1)
                asc = Math.max(asc, right[0] + 1);
            if(root.val == root.right.val + 1)
                desc = Math.max(desc, right[1] + 1);
        }
        // 此处包含了子父子组合、单链组合的情况，更新
        maxLen = Math.max(maxLen, asc + desc - 1);
        return new int[]{asc, desc};
    }
}
```