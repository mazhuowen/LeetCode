[toc]

You need to construct a binary tree from a string consisting of parenthesis and integers.

The whole input represents a binary tree. It contains an integer followed by zero, one or two pairs of parenthesis. The integer represents the root's value and a pair of parenthesis contains a child binary tree with the same structure.

You always start to construct the left child node of the parent first if it exists.



Note:

* There will only be `(`, `)`, `-` and`0~9`in the input string.
* An empty tree is represented by `""` instead of `"()"`.



## 题目解读

&emsp;给定类似前序遍历的形式，得出二叉树。题目规定`()`表示一个子树或叶节点，空树用`""`表示，其次题目限定每个字符都是独立的，即只有个位数数字。

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
    public TreeNode str2tree(String s) {
        
    }
}
```

## 程序设计

* 已知前序遍历的变形，得到二叉树可以使用栈，从左到右遍历字符串，遇到闭括号则出栈，此时栈顶结点是父节点，需要判断是左子节点还是右子节点。

```java
class Solution {
    public TreeNode str2tree(String s) {
        Stack<TreeNode> stack = new Stack<>();
        boolean sign = false;
        for(int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            switch(c) {
                // 闭括号出栈，维护树的关系
                case ')':
                    TreeNode cur = stack.pop();
                    TreeNode parent = stack.peek();
                    // 判断左右子节点
                    if(parent.left == null) {
                        parent.left = cur;
                    } else {
                        parent.right = cur;
                    }
                    break;
                case '(':
                    break;
                case '-':
                    sign = true;
                    break;
                default:
                    // 遍历数字并确定符号
                    int val = 0;
                    while(i < s.length() && Character.isDigit(s.charAt(i))) {
                        val = val * 10 + (s.charAt(i) - '0');
                        i++;
                    }
                    if(sign) {
                        val = -val;
                        sign = false;
                    }
                    TreeNode node = new TreeNode(val);
                    stack.push(node);
                    i--;
                    break;
            }
        }
        // 栈为空代表空树的情况
        return stack.isEmpty() ? null : stack.pop();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：15ms，在所有java提交中击败了51.51%的用户。

内存消耗：45.2MB，在所有java提交中击败了5.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有递归思路。

```java
class Solution {
    private int idx = 0;

    public TreeNode str2tree(String s) {
        if("".equals(s)) {
            return null;
        }
        // 化特殊为一般
        s = "(" + s + ")";
        return buildTree(s);
    }

    private TreeNode buildTree(String s) {
        int val = 0;
        boolean sign = false;
        // 开括号后是负号
        if(s.charAt(++idx) == '-') {
            // 后移
            idx++;
            sign = true;
        }
        // 数字
        while(idx < s.length() && Character.isDigit(s.charAt(idx))) {
            val = val * 10 + (s.charAt(idx++) - '0');
        }
        TreeNode head = new TreeNode(sign ? -val : val);

        // 叶节点或单独结点返回当前结点
        if(idx >= s.length() || s.charAt(idx) == ')') {
            // 移到闭括号后面
            idx++;
            return head;
        }

        // idx必然是开括号，为左结点，递归
        head.left = buildTree(s);
        if(idx >= s.length() || s.charAt(idx) == ')') {
            idx++;
            return head;
        }

        // idx必然是开括号
        head.right = buildTree(s);
        if(idx >= s.length() || s.charAt(idx) == ')') {
            idx++;
            return head;
        }
        return head;
    }
}
```

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：6ms，在所有java提交中击败了88.64%的用户。

内存消耗：46.8MB，在所有java提交中击败了5.00%的用户。