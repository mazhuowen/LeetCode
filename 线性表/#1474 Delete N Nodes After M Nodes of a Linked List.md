[toc]

Given the `head` of a linked list and two integers $m$ and $n$. Traverse the linked list and remove some nodes in the following way:

* Start with the head as the current node.
* Keep the first $m$ nodes starting with the current node.
* Remove the next $n$ nodes
* Keep repeating steps $2$ and $3$ until you reach the end of the list.

Return the head of the modified list after removing the mentioned nodes.

**Follow up question**: How can you solve this problem by modifying the list in-place?



**Constraints**:

* The given linked list will contain between $1$ and $10^4$ nodes.
* The value of each node in the linked list will be in the range $[1, 10^6]$.
* $1 \le m,n \le 1000$



## 题目解读

&emsp;每隔$m$个节点删除$n$个节点。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteNodes(ListNode head, int m, int n) {
        
    }
}
```

## 程序设计

* 常规遍历删除。

```java
class Solution {
    public ListNode deleteNodes(ListNode head, int m, int n) {
        if (m < 1 || n < 1) throw new IllegalArgumentException("invalid param");

        ListNode dummy = new ListNode(-1, head);
        ListNode pre = dummy;
        while (pre != null) {
            // 跳过m个节点
            int count = 0;
            while (pre != null && count < m) {
                count++;
                pre = pre.next;
            }
            // 剩余节点不够m个，或正好m个，保留
            if (count < m || pre == null) break;

            // 删除n个
            ListNode next = pre;
            count = 0;
            while (next != null && count < n) {
                count++;
                next = next.next;
            }
            if (next != null) pre.next = next.next;
            else pre.next = null;
            pre = next;
        }

        return dummy.next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了90.60%的用户。

内存消耗：39.3MB，在所有java提交中击败了9.80%的用户。

## 官方解题

&emsp;暂无，密切关注。