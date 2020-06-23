[toc]

Print a binary tree in an $m \times n$ 2D string array following these rules:

* The row number `m` should be equal to the height of the given binary tree.
* The column number `n` should always be an odd number.
* The root node's value (in string format) should be put in the exactly middle of the first row it can be put. The column and the row where the root node belongs will separate the rest space into two parts (**left-bottom part and right-bottom part**). You should print the left subtree in the left-bottom part and print the right subtree in the right-bottom part. The left-bottom part and the right-bottom part should have the same size. Even if one subtree is none while the other is not, you don't need to print anything for the none subtree but still need to leave the space as large as that for the other subtree. However, if two subtrees are none, then you don't need to leave space for both of them.
* Each unused space should contain an empty string `""`.
* Print the subtrees following the same rules.

Note: The height of binary tree is in the range of `[1, 10]`.



## 题目解读

&emsp;打印二叉树到列表。

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
    public List<List<String>> printTree(TreeNode root) {

    }
}
```

## 程序设计

* 首先求树的高度，然后注意到当前根节点是数组中心位置，传递区间进行递归。

```java
class Solution {
    public List<List<String>> printTree(TreeNode root) {
        List<List<String>> res = new ArrayList<>();
        
        int high = getHigh(root);
        if (high == 0) return res;
        
        int size = (1 << high) - 1;
        initArray(res, high, size);
        
        traverseTree(root, 0, 0, size - 1, res);
        return res;
    }

    private int getHigh(TreeNode root) {
        if (root == null) return 0;
        return Math.max(getHigh(root.left), getHigh(root.right)) + 1;
    }

    private void initArray(List<List<String>> array, int high, int size) {
        for (int i = 0; i < high; i++) {
            List<String> row = new ArrayList<>();
            array.add(row);
            for (int j = 0; j < size; j++) {
                row.add("");
            }
        }
    }

    // 高度、当前根节点所在子树的索引区间
    private void traverseTree(TreeNode root, int high, int left, int right, List<List<String>> res) {
        if (root == null) return;

        // 根节点位置
        int mid = left + (right - left) / 2;
        res.get(high).set(mid, Integer.toString(root.val));

        traverseTree(root.left, high + 1, left, mid - 1, res);
        traverseTree(root.right, high + 1, mid + 1, right, res);
    }
}
```

## 性能分析

&emsp;遍历树时间复杂度为$O(N)$，初始化数组时间复杂度为$O(H * (2^H - 1))$；空间复杂度为$O(H * (2^H - 1))$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。