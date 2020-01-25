[toc]

Given a sorted linked list, delete all duplicates such that each element appear only once.



## 题目解读

&emsp;给定有序链表，删除重复的结点，只保留一个。数据结构为链表，入参为链表头。

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
    public ListNode deleteDuplicates(ListNode head) {
    }
}
```

## 程序设计

* 最简单的，在遍历的过程中保留第一个结点，其后重复的结点进行删除操作即可。

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        ListNode tempNode = head;
        // 遍历链表，每个结点与后继结点对比
        while(tempNode.next != null) {
            // 重复结点
            if(tempNode.val == tempNode.next.val) {
                // 删除操作
                tempNode.next = tempNode.next.next;
            } 
            // 不重复结点，继续遍历
            else {
                tempNode = tempNode.next;
            }
        }
        return head;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$。空间复杂度$O(1)$。

执行用时：1ms，在所有java提交中击败了98.64%的用户。

内存消耗：36.7MB，在所有java提交中击败了41.84%的用户。

## 官方解题

&emsp;官方解题思路与上述一致。

## 代码改进

&emsp;上述代码对于重复结点，每遍历一次便操作一次删除操作，可以优化为遍历到重复结点的尾结点，再做重复链表的删除，只需一次删除。

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        ListNode tempNode = head;
        ListNode traversalNode;
        while(tempNode.next != null) {
            if(tempNode.val == tempNode.next.val) {
                traversalNode = tempNode.next;
                // traversalNode结点遍历到重复结点尾结点，再做删除，而不是一个个删除
                while(traversalNode.next != null && traversalNode.val == traversalNode.next.val){
                        traversalNode = traversalNode.next;
                }
                tempNode.next = traversalNode.next;
            } else {
                tempNode = tempNode.next;
            }
        }
        return head;
    }
}
```

时间复杂度、空间复杂度分别是$O(N)$、$O(1)$。