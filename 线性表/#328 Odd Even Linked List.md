[toc]

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.



Note:

* The relative order inside both the even and odd groups should remain as it was in the input.
* The first node is considered odd, the second node even and so on ...



## 题目解读

&emsp;给定链表，将结点索引下标为奇数的结点放在链表前，为偶数的结点放在链表后。数据结构为单链表，入参为链表头。

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
    public ListNode oddEvenList(ListNode head) {
        
    }
}
```

## 程序设计

* 可以遍历偶索引结点，遍历到的结点删除并拼接到一起，这就是偶数索引的链表，然后再拼接到原链表后面。

```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null || head.next.next == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        ListNode traversal = head, even = dummy, temp;
        // traversal为奇数索引结点，后继为偶数索引结点
        while(traversal != null && traversal.next != null) {
            temp = traversal.next;
            // 删除偶数索引结点
            traversal.next = temp.next;
            // 尾插法
            even.next = temp;
            even = temp;
            // 奇索引结点后继为空，停止遍历（确保不为null，方便后续拼接）
            if(traversal.next == null) {
                break;
            }
            // 继续遍历
            traversal = traversal.next;
        }
        // 一定要中断，否则对于结点是奇数的链表，奇数链表和偶数链表表尾相连
        // 再进行下面连接操作会形成环路，导致测试程序死循环
        even.next = null;
        // 拼接
        traversal.next = dummy.next;
        return head;
    }
}
```

测试样例：$2 \to 1 \to 3 \to 5 \to 6 \to 4 \to 7$，输出$2 \to 3 \to 6 \to 7 \to 1 \to 5 \to 4$。

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2 MB，在所有java提交中击败了61.24%的用户。

## 官方解题

&emsp;官方思路类似，实现略有不同，将奇偶结点分别拼接，最后再将两个链表拼接。