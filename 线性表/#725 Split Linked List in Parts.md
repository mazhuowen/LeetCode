[toc]

Given a (singly) linked list with head node `root`, write a function to split the linked list into `k` consecutive linked list "parts".

The length of each part should be as equal as possible: no two parts should have a size differing by more than 1. This may lead to some parts being null.

The parts should be in order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal parts occurring later.

Return a List of ListNode's representing the linked list parts that are formed.

Examples 1->2->3->4, k = 5 // 5 equal parts [ [1], [2], [3], [4], null ]

Note:

* The length of root will be in the range [0, 1000].
* Each value of a node in the input will be an integer in the range [0, 999].
* k will be an integer in the range [1, 50].



## 题目解读

&emsp;给定链表，将链表分成$k$段，每一段和前一段的差距不能超过1。

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
    public ListNode[] splitListToParts(ListNode root, int k) {

    }
}
```

## 程序设计

* 需要仔细审题，是分成$k$段，而不是每段$k$个。这样肯定先需要计数链表的长度，在得到长度后可以除以$k$初步得到每段至少要有的结点数$num$，而余数$rem$则表示不能均匀分配给$k$段链表的，根据题目，前后链表长度不能超过1，可以将余数分配到前面的$rem$个链表。
* 引入哑结点方便划分。

```java
class Solution {
    public ListNode[] splitListToParts(ListNode root, int k) {
        int len = 0;
        //引入哑结点方便分段
        ListNode dummy = new ListNode(-1);
        dummy.next = root;
        // 统计长度
        while(root != null) {
            len++;
            root = root.next;
        }
        // 拆分后每条的数目
        int num = len / k;
        // 余下的数目（肯定小于k，可以均摊到前面的rem条，这样后面的差距不会大于1）
        int rem = len % k;

        ListNode[] res = new ListNode[k];
        // 遍历
        for(int i = 0; i < k; i++) {
            ListNode traverse = dummy;
            int count = num;
            // 分配count数
            while(count > 0) {
                traverse = traverse.next;
                count--;
            }
            // 分配多余结点（只分配一个）
            if(rem > 0) {
                traverse = traverse.next;
                rem--;
            }
            // 赋值，并将dummy指向下一个遍历点
            res[i] = dummy.next;
            dummy.next = traverse.next;
            // 断开链接
            traverse.next = null;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(k)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.3MB，在所有java提交中击败了5.12%的用户。

## 官方解题

&emsp;同上。