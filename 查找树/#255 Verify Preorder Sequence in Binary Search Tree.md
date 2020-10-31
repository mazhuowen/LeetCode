[toc]

Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree.

You may assume each number in the sequence is unique.



**Follow up:**
Could you do it using only constant space complexity?



## 题目解读

&emsp;给定序列，判断是否是二叉查找树的前序序列。

```java
class Solution {
    public boolean verifyPreorder(int[] preorder) {
        
    }
}
```

## 程序设计

* 对于二叉树，只凭借前序序列无法确认唯一一棵树，但是二叉查找树的特性是根节点左侧都小于根节点，右侧都大于根节点，结合前序序列的性质可以唯一确定一棵树。

```java
class Solution {
    public boolean verifyPreorder(int[] preorder) {
        return verifyPreorder(preorder, 0, preorder.length - 1);
    }

    private boolean verifyPreorder(int[] preorder, int start, int end) {
        if(start > end) return true;
        
        // 头结点
        int root = preorder[start];
        // 左右子树划分位置
        int split = -1;
        for(int i = start + 1; i <= end; i++) {
            // 存在右子树，赋值
            if(split == -1 && preorder[i] > root) {
                split = i;
            }
            // 在右子树存在比头结点数值小的元素，违法
            if(split != -1 && preorder[i] < root) {
                return false;
            }
        }
        // 递归，不存在右子树
        if(split == -1) {
            return verifyPreorder(preorder, start + 1, end);
        } 
        // 递归，存在左右子树
        else {
            return verifyPreorder(preorder, start + 1, split - 1) && verifyPreorder(preorder, split, end);
        }
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N^2)$；由于是尾调用，空间复杂度为$O(1)$。

执行用时：747ms，在所有java提交中击败了5.38%的用户。

内存消耗：42.7MB，在所有java提交中击败了5.68%的用户。

## 官方解题

&emsp;暂无，密切关注。社区运行性能较好的代码使用单调栈，巧妙的使用栈的特性来模拟二叉查找树遍历从左子树跨到右子树的规律。

```java
class Solution {
    public boolean verifyPreorder(int[] preorder) {
        // 栈保存递减序列，即左链路
        Stack<Integer> stack = new Stack<>();
        // 表示头结点值，初始为整数最小值
        int head = Integer.MIN_VALUE;
        for(int val : preorder) {
            // 大于头结点值，否则不合法（初始时头结点为最小值，这使得前序遍历的左链路可以进栈）
            if(val < head) return false;
            
            // 待入栈值比栈顶大，意味着从左子树跨到了右子树，更新head（后续的值都要大于head否则违法）
            while(!stack.isEmpty() && val > stack.peek()) {
                head = stack.pop();
            }
            // 入栈
            stack.push(val);
        }
        return true;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：12ms，在所有java提交中击败了89.78%的用户。

内存消耗：42.1MB，在所有java提交中击败了5.68%的用户。