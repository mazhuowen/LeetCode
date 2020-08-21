[toc]

You have `n` binary tree nodes numbered from `0` to `n - 1` where node `i` has two children `leftChild[i]` and `rightChild[i]`, return `true` if and only if **all** the given nodes form **exactly one** valid binary tree.

If node `i` has no left child then `leftChild[i]` will equal `-1`, similarly for the right child.

Note that the nodes have no values and that we only use the node numbers in this problem.

**Constraints:**

- $1 \le n \le 10^4$
- $\text{leftChild.length} == \text{rightChild.length} == n$
- $-1 \le \text{leftChild[i], rightChild[i]} \le n - 1$



## 题目解读

&emsp;给定每个结点的左右子结点，判断是否是一棵二叉树。

```java
class Solution {
    public boolean validateBinaryTreeNodes(int n, int[] leftChild, int[] rightChild) {

    }
}
```

## 程序设计

* 树的定义为除了根节点入度为$0$，其它结点入度为$1$，也就是说只能存在一个入度为$0$的点，其它点必须入度为$1$。
* 其次很容易忽视的一点就是根到任意一个结点都是连通的，如果只计算入度，则一个环和一个点的组合也是正确的，但是这显然不是树。

```java
class Solution {
    public boolean validateBinaryTreeNodes(int n, int[] leftChild, int[] rightChild) {
        if (n <= 0 || n != leftChild.length || n != rightChild.length) throw new IllegalArgumentException("invalid param");
        int[] inDegree = new int[n];
        for (int i = 0; i < n; i++) {
             // 入度超过1，不符合要求
            if (leftChild[i] != -1 && ++inDegree[leftChild[i]] > 1) return false;
            if (rightChild[i] != -1 && ++inDegree[rightChild[i]] > 1) return false;
        }
        // 查找根节点
        int root = -1;
        for (int i = 0; i < n; i++) {
            if (inDegree[i] == 0) {
                if (root == -1) root = i;
                // 存在多于一个点入度为0
                else return false;
            }
        }
        // 判断根到每个结点是否连通
        traserve(root, leftChild, rightChild, inDegree);
        for (int degree : inDegree) {
            if (degree > 0) return false;
        }
        return true;
    }

    private void traserve(int root, int[] left, int[] right, int[] degree) {
        if (root < 0) return;
        if (left[root] != -1) degree[left[root]]--;
        if (right[root] != -1) degree[right[root]]--;
        traserve(left[root], left, right, degree);
        traserve(right[root], left, right, degree);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了80.41%的用户。

内存消耗：42.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。