[toc]

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.



Note:

* The linked list will have at least two elements.
* All of the nodes' values will be unique.
* The given node will not be the tail and it will always be a valid node of the linked list.
* Do not return anything from your function.



## 题目解读

&emsp;给定链表结点值都是唯一的，给定一个值，删除该值对应的结点，尾结点除外。题目规定链表至少有两个结点，给定的值不是尾结点的值（即不会删除尾结点）且在链表中存在。数据结构是单链表，入参为待删除结点。

> 此题描述误导性较大，需向面试官仔细确认，入参为待删除结点，却没提供链表。

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
    public void deleteNode(ListNode node) {
        
    }
}
```

## 程序设计

* 由于未提供链表，我们不知道待删除结点的前驱，无法常规操作删除该结点，只能待删除结点复制后继的值，然后删除后继，变相删除待删除结点。

```java
class Solution {
    public void deleteNode(ListNode node) {
        // 复制
       node.val = node.next.val;
        // 删除
       node.next = node.next.next;
    }
}
```

## 性能分析

&emsp;时间复杂度、空间复杂度均为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.1MB，在所有java提交中击败了5.10%的用户。

## 官方解题

&emsp;思路同上。