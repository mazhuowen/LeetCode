[toc]

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.



## 题目解读

&emsp;题目要求遍历两个有序链表并合并为一个有序链表，要求拼接给定结点而不是创建新结点。数据结构为单链表，方法传入两个链表，注意此处未说明链表是否为空。

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
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    }
}
```

## 程序设计

* 题目只是简单的链表遍历，在遍历过程中需要判断当前的两个结点，将小的那个结点拼接到新链表后面，然后取该结点的后继继续与另一个结点比较。需要考虑到一个链表遍历完后另一链表需要拼接及空链表的问题。

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // 链表为空，直接返回另一个链表
        // 哑结点，避免表首的判断
        ListNode dummy = new ListNode(0);
        // 当前需要拼接的结点
        ListNode tmpNode = dummy;
        // 遍历比较结点，直到其中一个链表遍历完
        while(l1 != null && l2 != null){
            	// 将小的结点拼接到tmpNode后面，并遍历下一个结点
                if(l1.val < l2.val){
                    tmpNode.next = l1;
                    tmpNode = l1;
                    l1 = l1.next;
                } else {
                    tmpNode.next = l2;
                    tmpNode = l2;
                    l2 = l2.next;
                }
        }
        // 拼接未遍历完的链表，由于有序性，未遍历完的链表必然大于前面的结点，直接拼接
        if(l1 != null) {
            tmpNode.next = l1;
        }
        if(l2 != null) {
            tmpNode.next = l2;
        }
        // 返回拼接后的链表
        return dummy.next;
    }
}
```

&emsp;测试样例：$1 \to 2 \to 4$和$1 \to 4 \to 4$合并为$1 \to 1 \to 2 \to 3 \to 4 \to 4$测试普通情况；两个不等长链表测试遍历算法；至少一个链表为null测试特殊情况。

## 性能分析

&emsp;时间复杂度为遍历两个链表所费时间，注意此处依次比较遍历两个链表，时间复杂度为线性，具体为两个链表表长之和。空间复杂度创建了两个指针结点，为常量级。

执行用时：1ms，在所有java提交中击败了85.53%的用户。

内存消耗：40.1MB，在所有Java提交中击败了5.26%的用户。

## 官方解题

&emsp;除了上述遍历的思路，官方还提供了一种递归的方式：

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // 特殊情况处理
        if (l1 == null) {
            return l2;
        }
        else if (l2 == null) {
            return l1;
        }
        // 比较两个结点，拼接小的结点，并递归比较
        else if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }
        else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }

    }
}
```

&emsp;时间复杂度为两个链表表长之和。由于递归时传入参数总有一个已经遍历过，而遍历为两链表表长之和，空间复杂度为两个链表表长之和。