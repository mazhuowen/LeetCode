[toc]

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

Note:

Only constant extra memory is allowed.
You may not alter the values in the list's nodes, only nodes itself may be changed.



## 题目解读

&emsp;遍历链表，以$k$个结点为一组进行翻转，最后剩余的不满足$k$个的结点保持原有顺序。其中规定$k \le L$，算法要求只能使用常数级别的空间。数据结构为单链表，传入表头结点和$k$。

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
    public ListNode reverseKGroup(ListNode head, int k) {
    }
}
```

## 程序设计

* 分析题目可知算法分解为两个模块：根据$k$划分链表和翻转链表。最简单的先遍历链表得到表长，根据表长和$k$得到翻转的轮数，然后利用喜欢进行翻转拼接。这种方法遍历了两遍链表。如果要在一次遍历中进行翻转并拼接到链表中，需要两个指针记录翻转的链表需要拼接的前驱结点和后继结点，及翻转链表的表头结点和表尾结点。
* 划分模块如果通过pre及next指针实现，next每次要先于pre遍历$k+1$步；考虑到遍历过程中链表可能没有足够的结点数，或者第$k + 1$个结点是null等特殊情况，可以使用pre及end指针代替实现，end指针先走$k$步，如果end指针是null表示链表不够$k$个结点，直接返回，否则next指针就是end指针后继结点。
* 翻转模块新建一个null结点作为翻转链表的头结点，遍历原链表的时候采用头插法插入到新的链表，并更新投头结点，最后返回头结点。

<img src="../images/#25.png" style="zoom: 80%;" />

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if(head == null || head.next == null || k <= 1) {
            return head;
        }
        // 哑结点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // 需要翻转的链表前后指针
        ListNode pre = dummy, next;
        // 翻转链表的头结点、尾结点
        ListNode start, end=dummy;
        
        while(true){
            // 翻转链表遍历到表尾
            for(int i = 0; i < k && end != null; i++){
                end = end.next;
            }
            // 翻转链表不满足k个，直接返回
            if(end == null){
                break;
            }
            start = pre.next;
            next = end.next;
            end.next = null;
            // 翻转链表
            pre.next = reverse(start); 
            // 拼接
            start.next = next;
            // 得到下次划分的位置
            pre = end = start;
        }
        
        return dummy.next;
    }

    // 翻转链接函数
    private ListNode reverse(ListNode start){
        ListNode headNode = null, traversalNode = start, tempNode;
        while(traversalNode != null){
            // 临时结点保存后继结点
            tempNode = traversalNode.next;
            // 头插法并更新头结点
            traversalNode.next = headNode;
            headNode = traversalNode;
            // 更新遍历结点
            traversalNode = tempNode;
        }
        return headNode;
    }
}
```

测试样例：给定$1 \to 2 \to 3 \to 4 \to 5$，$k=2$，则结果为$2 \to 1 \to 4 \to 3 \to 5$；$k=3$则结果为$3 \to 2 \to 1 \to 4 \to 5$。

## 性能分析

&emsp;对于每次遍历花费$O(k)$，翻转花费$O(k)$，总共遍历$\frac{N}{k}$次，总的时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了77.77%的用户。

内存消耗：37.1MB，在所有java提交中击败了65.07%的用户。

## 官方解题

&emsp;暂无官方解，社区最优算法如下：

```java
class Solution {
    public ListNode reverseKGroup(ListNode start, int k) {
        if(start == null || start.next == null || k <= 1) {
            return start;
        }
        int counter;
        ListNode end = start;
        // 遍历
        for(counter = k - 1; counter > 0 && end != null; counter--) {
                end = end.next;
        }
        // 不满足k个元素，直接返回
        if(counter != 0 || end == null){
            return start;
        }
        ListNode next = end.next;
        // 翻转
        ListNode newHead = reverse(start, next);
        // 递归并拼接，迭代起始结点为next
        start.next = reverseKGroup(next, k);
        // 返回新的结点
        return newHead;
    }

     private ListNode reverse(ListNode start, ListNode next){  
         ListNode head = null;
         ListNode tempNode;
         while(start != next){
             tempNode = start.next;
             // 头插法
             start.next = head;
             // 更新头结点
             head = start;
			
             start = tempNode;
         }

         return head;
     }
}
```

&emsp;注意使用递归空间复杂度不再是常量级别。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.7MB，在所有java提交中击败了21.95%的用户。