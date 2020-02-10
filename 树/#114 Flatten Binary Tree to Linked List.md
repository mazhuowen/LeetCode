[toc]

Given a binary tree, flatten it to a linked list in-place.



## 题目解读

&emsp;给定二叉树，根据前序遍历顺序将数展开为链表，链表指针为`right`。

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
    public void flatten(TreeNode root) {
        
    }
}
```

## 程序设计

* 最容易想到使用前序遍历的递归形式，每当遍历一个结点就拼接该结点。需要在类里设置全局变量方便拼接。

```java
class Solution {
    private TreeNode pre = new TreeNode(-1);

    public void flatten(TreeNode root) {
        if(root == null) {
            return;
        }
        // 拼接当前结点
        pre.right = root;
        // 迭代
        pre = pre.right;
        // 记录左右子树
        TreeNode tempLeft = root.left;
        TreeNode tempRight = root.right;
        // 将当前结点左子树置空
        root.left = null;
        // 递归调用
        if(tempLeft != null) {
            flatten(tempLeft);
        }
        if(tempRight != null) {
            flatten(tempRight);
        }
    }
}
```

* 可不设置全局变量，如下：

```java
class Solution {
    public void flatten(TreeNode root) {
        TreeNode tail = new TreeNode(-1);
        flatten(root, tail);
    }

    private TreeNode flatten(TreeNode root, TreeNode tail) {
        if(root == null) {
            return null;
        }
        tail.right = root;
        tail = root;
        TreeNode leftTemp = root.left;
        TreeNode rightTemp = root.right;
        root.left = null;
        if(leftTemp != null) {
            tail = flatten(leftTemp, tail);
        }
        if(rightTemp != null) {
            tail = flatten(rightTemp, tail);
        }
        return tail;
    }
}
```

* 非递归方式在前序遍历非递归的实现上稍作修改。

```java
class Solution {
    public void flatten(TreeNode root) {
        if(root == null) {
            return;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        // 记录前一个结点
        TreeNode pre = null;
        while(!stack.isEmpty()) {
            TreeNode cur =  stack.pop();
            if(cur.right != null) {
                stack.push(cur.right);
            }
            if(cur.left != null) {
                stack.push(cur.left);
            }
            cur.left = null;
            if(pre != null) {
                pre.right = cur;
            }
            pre = cur;
        }
    }
}
```

## 性能分析

&emsp;递归实现时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏情况$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41MB，在所有java提交中击败了5.00%的用户。

&emsp;非递归实现时间复杂度$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了14.88%的用户。

内存消耗：41.2MB，在所有java提交中击败了5.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有一种非前序遍历的思路，每当遍历到一个结点，将其左子树拼接到右结点，而原先的右子树拼接到新的右子树的右支路末端；以此类推，直到没有左结点为止。

```java
class Solution {
    public void flatten(TreeNode root) {
        while(root != null) {
            // 左子节点不为空，拼接
            if(root.left != null) {
                // 将右子树拼接到左子树的右支路
                TreeNode temp = root.left;
                 while(temp.right != null) {
                    temp = temp.right;
                }
                temp.right = root.right;
                // 将左子树拼接到当前结点的右边
                root.right = root.left;
                // 置空
                root.left = null;
            }
            // 继续遍历
            root = root.right;
        }
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41MB，在所有java提交中击败了5.00%的用户。