[toc]

A `binary expression tree` is a kind of binary tree used to represent arithmetic expressions. Each node of a binary expression tree has either zero or two children. Leaf nodes (nodes with 0 children) correspond to operands (numbers), and internal nodes (nodes with 2 children) correspond to the operators `'+'` (addition), `'-'` (subtraction), `'*'` (multiplication), and `'/'` (division).

For each internal node with operator `o`, the `infix expression` that it represents is `(A o B)`, where `A` is the expression the left subtree represents and `B` is the expression the right subtree represents.

You are given a string `s`, an **infix expression** containing operands, the operators described above, and parentheses `'('` and `')'`.

Return any valid **binary expression tree**, which its `in-order traversal` reproduces `s` after omitting the parenthesis from it (see examples below).

**Please note that order of operations applies in** `s`. That is, expressions in parentheses are evaluated first, and multiplication and division happen before addition and subtraction.

Operands must also appear in the **same order** in both `s` and the in-order traversal of the tree.



**Constraints**:

* $1 \le \text{s.length} \le 10^5$
* `s` consists of digits and the characters `'+'`, `'-'`, `'*'`, `'/'`, `'('`, and `')'`.
* Operands in `s` are **exactly** $1$ digit.
* It is guaranteed that `s` is a valid expression.



## 题目解读

&emsp;根据表达式构建计算树。

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
    public Node expTree(String s) {
        
    }
}
```

## 程序设计

* 考虑到后缀表达式是计算树的前序序列，一个基本思路是将中缀表达式转化为后缀表达式，然后结合中缀表达式唯一确定一个树；事实上对于计算树，操作符是操作数的父节点，在转化为后缀表达式的过程中子节点都是已知的，可直接拼接到父节点生成计算树；
* 代码为常规中缀表达式转后缀表达式，只是数字不再是维护在链表里，而是也维护在栈中，在每次高优先级操作符出栈时也不是拼接到链表后面，而是与栈中的两个操作数结合，并入数字栈。

```java
class Solution {
    public Node expTree(String s) {
        if (s == null || s.isEmpty()) return null;
        // 存储数字及表达式（合并操作符的子树）
        Stack<Node> nums = new Stack<>();
        // 存储符号
        Stack<Node> ops = new Stack<>();

        for (char c : s.toCharArray()) {
            switch(c) {
                case '(':
                    ops.add(new Node(c));
                    break;
                case ')':
                    // 出栈所有操作符
                    while (!ops.isEmpty() && ops.peek().val != '(') transform(nums, ops);
                    // 出栈开括号
                    ops.pop();
                    break;
                case '+':
                case '-':
                    while (!ops.isEmpty() && ops.peek().val != '(') transform(nums, ops);
                    // 入栈符号
                    ops.push(new Node(c));
                    break;
                case '*':
                case '/':
                    while (!ops.isEmpty() && (ops.peek().val == '*' || ops.peek().val == '/')) transform(nums, ops);
                    // 入栈符号
                    ops.push(new Node(c));
                    break;
                // 数字
                default:
                    nums.push(new Node(c));
                    break;
            }
        }
        while (!ops.isEmpty()) transform(nums, ops);
        return nums.pop();
    }

    private void transform(Stack<Node> nums, Stack<Node> ops) {
        // 操作符与数字结合
        Node num2 = nums.pop(), num1 = nums.pop();
        ops.peek().left = num1;
        ops.peek().right = num2;
        nums.push(ops.pop());
    }
}
```

* 改为数组栈：

```java
class Solution {
    public Node expTree(String s) {
        if (s == null || s.isEmpty()) return null;
        // 存储数字及表达式
        int idx1 = 0;
        Node[] nums = new Node[s.length()];
        // 存储符号
        int idx2 = 0;
        Node[] ops = new Node[s.length()];

        for (char c : s.toCharArray()) {
            switch(c) {
                case '(':
                    ops[idx2++] = new Node(c);
                    break;
                case ')':
                    // 出栈所有操作符
                    while (idx2 > 0 && ops[idx2 - 1].val != '(') {
                        transform(nums, idx1, ops, idx2);
                        idx1--;
                        idx2--;
                    }
                    // 出栈开括号
                    idx2--;
                    break;
                case '+':
                case '-':
                    while (idx2 > 0 && ops[idx2 - 1].val != '(') {
                        transform(nums, idx1, ops, idx2);
                        idx1--;
                        idx2--;
                    }
                    // 入栈符号
                    ops[idx2++] = new Node(c);
                    break;
                case '*':
                case '/':
                    while (idx2 > 0 && (ops[idx2 - 1].val == '*' || ops[idx2 - 1].val == '/')) {
                        transform(nums, idx1, ops, idx2);
                        idx1--;
                        idx2--;
                    }
                    // 入栈符号
                    ops[idx2++] = new Node(c);
                    break;
                // 数字
                default:
                    nums[idx1++] = new Node(c);
                    break;
            }
        }
        while (idx2 > 0) {
            transform(nums, idx1, ops, idx2);
            idx1--;
            idx2--;
        }
        return nums[idx1 - 1];
    }

    private void transform(Node[] nums, int idx1, Node[] ops, int idx2) {
        // 操作符与数字结合
        Node num2 = nums[--idx1], num1 = nums[--idx1];
        ops[idx2 - 1].left = num1;
        ops[idx2 - 1].right = num2;
        nums[idx1++] = ops[--idx2];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：31ms，在所有java提交中击败了57.14%的用户。

内存消耗：43.5MB，在所有java提交中击败了100.00%的用户。

&emsp;改为数组栈，时间性能提高。

执行用时：10ms，在所有java提交中击败了100.00%的用户。

内存消耗：45.4MB，在所有java提交中击败了57.14%的用户。

## 官方解题

&emsp;暂无，密切关注。