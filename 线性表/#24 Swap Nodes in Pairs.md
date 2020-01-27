[toc]

Given a linked list, swap every two adjacent nodes and return its head.

You may **not** modify the values in the list's nodes, only nodes itself may be changed.



## 题目解读

&emsp;遍历的过程中交换相邻两个结点，即交换当前两个结点，然后交换第三个结点及其相邻结点。若最后剩余一个结点，则不做操作。数据结构为单链表，参数可以是空表。

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
    public ListNode swapPairs(ListNode head) {
        
    }
}
```

## 程序设计

* 交换两个结点，首先链表头结点会发生变化，需要额外记录；更一般的，可以添加哑结消除这种特殊性，只需记录哑结点。
* 交换过程中，实质是对后一个结点的删除及插入到前一个结点的过程，需要两个变量来记录这两个结点。

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        // 哑结点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // 交换结点的前驱
        ListNode tmpNode = dummy;
        // 记录交换结点，分别为前后结点
        ListNode aNode,bNode;
        // 遍历交换，对于存在邻居的结点进行交换
        // 遍历条件同时满足空表、奇数结点特殊情况的处理
        while((aNode = tmpNode.next) != null && (bNode = tmpNode.next.next) != null){
            // 删除后面的结点b
            aNode.next = bNode.next;
            // 将结点b插入到tmp结点和a结点间，完成交换
            bNode.next = aNode;
            tmpNode.next = bNode;
            // tmp结点向前遍历两个，到达下次需要交换的结点对的前面
            tmpNode = tmpNode.next.next;
        }
        return dummy.next;
    }
}
```

测试样例：$1 \to 2 \to 3 \to 4$交换为$2 \to 1 \to 4 \to 3$测试普通情况；奇数个结点测试最后一个结点的特殊情况；空表测试特殊情况。

## 性能分析

&emsp;时间复杂度就是遍历链表的时间$O(n)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：34.5MB, 在所有java提交中击败了35.15%的用户。

## 官方解题

&emsp;除了上述最优解，官方还提供了递归的解题思路。从链表头遍历到链表尾，然后从尾到头依次交换。

```java
class Solution {
    public ListNode swapPairs(ListNode head) {

        // If the list has no node or has only one node left.
        if ((head == null) || (head.next == null)) {
            return head;
        }

        // Nodes to be swapped
        ListNode firstNode = head;
        ListNode secondNode = head.next;

        // Swapping
        firstNode.next  = swapPairs(secondNode.next);
        secondNode.next = firstNode;

        // Now the head is the second node
        return secondNode;
    }
}
```

&emsp;时间复杂度任然是$O(n)$，由于递归过程传入参数使用堆栈空间，空间复杂度为$O(n)$。