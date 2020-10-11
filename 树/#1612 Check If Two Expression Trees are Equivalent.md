[toc]

A `binary expression` tree is a kind of binary tree used to represent arithmetic expressions. Each node of a binary expression tree has either zero or two children. Leaf nodes (nodes with 0 children) correspond to operands (variables), and internal nodes (nodes with two children) correspond to the operators. In this problem, we only consider the `'+'` operator (i.e. addition).

You are given the roots of two binary expression trees, `root1` and `root2`. Return `true` if the two binary expression trees are equivalent. Otherwise, return `false`.

Two binary expression trees are equivalent if they **evaluate to the same value** regardless of what the variables are set to.

Follow up: What will you change in your solution if the tree also supports the `'-'` operator (i.e. subtraction)?

 

**Constraints**:

* The number of nodes in both trees are equal, odd and, in the range $[1, 4999]$.
* `Node.val` is `'+'` or a lower-case English letter.
* It's **guaranteed** that the tree given is a valid binary expression tree.



## 题目解读

&emsp;判断两个计算树结果是否相等。

```java
/**
 * Definition for a binary tree node.
 * class Node {
 *     char val;
 *     Node left;
 *     Node right;
 *     Node() {this.val = ' ';}
 *     Node(char val) { this.val = val; }
 *     Node(char val, Node left, Node right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean checkEquivalence(Node root1, Node root2) {
        
    }
}
```

## 程序设计

* 分别计算表达树，然后对比；
* 需注意节点中是字符，代表变量，不能转化为数值，可统计字符的数目。

```java
class Solution {
    public boolean checkEquivalence(Node root1, Node root2) {
        if (root1 == null || root2 == null) throw new IllegalArgumentException("invalid param");
        return Arrays.equals(calculation(root1), calculation(root2));
    }

    private int[] calculation(Node root) {
        // 叶节点返回
        if (root.val != '+') {
            int[] res = new int[26];
            res[root.val - 'a']++;
            return res;
        }
        return union(calculation(root.left), calculation(root.right));
    }

    private int[] union(int[] a, int [] b) {
        for (int i = 0; i < 26; i++) a[i] += b[i];
        return a;
    }
}
```

* 由于表达树操作数都是叶节点，而操作只有加法操作，可简化过程为遍历叶节点并统计。

```java
class Solution {
    public boolean checkEquivalence(Node root1, Node root2) {
        int[] c1 = new int[26], c2 = new int[26];
        dfs(root1, c1);
        dfs(root2, c2);
        return Arrays.equals(c1, c2);
    }

    private void dfs(Node root, int[] counter) {
        if (root.left == null) counter[root.val - 'a']++;
        else {
            dfs(root.left, counter);
            dfs(root.right, counter);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\max(N,M))$，空间复杂度为$O(\max(N,M))$。

执行用时：4ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.3MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;暂无，密切关注。