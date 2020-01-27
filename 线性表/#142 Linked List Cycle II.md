[toc]

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

Note: Do not modify the linked list.



## 题目解读

&emsp;是[#141 Linked List Cycle](./#141 Linked List Cycle.md)的拓展，在判断完是否存在环后还要给出环的起始结点，不存在返回`null`。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        
    }
}
```

> 题目描述模棱两可，需要与面试官沟通；比如没有环时返回是否为`null`等。

## 程序设计

* 在[#141 Linked List Cycle](./#141 Linked List Cycle.md)中一已经找到在环内的结点，在此题中需要找到环的起始结点；一个思路是通过环内结点遍历得到环的表长`len`，从而可利用双指针将问题转化为寻找链表倒数第`len`个结点的问题。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null || head.next == null){
            return null;
        }
        // 快慢指针
        ListNode slow = head, fast = head, cycleNode = null;
        while(fast != null && fast.next != null && fast.next.next != null){
            fast = fast.next.next;
            slow = slow.next;
            // 存在环
            if (slow == fast) {
                cycleNode = fast;
                break;
            }
        }
        // 不存在环
        if(cycleNode == null) {
            return null;
        }

        // 统计环的表长
        int len = 1;
        while (fast.next != cycleNode){
            fast = fast.next;
            len++;
        }

        // 快指针先走len步
        fast = slow = head;
        for(; len > 0; len--) {
            fast = fast.next;
        }
		// 双指针同时遍历，若是环的起始结点，则两个指针正好遍历到一个结点
        while(slow != fast){
            slow = slow.next;
            fast = fast.next;
        }

        return fast;
    }
}
```
测试样例：空表、普通无环链表、普通有环链表、循环链表。

## 性能分析

&emsp;整体需要三次链表遍历，总的时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了55.28%的用户。

内存消耗：38.2MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;官方除了提供基础的使用字典保存遍历过的点的方法，第二种方法更为一般化。实际上仔细想想上述过程，快指针比慢指针要快一倍，也就是快慢指针相遇的时候，快指针比慢指针多走了一个环的距离，而又快指针比慢指针快一倍，即慢指针走了$l$，快指针走了$2l$，而$2l - l = len \implies l = len$，可见快慢指针相遇的地方就是从头结点出发遍历`len`步的位置。再次审视我们的代码，会发现我们有两个环节：计算环的表长、快指针先走`len`步是多余的，代码可进一步修改：

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null || head.next == null){
            return null;
        }
        // 快慢指针
        ListNode slow = head, fast = head, cycleNode = null;
        while(fast != null && fast.next != null && fast.next.next != null){
            fast = fast.next.next;
            slow = slow.next;
            // 存在环
            if (slow == fast) {
                cycleNode = fast;
                break;
            }
        }
        // 不存在环
        if(cycleNode == null) {
            return null;
        }

        // 此时fast距离head为len距离，将slow归位为head
        slow = head;
        // 双指针寻找倒数第len个结点，也就是环的起始结点
        while(slow != fast){
            slow = slow.next;
            fast = fast.next;
        }

        return fast;
    }
}
```

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.6MB，在所有java提交中击败了5.03%的用户。