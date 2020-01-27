[toc]

Write a program to find the node at which the intersection of two singly linked lists begins.

Notes:

* If the two linked lists have no intersection at all, return null.
* The linked lists must retain their original structure after the function returns.
* You may assume there are no cycles anywhere in the entire linked structure.
* Your code should preferably run in O(n) time and use only O(1) memory.



## 题目解读

&emsp;给定两个链表，判断他们相交的结点。根据链表的特性，所谓链表相交指的是两个链表共用一段链表到结尾。题目还提出，若不存在则返回`null`，程序不能修改原来的链表结构，同时不存在环（这点很重要）。数据结构为单链表，入参为两个链表头。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    
    }
}
```

## 程序设计

* 两个链表是否相交，即两个链表是否存在共用的子链表，且这个子链表一定在两个链表结尾。首先可以遍历得到连个链表的表长，得到表长的差值后，让表比较长的链表先遍历差值步，然后连个链表同步遍历，第一次遍历到相同结点就是相交结点。

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode tempA = headA, tempB = headB;
        int lenA = getSize(tempA);
        int lenB = getSize(tempB);

        tempA = headA;
        tempB = headB;
        // 表比较长的链表先走差值步
        if(lenA - lenB > 0) {
            tempA = getNthNode(tempA, lenA - lenB);
        } else {
            tempB = getNthNode(tempB, lenB - lenA);
        }
        // 一同遍历（若有相交点，则剩余表长相同）
        while(tempA != null && tempB != null) {
            // 找到相交点
            if(tempA == tempB) {
                return tempA;
            }
            tempA = tempA.next;
            tempB = tempB.next;
        }
        // 遍历完两个链表中的一个或两个，没有找到相交点，即不相交
        return null;
    }

    // 获取表长
    private int getSize(ListNode head) {
        int len = 0;
        while(head != null) {
            head = head.next;
            len++;
        }
        return len;
    }

    // 获取第n个结点
    private ListNode getNthNode(ListNode head, int n) {
        while(head != null && n > 0) {
            head = head.next;
            n--;
        }
        // n大于表长
        if(n > 0) {
            return null;
        }
        return head;
    }
}
```

测试样例：不相交链表返回`null`，相交链表返回相交的第一个结点。

## 性能分析

&emsp;整体遍历链表三次，总的时间复杂度为$O(\max\{N,M\})$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.9MB，在所有java提交中击败了15.13%的用户。

## 官方解题

&emsp;思路同上，还提供了暴力和字典保存遍历结点两种可行基础解法，不做讨论。