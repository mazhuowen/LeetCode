[toc]

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```c++
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

 

**Follow up**:

* You may only use constant extra space.
* Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

Constraints:

* The number of nodes in the given tree is less than 4096.
* $-1000 \le \text{node.val} \le 1000$



## 题目解读

&emsp;给定满二叉树，将同层结点`next`指针指向右侧相邻的结点，每一层最后一个结点`next`指向`null`。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/
class Solution {
    public Node connect(Node root) {
        
    }
}
```

## 程序设计

* 首先想到的是层次遍历，同层结点指向以后面的结点，最后一个结点指向空。

```java
class Solution {
    public Node connect(Node root) {
        if(root == null) {
            return null;
        }
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            int len = queue.size();
            Node temp;
            for(int i = 0; i < len - 1; i++) {
                temp = queue.poll();
                temp.next = queue.peek();
                if(temp.left != null) {
                    queue.add(temp.left);
                    queue.add(temp.right);
                }
            }
            temp = queue.poll();
            temp.next = null;
            if(temp.left != null) {
                queue.add(temp.left);
                queue.add(temp.right);
            }
        }
        return root;
    }
}
```

* 第$N$层节点之间建立`next`指针后，再建立第$N+1$层节点的`next`指针。可以通过`next`指针访问同一层的所有节点，因此可以使用第$N$层的`next`指针，为第`N+1`层节点建立`next`指针。


```java
class Solution {
    public Node connect(Node root) {
        if(root == null) {
            return null;
        }
        Node traversal = root;
        // 遍历左支路
        while(traversal.left != null) {
            Node parent = traversal;
            while(parent != null) {
                // 左儿子指向右儿子
                parent.left.next = parent.right;
                // 兄弟结点为空，则表示是该层最后一个结点，其右子节点也是最后一个结点
                if(parent.next == null) {
                    parent.right.next = null;
                } else {
                    parent.right.next = parent.next.left;
                }
                // 继续迭代
                parent = parent.next;
            }
            // 继续迭代
            traversal = traversal.left;
        }
        return root;
    }
}
```

## 性能分析

&emsp;层次遍历时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了21.43%的用户。

内存消耗：46.1MB，在所有java提交中击败了5.14%的用户。

&emsp;非层次遍历时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：47.6MB，在所有java提交中击败了5.14%的用户。

## 官方解题

&emsp;思路同上。社区还有递归解法，空间复杂度不是常数级别。

```java
class Solution {
    public Node connect(Node root) {
        if(root == null) {
            return null;
        }
        Node left = root.left;
        Node right = root.right;
        while(left != null) {
            // 左子节点指向右子节点
            left.next = right;
            // 实质上是相邻的右子节点指向父结点的兄弟节点的左子节点
            left = left.right;
            right = right.left;
        }
        // 递归
        connect(root.left);
        connect(root.right);
        return root;
    }
}
```