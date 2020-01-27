[toc]

Remove all elements from a linked list of integers that have value **val**.



## 题目解读

&emsp;删除链表中制定值的结点，为简单的链表遍历。

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
    public ListNode removeElements(ListNode head, int val) {
        
    }
}
```

## 程序设计

* 引入哑结点，遍历时不采用当前结点，采用前驱结点。

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        // 引入哑结点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode temp = dummy;
        // 以前驱的形式遍历
        while(temp.next != null) {
            // 后继结点是待删除结点
            if(temp.next.val == val) {
                // 删除
                temp.next = temp.next.next;
            }
            else {
                // 继续遍历
                temp = temp.next;
            }
        }
        return dummy.next;
    }
}
```

测试样例：空表、同元素链表删除该元素返回空测试特殊情况；一般链表测试普通情况。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.1MB，在所有java 提交中击败了10.54%的用户。

## 官方解题

&emsp;思路同上。