[toc]

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.



## 题目解读

&emsp;给定有序链表，删除所有重复的结点，注意此处不是去重保留一个，而是只要重复就删除。主要知识点是链表删除操作。数据结构为单链表，入参为链表头。

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

* 首先考虑到头结点是重复结点，为了便于操作，引入哑结点将特殊一般化；考虑一段重复结点的删除，我们必须知道前驱结点和后继结点，将前驱与后继结点连在一起即可完成删除操作。可见我们需要维护两个指针，一个指针指向重复链表前驱，一个指针指向重复链表结尾（通过结尾可得到后继）。
* 遍历时，第二个指针指向的结点与后继结点比较，如果相同则继续遍历，直到与后继结点不相同。此时第二个指针就是当前结点重复元素的尾结点。当前结点重复元素组成的链表分两种情况：只包含一个；包含多个。显然我们要删除的是包含多个结点的链表。第一个指针的后继若是第二个指针，表明只有一个元素，无需删除；否则需要删除。
* 删除重复链表后，第一个指针不变，第二个指针继续遍历；否则两个指针遍历到下一个结点，然后第二个指针继续遍历。

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        // 哑结点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // 双指针，指向重复链表的前驱和结尾
        ListNode firstNode = dummy, secondNode = dummy.next;

        // 每次和后继结点比较
        while(secondNode.next != null){
            // 相同结点，secondNode遍历到重复链表的结尾
            if(secondNode.val == secondNode.next.val){
                secondNode = secondNode.next;
            } 
            // 不同结点，且都为单独出现不重复，两个指针遍历下一个
            else if(firstNode.next == secondNode){
                firstNode = firstNode.next;
                secondNode = secondNode.next;
            } 
            // 不同结点，中间有跨度，需要删除中间的相同结点
            else {
                // 删除相同结点
                firstNode.next = secondNode.next;
                secondNode = secondNode.next;
            }
        }
        // while循环没有对结尾做判断，对结尾做判断
        if(firstNode.next != secondNode){
            firstNode.next = null;
        }
         return dummy.next;
    }
}
```

上述代码while循环体没有处理结尾重复的情况，需要特殊处理。测试样例：$1 \to 2 \to 3 \to 3 \to 4 \to 4 \to 5$，输出$1 \to 2 \to 5$测试普通情况；$1 \to 1 \to 1$，输出为null，测试特殊情况；$1 \to 1 \to 2$输出$2$，测试头结点重复的情况；$1 \to 2 \to 2$输出$2$，测试尾结点重复的情况。

## 性能分析

&emsp;整体是链表的遍历删除，时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了98.67%的用户。

内存消耗：36.7MB，在所有java提交中击败了27.08%的用户。

## 官方解题

&emsp;暂无，密切关注。社区方法思路类似，实现格式稍有不同。