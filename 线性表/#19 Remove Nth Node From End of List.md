[toc]

Given a linked list, remove the n-th node from the end of list and return its head.

**Note:**

Given *n* will always be valid.



## 题目解读

&emsp;给定数据结构如下，是单链表；方法传入一个链表头和一个整数。题目需要移除倒数第$n$个结点，假设$n$是有效数字，即小于等于表长。常规的，这是一个搜索与删除的组合，即找到第$n$个结点的直接前驱，然后将直接前驱的后继改为待删结点的后继，完成删除。

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        
     }
}
```

## 程序设计

* 由于需要删除倒数的结点，最简单的，遍历链表得到表长，减去$n$得到遍历到指定结点的次数。但这种方法需要遍历两次链表；拓展的，可以想到使用递归得到倒数第$n$个结点，但是递归实现逻辑复杂，而且需要额外的全局变量计数，考虑到特殊情况如删除表首的元素，逻辑更加复杂；而栈的方式需要额外的空间；更一般的使用双指针，第一个指针先与第二个指针$n+1$步，当第一个指针遍历到null时，第二个指针正好是待删除结点的直接前驱。
* 采用双指针方法，考虑到特殊情况，比如表长$L$，删除倒数第$L$个结点也就是表首结点的情况，由于表首没有直接前驱，这无疑增加了难度；可通过增加哑结点的方式解决，哑结点后继为表首结点，这无疑将特殊情况一般化，大大简化算法。

```java
class Solution {

    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 哑结点
        ListNode tmpNode = new ListNode(0);
        tmpNode.next = head;
        // 第一个指针
        ListNode first = tmpNode; 
        // 第二个指针
        ListNode second = tmpNode; 
        // 第一个指针先走n+1步
        for(int i = 0; i < n + 1; i++){
            first = first.next;
        }
        // 两个指针同时遍历，直到第一个指针遍历到null，此时第二个指针是待删除结点的前驱
        while(first != null){
            first = first.next;
            second = second.next;
        }
        // 执行删除操作，由于有哑结点，无需特殊情况考虑
        second.next = second.next.next;
        return tmpNode.next;
    }
}
```

&emsp;测试样例：$1 \to 2 \to 3 \to 4 \to 5$删除倒数第二个结点为$1 \to 2 \to 3 \to 5$测试普通功能；$1 \to 2$删除倒数第二个结点为$2$，删除倒数第一个为$1$，测试特殊情况。

## 性能分析

&emsp;时间复杂度上，由于遍历整个链表，是线性的。空间复杂度上，由于创建了三个指针结点，整体上看是常量级别。

执行时间：执行时间1ms，在所有java提交中击败了59.23%的用户。

内存消耗：34.9MB，在所有的java提交中击败了36.60%的用户。

## 官方解题

&emsp;上述思路参考官方解题。
