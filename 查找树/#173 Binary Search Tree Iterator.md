[toc]

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.



**Note**:

* `next()` and `hasNext()` should run in average $O(1)$ time and uses $O(h)$ memory, where h is the height of the tree.
* You may assume that next() call will always be valid, that is, there will be at least a next smallest number in the BST when `next()` is called.
  

## 题目解读

&emsp;设计二叉查找树的迭代器，使得中序遍历输出元素。

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
class BSTIterator {

    public BSTIterator(TreeNode root) {

    }
    
    /** @return the next smallest number */
    public int next() {

    }
    
    /** @return whether we have a next smallest number */
    public boolean hasNext() {

    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

## 程序设计

* 由于题目限定时间复杂度为$O(1)$，空间复杂度为$O(h)$，这样就不能在构造函数中序遍历并计入链表。
* 官方给出通过栈保存中序遍历（就是中序遍历一种迭代形式的拆分），但是该方法`next()`方法不能保证单独操作为$O(1)$的时间复杂度，平摊后是$O(1)$的时间复杂度。

```java
class BSTIterator {
    private Stack<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        stack = new Stack<>();
        while(root != null) {
            stack.push(root);
            root = root.left;
        }
    }
    
    /** @return the next smallest number */
    public int next() {
        TreeNode cur = stack.pop();
        TreeNode temp = cur.right;
        while(temp != null) {
            stack.push(temp);
            temp = temp.left;
        }
        return cur.val;
    }
    
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.isEmpty();
    }
}
```

## 性能分析

&emsp;官方解法`next`方法时间复杂度最坏为$O(h)$，空间复杂度为$O(h)$。

执行用时：28ms，在所有java提交中击败了36.47%的用户。

内存消耗：44.4MB，在所有java提交中击败了99.10%的用户。

## 官方解题

&emsp;同上。