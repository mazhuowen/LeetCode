[toc]

You are given a doubly linked list which in addition to the next and previous pointers, it could have a child pointer, which may or may not point to a separate doubly linked list. These child lists may have one or more children of their own, and so on, to produce a multilevel data structure, as shown in the example below.

Flatten the list so that all the nodes appear in a single-level, doubly linked list. You are given the head of the first level of the list.

**Constraints:**

* Number of Nodes will not exceed 1000.

- $1 \le \text{Node.val} \le 10^5$



## 题目解读

&emsp;给定多层双向链表，将其转换为单层双向链表。根据示例，转化规则为将`child`及其链表作为后继，原先的后继拼接在其后。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
};
*/
class Solution {
    public Node flatten(Node head) {
        
    }
}
```

## 程序设计

* 

```java
class Solution {
    public Node flatten(Node head) {
        if(head == null) {
            return head;
        }
        Node dummy = new Node(-1);
        Node tail = dummy;
        Node traversal;
        // 栈保存下次遍历的结点
        Stack<Node> temp = new Stack<>();
        temp.push(head);
        while(!temp.isEmpty()) {
            traversal = temp.pop();
            // 拼接到单层链表
            tail.next = new Node(traversal.val);
            tail.next.prev = tail;
            // 尾结点后移
            tail = tail.next;
            // 入栈后继结点，若不存在子节点，下次遍历从后继开始，否则遍历完子节点链表再从后继开始
            if(traversal.next != null) {
                temp.push(traversal.next);
            }
            // 存在子结点，入栈子结点，下次遍历从子节点开始
            if(traversal.child != null) {
                temp.push(traversal.child);
            } 
        }
        // 一定要中断哑结点与头结点的链接
        dummy.next.prev = null;
        return dummy.next;
    }
}

```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：1ms，在所有java提交中击败了29.32%的用户。

内存消耗：35.8MB，在所有java提交中击败了73.98%的用户。

## 官方解题

&emsp;官方还提供了递归的思路。

```java
class Solution {
    public Node flatten(Node head) {
        if(head == null) {
            return head;
        }
        Node dummy = new Node(-1);
        // 递归调用
        flatten(head, dummy);
        dummy.next.prev = null;
        return dummy.next;
    }

    private Node flatten(Node head, Node pre) {
        while(head != null) {
            // 拼接
            pre.next = head;
            pre.next.prev = pre;
            pre = pre.next;
            // 记录后继，防止拼接中断造成死循环
            Node temp = head.next;
            // 如果有子结点，拼接子节点，递归调用
            if(head.child != null) {
                // 子链表返回的尾结点就是新的pre结点
                pre = flatten(head.child, pre);
                // 释放子节点
                head.child = null;
            }
            // 继续遍历
            head = temp;
        }
        return pre;
    }
}
```

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.6MB，在所有java提交中击败了89.13%的用户。