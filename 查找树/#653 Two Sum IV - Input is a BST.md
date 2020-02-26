[toc]

Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.



## 题目解读

&emsp;检查二叉查找树是否存在两个值之和等于`target`。

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
    public boolean findTarget(TreeNode root, int k) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        List<Integer> list = new LinkedList<>();
        inorder(root, k, list);
        // 双指针判断有序数组两数之和
        int left = 0, right = list.size() - 1;
        while(left < right) {
            int sum = list.get(left) + list.get(right);
            if(sum == k) {
                return true;
            } else if(sum < k) {
                left++;
            } else {
                right--;
            }
        }
        return false;
    }
	// 中序遍历
    private void inorder(TreeNode root, int k, List<Integer> list) {
        if(root == null) {
            return;
        }
        inorder(root.left, k, list);
        // 提前遍历结束
        if(list.size() > 0 && root.val + list.get(0) > k) {
            return;
        }
        list.add(root.val);
        inorder(root.right, k, list);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了92.42%的用户。

内存消耗：42.2MB，在所有java提交中击败了40.34%的用户。

## 官方解题

&emsp;官方除了上述思路，还提供了利用哈希表保存遍历过的数字，在遍历时判断的思路。