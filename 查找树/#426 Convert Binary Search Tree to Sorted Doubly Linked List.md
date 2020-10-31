[toc]

Convert a **Binary Search Tree** to a sorted **Circular Doubly-Linked List** in place.

You can think of the left and right pointers as synonymous to the predecessor and successor pointers in a doubly-linked list. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

We want to do the transformation **in place**. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. You should return the pointer to the smallest element of the linked list.



**Constraints**:

* $-1000 \le \text{Node.val} \le 1000$
* $\text{Node.left.val} < \text{Node.val} < \text{Node.right.val}$
* All values of `Node.val` are unique.
* $0 \le \text{Number of Nodes} \le 2000$



## 题目解读

&emsp;将二叉查找树转化为双向循环链表。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
    public Node treeToDoublyList(Node root) {
        
    }
}
```

## 程序设计

* 中序遍历连接各结点，需要格外的指针记录头尾。

```java
class Solution {
    // 记录头尾结点
    private Node first, last;

    public Node treeToDoublyList(Node root) {
        if(root == null) {
            return null;
        }
        inorderTraverse(root);
        first.left = last;
        last.right = first;
        return first;
    }

    private void inorderTraverse(Node root) {
        if(root == null) {
            return;
        }
        inorderTraverse(root.left);
        if(last != null) {
            last.right = root;
            root.left = last;
        } else {
            first = root;
        }
        last = root;
        inorderTraverse(root.right);
    }
}
```

* 迭代方式：

```java
class Solution {
    public Node treeToDoublyList(Node root) {
        Node first = null, last = null;
        while(root != null) {
            // 左子树存在
            if(root.left != null) {
                Node cur = root.left;
                while(cur.right != null && cur.right != root) {
                    cur = cur.right;
                }
                // 中序遍历连接后继并遍历左子树
                if(cur.right == null) {
                    cur.right = root;
                    root = root.left;
                } 
                // 左子树已遍历完，拼接当前结点
                else {
                    if(first == null) {
                        first = root;
                    } else {
                        last.right = root;
                        root.left = last;
                    }
                    last = root;
                    // 注意此处不能断开
                    //cur.right = null;
                    // 进入右子树
                    root = root.right;
                }
            } 
            // 不存在左结点，遍历当前结点并进入右子树
            else {
                if(first == null) {
                    first = root;
                } else {
                    last.right = root;
                    root.left = last;
                }
                last = root;
                // 进入右子树
                root = root.right;
            }
        }
        // 首尾相连
        if(last != null) {
            first.left = last;
            last.right = first;
        }
        return first;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.9MB，在所有java提交中击败了5.13%的用户。

&emsp;迭代方法时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.1MB，在所有java提交中击败了5.13%的用户。

## 官方解题

&emsp;同上。