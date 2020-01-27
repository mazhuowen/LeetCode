[toc]

Given a singly linked list $L$: $L_0 \to L_1 \to \dots \to L_{n-1} \to L_n$,
reorder it to: $L_0 \to L_n \to L_1 \to L_{n-1} \to L_2 \to L_{n-2} \to \dots$

You may not modify the values in the list's nodes, only nodes itself may be changed.



## 题目解读

&emsp;重新排列给定链表，数据结构是单链表，入参为链表头。

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
    public void reorderList(ListNode head) {
        
    }
}
```

## 程序设计

* 链表的重排列可分为三部分内容：找到链表中间结点；将后半部分链表翻转；将两个链表交叉拼接得到最终链表。
* 找中间结点可以使用快慢指针。翻转时需注意循环条件判断，特殊情况如两个结点时，此时中间结点就是尾结点，不需要翻转；三个结点时，中间结点后只有一个结点，这一个结点无需翻转。
* 注意到函数返回为`void`，即不能随意修改`head`引用。

```java
class Solution {
    public void reorderList(ListNode head) {
        if(head == null || head.next == null){
            return;
        }
        // 偶数长，tail遍历到null，奇数长，tail遍历到尾结点
        // 不管怎样，mid遍历到的结点是待翻转链表的前驱结点
        ListNode tail = head, mid = head;
        while(tail != null && tail.next != null) {
            tail = tail.next.next;
            mid = mid.next;
        }

        // 将mid后的结点翻转
        ListNode start = mid.next, temp;
        // 注意特殊情况，及mid指向的结点是尾界定啊：比如只有两个结点，此时start为空；
        // mid指向的结点是倒数第二个结点：比如三个结点的情况，后面只有一个结点不需要翻转
        while(start != null && start.next != null) {
            // 删除traversal的后继结点
            temp = start.next;
            start.next = temp.next;
            // 插入到mid后面
            temp.next = mid.next;
            mid.next = temp;
        }

        // 后半部分链表
        start = mid.next;
        // 截断后面翻转的链表
        mid.next = null;
        // 前半部分链表
        mid = head;
        // 前部分链表表长一定大于等于后半部分链表，以后半部分为主，插入拼接
        while(start != null) {
            temp = start.next;
            // 将反序结点插入链表
            start.next = mid.next;
            mid.next = start;

            // 继续遍历
            start = temp;
            mid = mid.next.next;
        }
    }
}
```

测试样例：两个结点、三个结点测试特殊情况；奇数长、偶数长链表测试普通功能。

## 性能分析

&emsp;总体而言由三个遍历组成，时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.3MB, 在所有java提交中击败了53.71%的用户。

## 官方解题

&emsp;暂无，持续关注。