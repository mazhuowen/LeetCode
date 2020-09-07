[toc]

Given an array `nums` that represents a permutation of integers from $1$ to $n$. We are going to construct a binary search tree (BST) by inserting the elements of `nums` in order into an initially empty BST. Find the number of different ways to reorder nums so that the constructed BST is identical to that formed from the original array `nums`.

For example, given `nums = [2,1,3]`, we will have 2 as the root, 1 as a left child, and 3 as a right child. The array `[2,3,1]` also yields the same BST but `[3,2,1]` yields a different BST.

Return the number of ways to reorder `nums` such that the BST formed is identical to the original BST formed from `nums`.

Since the answer may be very large, **return it modulo** $10^9 + 7$.



**Constraints**:

* $1 \le \text{nums.length} \le 1000$
* $1 \le \text{nums[i]} \le \text{nums.length}$
* All integers in `nums` are **distinct**.



## 题目解读

&emsp;给定二叉查找树的插入顺序，求可得到一样树结构的所有插入顺序数目。

```java
class Solution {
    public int numOfWays(int[] nums) {
        
    }
}
```

## 程序设计

* 首先对于同一个二叉查找树结构，父子节点的插入顺序一定是固定的，即父节点必须在子节点前插入，而对于不同子节点及其子孙节点，插入顺序是灵活的；
* 假设当前节点左子树有$l$个节点，右子树有$r$个节点，则可得到递归公式$f(cur) = C_{l + r}^l * f(left) * f(right)$。

> 计算组合可使用递归公式：$C_n^k = C_{n - 1}^{k - 1} + C_{n - 1}^{k}$。

```java
class Solution {
    private static final int MOD = 1_000_000_007;
    // 组合加速数组
    int[][] record = new int[1001][1001];

    public int numOfWays(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        for (int[] array : record) Arrays.fill(array, -1);

        // 构建二叉查找树
        Tree root = buildTree(nums);
        return (int)count(root) - 1;
    }

    private Tree buildTree(int[] nums) {
        Tree root = new Tree(nums[0], 1);
        for (int i = 1; i < nums.length; i++) {
            root.insert(nums[i]);
        }
        return root;
    }

    private long count(Tree root) {
        if (root == null) return 1;
        // 子孙节点组合数
        long res = combination(size(root) - 1, size(root.left));
        res = (res * count(root.left)) % MOD;
        res = (res * count(root.right)) % MOD;
        return res;
    }

    // 组合计算
    private int combination(int n, int m) {
        if (m == 0 || m == n) return 1;
        if (record[n][m] != -1) return record[n][m];
        return record[n][m] = (combination(n - 1, m - 1) + combination(n - 1, m)) % MOD;
    }

    private int size(Tree root) {
        return root == null ? 0 : root.size;
    }
}

class Tree {
    int val;
    int size;

    Tree left;
    Tree right;

    Tree(int val, int size) {
        this.val = val;
        this.size = size;
    }

    public void insert(int val) {
        this.size++;
        if (val > this.val) {
            if (this.right == null) this.right = new Tree(val, 1);
            else this.right.insert(val);
        } else {
            if (this.left == null) this.left = new Tree(val, 1);
            else this.left.insert(val);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，最坏$O(N^2)$，空间复杂度为$O(N)$。

执行用时：233ms，在所有java提交中击败了26.57%的用户。

内存消耗：51.3MB，在所有java提交中击败了37.40%的用户。

## 官方解题

&emsp;官方思路类似，但不会构建二叉查找树，而是每次将小于当前根节点的值划入左子树，大于的值划入右子树；除了上述思路，官方还提供了采用乘法逆元优化求取组合的时间复杂度，将$O(N^2)$将为$O(N)$。