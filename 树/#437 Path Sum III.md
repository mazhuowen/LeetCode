[toc]

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.



## 题目解读

&emsp;给定二叉树和数值，得到二叉树链路中所有和该数值相等的链路个数。

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
    public int pathSum(TreeNode root, int sum) {
        
    }
}
```

## 程序设计

* 可以自顶向下遍历，保存遍历路径，每遍历一个点，向上迭代计算路径和。

```java
class Solution {
    private List<Integer> path = new LinkedList<>();

    public int pathSum(TreeNode root, int sum) {
        if(root == null) {
            return 0;
        }
        path.add(root.val);
        int count = 0;
        int pathSum = 0;
        // 当前结点向前计算
        for(int i = path.size() - 1; i >= 0; i--) {
            pathSum += path.get(i);
            if(pathSum == sum) {
                count++;
            }
        }
        // 向后递归
        count += pathSum(root.left, sum);
        count += pathSum(root.right, sum);
        // 回溯
        path.remove(path.size() - 1);
        return count;
    }
}
```

## 性能分析

&emsp;递归到达需要遍历$N$次，每次需要向上遍历路径计算平均$\log_2N$，最坏$O(N)$，故时间复杂度平均$O(N\log_2N)$，最坏$O(N^2)$；空间复杂度为$O(N)$。

执行用时：382ms，在所有java提交中击败了5.10%的用户。

内存消耗：48MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;暂无，密切关注。社区中运行较快的方法将链表替换为数组，运行时间大致在3ms，思路一致。