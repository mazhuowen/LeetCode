[toc]

Given a non-empty, singly linked list with head node `head`, return a middle node of linked list.

If there are two middle nodes, return the second middle node.



**Note:**

- The number of nodes in the given list will be between $1$ and $100$.



## 题目解读

&emsp;返回链表中点。

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
    public ListNode middleNode(ListNode head) {
        
    }
}
```

## 程序设计

* 哑节点加快慢指针。

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode slow = dummy, fast = dummy;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return fast == null ? slow : slow.next; 
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2MB，在所有java提交中击败了34.45%的用户。

## 官方解题

&emsp;官方思路类似。