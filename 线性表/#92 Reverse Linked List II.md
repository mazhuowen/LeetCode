[toc]

Reverse a linked list from position m to n. Do it in one-pass.

Note: $1 \le m \le n \le length$ of list.



## 题目解读

&emsp;翻转特定位置区间的链表，题目对参数$m$、$n$做了限制。数据结构为单链表，入参为链表头及$m$、$n$。

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
    public ListNode reverseBetween(ListNode head, int m, int n) {
    }
}
```

## 程序设计

* 由于题目已做参数限制，考虑到常规的链表的翻转需要记住待翻转链表的前驱和后继、首尾结点。最优算法简化为两个结点，前驱和首结点，翻转操作变为将首结点的后继结点删除，将该结点插入到前驱结点后面，这样执行完指定数目的结点也就完成了翻转。

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if(m == n){
            return head;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // 翻转链表的前驱结点，即第m-1个结点
        ListNode pre = dummy;
        n = n - m;
        // 遍历找到前驱结点，即第m-1个结点
        while(--m > 0){
            pre = pre.next;
        }
        // 删除start结点的后继，并头插法插入pre后面
        ListNode start = pre.next, tempNode;
        while(n-- > 0){
            // 删除start的后继
            tempNode = start.next;
            start.next = tempNode.next;
            // 头插法插入pre后面
            tempNode.next = pre.next;
            pre.next = tempNode;
        }
        return dummy.next;
    }   
}
```

测试样例：给定$1 \to 2 \to 3 \to 4 \to 5$，$m=2$，$n = 4$，输出$1 \to 4 \to 3 \to 2 \to 5$测试常规功能；给定$m=n$的情况、$n=1$或$m=L$的情况，测试边界特殊情况。

## 性能分析

&emsp;由于遍历一次链表，时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：34.6MB，在所有java提交中击败了5.71%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了一种递归的思路，链表翻转是很基础的操作，不推荐使用递归等。