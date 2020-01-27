[toc]

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.



## 题目解读

&emsp;判断给定的链表中是否有环，输出是否有环。数据结构为单链表，对于存在环的单链表，没有尾结点，即后面部分是一个换；入参为链表头。同时题目进阶要求空间复杂度常量级。

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
    public boolean hasCycle(ListNode head) {
        
    }
}
```

## 程序设计

* 首先考虑到的是，在环上迭代会回到原来的位置，可以使用额外的空间记录某一个结点，如果重复遍历到该结点，则证明存在环。上面的问题在于，由于不确定哪个点在环上，我们不得不保存所有已遍历的结点，这样空间复杂度就不是题目要求的常量级。一般的，可使用快慢双指针，快指针总是比慢指针快一倍，如果某个时刻指针指向的结点相同则存在环，否则遍历到表尾不存在环。

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null){
            return false;
        }
        // 快慢指针
        ListNode fast = head, slow = head, cycleNode = null;
        // 若是循环链表没有尾结点
        while(fast != null && fast.next != null && fast.next.next != null){
            fast = fast.next.next;
            slow = slow.next;
            // 找到在环上的点
            if(fast == slow){
                cycleNode = fast;
                break;
            }
        }
        // 存在表尾，即不是循环链表
        if(cycleNode == null) {
            return false;
        }
       return true;
    }
}
```

测试样例：存在环的链表、无环链表、整体是循环链表的链表测试。

## 性能分析

&emsp;由于链表遍历在无环情况下是表长，有环情况比表长稍大，最坏情况遍历$2L - 1$，总的来说时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2MB，在所有java 提交中击败了99.50%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了基础的使用字典保存遍历过的结点，通过比对来判断重复的解法，较为基础。