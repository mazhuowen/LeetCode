[toc]

Reverse a singly linked list.



Follow up:

A linked list can be reversed either iteratively or recursively. Could you implement both?



## 题目解读

&emsp;给定链表，翻转链表，分别使用非递归、递归方法实现。数据结构为单链表，入参为链表头。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        
    }
}
```

## 程序设计

* 非递归实现引入哑结点，遍历时不断插入哑结点后，完成链表翻转。

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode traversal = dummy.next;
        ListNode temp;
        while(traversal.next != null) {
            // 删除后继结点
            temp = traversal.next;
            traversal.next = temp.next;
            // 插入哑结点后
            temp.next = dummy.next;
            dummy.next = temp;
        }
        return dummy.next;
    }
}
```

* 递归实现假设方法传入当前结点，返回翻转结点的起始结点。若当前结点$k$后的结点已完成翻转：$1 \to 2 \to \dots \to k \to k + 1 \gets k + 2 \gets \dots \gets N$，我们要做的是将$k + 1$指向$k$。

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        // 函数返回root为根节点
        ListNode root = reverseList(head.next);
        // k+1结点指向k
        head.next.next = head;
        // 此时k+1指向k，k指向k+1，形成循环链表，需要断掉
        head.next = null;
        return root;
    }
}
```

测试样例：$1 \to 2 \to 3 \to 4 \to 5$输出$5 \to 4 \to 3 \to 2 \to 1$。

## 性能分析

&emsp;非递归实现时间复杂度$O(N)$，空间复杂度$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.3MB，在所有java提交中击败了10.44%的用户。

&emsp;递归实现时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.3MB，在所有java提交中击败了12.57%的用户。

## 官方解题

&emsp;同上。