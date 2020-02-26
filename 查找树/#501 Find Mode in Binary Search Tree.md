[toc]

Given a binary search tree (BST) with duplicates, find all the `mode(s)` (the most frequently occurred element) in the given BST.

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than or equal to** the node's key.
* The right subtree of a node contains only nodes with keys **greater than or equal to** the node's key.
* Both the left and right subtrees must also be binary search trees.



Note: If a tree has more than one mode, you can return them in any order.

Follow up: Could you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).



## 题目解读

&emsp;存在重复数字的二叉查找树，找出重复频率最高的那个数。

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
    public int[] findMode(TreeNode root) {

    }
}
```

## 程序设计

* 使用递归的方式中序遍历，并统计重复值。

```java
class Solution {
    private int max = Integer.MIN_VALUE;
    private int count = 0;
    private int pre = 0;

    public int[] findMode(TreeNode root) {
        List<Integer> list = new LinkedList<>();
        inorderCount(root, list);
        int[] res = new int[list.size()];
        for(int i = 0; i < list.size(); i++) {
            res[i] = list.get(i);
        }
        return res;
    }

    private void inorderCount(TreeNode root, List<Integer> list) {
        if(root == null) {
            return;
        }
        inorderCount(root.left, list);
        // 当前结点与前驱值一致
        if(root.val == pre) {
            count++;
        } 
        // 不一致，则重置计数和前驱
        else {
            count = 1;
            pre = root.val;
        }
        // 判断计数和最大计数的比较，并更新
        if(count > max) {
            list.clear();
            list.add(root.val);
            // 更新最大频率
            max = count;
        } else if(count == max) {
            list.add(root.val);
        }
        inorderCount(root.right, list);
    }
}
```

* 递归的中序遍历空间复杂度不为常量，可以使用迭代的方式实现：

```java
class Solution {

    public int[] findMode(TreeNode root) {
        List<Integer> list = new LinkedList<>();
        inorderCount(root, list);
        int[] res = new int[list.size()];
        for(int i = 0; i < list.size(); i++) {
            res[i] = list.get(i);
        }
        return res;
    }

    private void inorderCount(TreeNode root, List<Integer> list) {
        if(root == null) {
            return;
        }
        int max = Integer.MIN_VALUE;
        int count = 0;
        int pre = 0;
        while(root != null) {
            // 存在左子树
            if(root.left != null) {
                TreeNode temp = root.left;
                while(temp.right != null && temp.right != root) {
                    temp = temp.right;
                }
                if(temp.right == null) {
                    temp.right = root;
                    root = root.left;
                } else {
                    if(root.val == pre) {
                        count++;
                    } else {
                        count = 1;
                        pre = root.val;
                    }
                    if(count > max) {
                        list.clear();
                        list.add(root.val);
                        max = count;
                    } else if(count == max) {
                        list.add(root.val);
                    }
                    temp.right = null;
                    root = root.right;
                }
            }
            // 不存在左子树
            else {
                if(root.val == pre) {
                    count++;
                } else {
                    count = 1;
                    pre = root.val;
                }
                if(count > max) {
                    list.clear();
                    list.add(root.val);
                    max = count;
                } else if(count == max) {
                    list.add(root.val);
                }
                root = root.right;
            }
        }
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.6MB，在所有java提交中击败了5.20%的用户。

&emsp;非递归方式采用莫里斯中序遍历，时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了42.35%的用户。

内存消耗：41.6MB，在所有java提交中击败了5.20%的用户。

## 官方解题

&emsp;暂无，密切关注。