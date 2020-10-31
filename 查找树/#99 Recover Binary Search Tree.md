[toc]

Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.



**Follow up**:

* A solution using $O(n)$ space is pretty straight forward.
* Could you devise a constant space solution?



## 题目解读

&emsp;二叉树有两个元素位置错误，纠正得到正确的结构。

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
    public void recoverTree(TreeNode root) {

    }
}
```

## 程序设计

* 二叉查找树的特质就是中序遍历是有序的，如果有两个位置有错误，则序列就不是单增，这两个数前面的一个大于其前驱和后继，后面的一个小于其前驱和后继。可以利用这个特性进行中序遍历，和前一个结点值作对比。
* 特别注意两个错误元素在中序遍历中相邻的情况，这种情况极有可能漏掉第二个结点。

```java
class Solution {
    // 记录遍历中的两个结点
    private TreeNode pre;
    // 记录错误的两个结点
    private TreeNode first, second;
    public void recoverTree(TreeNode root) {
        inorderTraverse(root);
        // 交换位置
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
    }

    private void inorderTraverse(TreeNode root) {
        // 递归终止条件
        if(root == null) return;
        
        inorderTraverse(root.left);
        // pre记录上一个结点
        if(pre != null && root.val < pre.val) {
            // 第一个违法结点为pre
            if(first == null) first = pre;
            // root可能是第二个违法结点，这一步很关键
            // 这种情况一般是两个违规结点相邻的情况，如果不是这种情况后续会更新覆盖
            second = root;
        }
        pre = root;
        inorderTraverse(root.right);
    } 
}
```

* 迭代方式实现：

```java
class Solution {
    public void recoverTree(TreeNode root) {
        TreeNode first = null, second  = null;
        TreeNode pre = null;
        Stack<TreeNode> stack = new Stack<>();
        Stack<Integer> counter = new Stack<>();
        stack.push(root);
        counter.push(1);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            Integer count = counter.pop();
            // 遍历当前结点
            if(count++ == 2) {
                // 更新结点
                if(pre != null && cur.val < pre.val) {
                    if(first == null) {
                        first = pre;
                    }
                    second = cur;
                }
                // 迭代记录前驱
                pre = cur;
                
                if(cur.right != null) {
                    stack.push(cur.right);
                    counter.push(1);
                }
            } 
            // 入栈左子节点
            else {
                stack.push(cur);
                counter.push(count);
                if(cur.left != null) {
                    stack.push(cur.left);
                    counter.push(1);
                }
            }
        }
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：2ms，在所有java提交中击败了99.91%的用户。

内存消耗：40.8MB，在所有java提交中击败了28.13%的用户。

&emsp;迭代方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：5ms，在所有java提交中击败了22.50%的用户。

内存消耗：40.9MB，在所有java提交中击败了26.67%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了Morris中序遍历的思路。Morris算法的思想是在节点和它的直接前驱之间设置一个临时的链接：`predecessor.right = root`，从该节点开始，找到它的直接前驱并验证是否存在链接：如果没有链接，设置连接并走向左子树；如果有连接，断开连接并走向右子树。这里有一个小问题要处理：如果该节点没有左孩子，即没有左子树，则直接走向右子树。

```java
class Solution {
  public void swap(TreeNode a, TreeNode b) {
    int tmp = a.val;
    a.val = b.val;
    b.val = tmp;
  }

  public void recoverTree(TreeNode root) {
    TreeNode first = null, second = null, pre = null, predecessor = null;

    while (root != null) {
      // 如果root存在左子节点，predecessor设置为root的前驱，即左子节点的右支路每段
      if (root.left != null) {
        predecessor = root.left;
        while (predecessor.right != null && predecessor.right != root)
          predecessor = predecessor.right;

        // 中序遍历predecessor的right指向root，predecessor为root的前驱，并走进左子树
        if (predecessor.right == null) {
          predecessor.right = root;
          // 走进左子树继续遍历
          root = root.left;
        }
        // 可能是存在右子节点也可能是predecessor设置的指向后继的指针，总之先遍历当前结点，后遍历右子节点
        else {
          // 遍历当前结点
          if (pre != null && root.val < pre.val) {
            if(first == null) first = pre;
            second = root;
          }
          pre = root;
		  // 中序结点已访问完，断开前驱指向中序结点链接
          predecessor.right = null;
          // 进入右子树遍历
          root = root.right;
        }
      }
      // 没有左子树，则遍历当前结点并继续遍历右子树
      else {
        // 违规结点
        if(pre != null && root.val < pre.val) {
          if(first == null) first = pre;
          second = root;
        }
        // 迭代
        pre = root;
		// 遍历右子树。
        root = root.right;
      }
    }
    // 交换
    swap(first, second);
  }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了99.91%的用户。

内存消耗：41.1MB，在所有java提交中击败了26.26%的用户。