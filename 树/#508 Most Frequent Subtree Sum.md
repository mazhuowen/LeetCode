[toc]

Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.

Note: You may assume the sum of values in any subtree is in the range of 32-bit signed integer.



## 题目解读

&emsp;给定二叉树，返回出现频率最多的子树和。注意，题目说明结果可以以任意的顺序给出。

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
    public int[] findFrequentTreeSum(TreeNode root) {
        
    }
}
```

## 程序设计

* 使用前序遍历统计子树和，用字典保存子树和及其频率。最后从字典中取出最大的频率对应的和，并输出。

```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();

    public int[] findFrequentTreeSum(TreeNode root) {
        subTreeSum(root);
        List<Integer> l = new LinkedList<>();
        int max = Integer.MIN_VALUE;
        for(Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if(entry.getValue() == max) {
                l.add(entry.getKey());
            }
            else if(entry.getValue() > max) {
                max = entry.getValue();
                l.clear();
                l.add(entry.getKey());
            }
        }
        int[] res = new int[l.size()];
        for(int i = 0; i < l.size(); i++) {
            res[i] = l.get(i);
        }
        return res;
    }

    private int subTreeSum(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int sum = root.val;
        sum += subTreeSum(root.left);
        sum += subTreeSum(root.right);
        // 计数
        map.put(sum, map.getOrDefault(sum, 0) + 1);
        return sum;
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$；遍历字典、组装结果均为$O(N)$。最终时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时 :8 ms, 在所有 Java 提交中击败了58.41%的用户

内存消耗 :48.7 MB, 在所有 Java 提交中击败了5.42%的用户

## 官方解题

&emsp;暂无，密切关注。