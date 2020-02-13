[toc]

One way to serialize a binary tree is to use pre-order traversal. When we encounter a non-null node, we record the node's value. If it is a null node, we record using a sentinel value such as #.

Given a string of comma separated values, verify whether it is a correct preorder traversal serialization of a binary tree. Find an algorithm without reconstructing the tree.

Each comma separated value in the string must be either an integer or a character '#' representing null pointer.

You may assume that the input format is always valid, for example it could never contain two consecutive commas such as "1,,3".



## 题目解读

&emsp;给定前序序列的变形，验证是否是二叉树的前序遍历。相比普通的前序遍历，题目中加入了空结点的表示`#`。

```java
class Solution {
    public boolean isValidSerialization(String preorder) {
        
    }
}
```

## 程序设计

* 单独的前序序列无法确定一棵二叉树，但在本题中加入了空结点`#`，使得可以确定一棵树。序列中`x##`表示的是叶节点，考虑到二叉树的删除，从叶节点开始删除，删除完后原来的结点位置是空结点；重复这个操作直到最后删除根节点。对应到前序序列中，先删除`x##`的叶节点，删除后是空结点，用`#`来替换；重复这个操作直到最后是空结点，即`#`。
* 这个过程可以用栈来实现，当栈顶是`#`，而当前遍历值也是`#`，则需要出栈两次，最后入栈`#`。

```java
class Solution {
    public boolean isValidSerialization(String preorder) {
        if(preorder == null || preorder.isEmpty()) {
            return true;
        }
        String[] express = preorder.split(",");
        Stack<String> stack = new Stack<>();
        for(int i = 0; i < express.length; i++) {
            String sympol = express[i];
            if("#".equals(sympol)) {
                // 栈顶也是#号，消解
                while(!stack.isEmpty() && "#".equals(stack.peek())) {
                    // 弹出兄弟
                    stack.pop();
                    // 弹出父结点（如果没有说明格式有误）
                    if(!stack.isEmpty()) {
                        stack.pop();
                    } else {
                        return false;
                    }
                }
            }
            // 入栈
            stack.push(sympol);
        }
        // 最后栈中只有#号
        return stack.size() == 1 && "#".equals(stack.peek());
    }
}
```

* 如果不把`#`看做空结点，而是看做叶节点，则二叉树中`#`表示叶节点，其它所有结点都是有两个子节点的结点，由于$n_0 = n_2 + 1$，$n_0$是`#`号的个数，$n_2$是数字的个数。其次在遍历过程中，叶结点树数要小于等于内部结点数，因为若此时$n_0 > n_2$，则意味着$n_0 \ge n_2 + 1$，即此时前面的点已经可以组成一棵二叉树，叶节点全部是`#`，而未遍历的后面的结点没有在树上拼接的位置。遵循这两个条件可得：

```java
class Solution {
    public boolean isValidSerialization(String preorder) {
        String[] express = preorder.split(",");
        int n0 = 0;
        int n2 = 0;
        for(int i = 0; i < express.length; i++) {
            if("#".equals(express[i])) {
                n0++;
            } else {
                n2++;
            }
            if(i != (express.length - 1) && n0 > n2) {
                return false;
            }
        }
        // 二叉树条件
        return n0 == (n2 + 1);
    }
}
```

## 性能分析

&emsp;栈的方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了23.71%的用户。

内存消耗：43MB，在所有java提交中击败了5.31%的用户。

&emsp;递归的方法时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了82.27%的用户。

内存消耗：43.6MB，在所有java提交中击败了5.31%的用户。

## 官方解题

&emsp;暂无，密切关注。