[toc]

A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.

Write a data structure `CBTInserter` that is initialized with a complete binary tree and supports the following operations:

* `CBTInserter(TreeNode root)` initializes the data structure on a given tree with head node `root`;
* `CBTInserter.insert(int v)` will insert a `TreeNode` into the tree with value `node.val = v` so that the tree remains complete, **and returns the value of the parent of the inserted** `TreeNode`;
* `CBTInserter.get_root()` will return the head node of the tree.



**Note**:

* The initial given tree is complete and contains between $1$ and $1000$ nodes.
* `CBTInserter.insert` is called at most $10000$ times per test case.
* Every value of a given or inserted node is between $0$ and $5000$.



## 题目解读

&emsp;设计完全二叉树的插入，使得插入后仍然保持完全二叉树。

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
class CBTInserter {

    public CBTInserter(TreeNode root) {

    }
    
    public int insert(int v) {

    }
    
    public TreeNode get_root() {

    }
}

/**
 * Your CBTInserter object will be instantiated and called as such:
 * CBTInserter obj = new CBTInserter(root);
 * int param_1 = obj.insert(v);
 * TreeNode param_2 = obj.get_root();
 */
```

## 程序设计

* 由于不是维护二叉查找树，而是二叉树，故只需将叶子节点按照从低层到高层，从左到右的顺序保存在队列中，然后将新加的节点拼接到起始节点即可。

> 审题很重要，如果是二叉查找树，则会难很多。

```java
class CBTInserter {
    private TreeNode root;
    private Queue<TreeNode> queue;

    public CBTInserter(TreeNode root) {
        this.root = root;
        this.queue = new LinkedList<>();
        // 将叶节点加入队列
        initQueue();
    }

    // 层次遍历，将叶节点加入队列
    private void initQueue() {
        Queue<TreeNode> tmp = new LinkedList<>();
        tmp.add(this.root);
        while (!tmp.isEmpty()) {
            TreeNode cur = tmp.poll();
            // 缺失子节点，入队
            if (cur.left == null || cur.right == null) queue.add(cur);
            // 入队子节点
            if (cur.left != null) tmp.add(cur.left);
            if (cur.right != null) tmp.add(cur.right);
        }
    }
    
    public int insert(int v) {
        TreeNode node = new TreeNode(v);
        TreeNode parent = queue.peek();
        // 缺失左子节点
        if (parent.left == null) parent.left = node;
        // 缺失右子节点，出队父节点
        else queue.poll().right = node;
        queue.add(node);
        return parent.val;
    }
    
    public TreeNode get_root() {
        return root;
    }
}
```

## 性能分析

&emsp;构建队列时间复杂度为$O(N)$，空间复杂度为$O(N)$；插入时间复杂度为$O(1)$，空间复杂度于插入层数有关。

执行用时：20ms，在所有java提交中击败了94.44%的用户。

内存消耗：40.6MB，在所有java提交中击败了30.77%的用户。

## 官方解题

&emsp;同上。