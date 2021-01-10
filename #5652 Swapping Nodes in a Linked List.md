[toc]

You are given the head of a linked list, and an integer k.

Return the head of the linked list after swapping the values of the kth node from the beginning and the kth node from the end (the list is 1-indexed).

 

Example 1:


Input: head = [1,2,3,4,5], k = 2
Output: [1,4,3,2,5]
Example 2:

Input: head = [7,9,6,6,7,8,3,0,9,5], k = 5
Output: [7,9,6,6,8,7,3,0,9,5]
Example 3:

Input: head = [1], k = 1
Output: [1]
Example 4:

Input: head = [1,2], k = 1
Output: [2,1]
Example 5:

Input: head = [1,2,3], k = 2
Output: [1,2,3]


Constraints:

The number of nodes in the list is n.
1 <= k <= n <= 105
0 <= Node.val <= 100



## 题目解读

&emsp;

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
    public ListNode swapNodes(ListNode head, int k) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public ListNode swapNodes(ListNode head, int k) {
        if (head == null) return head;
        // 计算长度
        int len = len(head);
        if (k > len) throw new IllegalArgumentException("invalid param");

        int count = 0;
        int idx1 = k, idx2 = len - k + 1;
        ListNode tmp = head, replace = null;
        while (tmp != null) {
            if (++count == idx1 || count == idx2) {
                if (replace == null) replace = tmp;
                // 交换值
                else {
                    int val = replace.val;
                    replace.val = tmp.val;
                    tmp.val = val;
                    break;
                }
            }
            tmp = tmp.next;
        }
        return head;
    }
    
    private int len(ListNode head) {
        int len = 0;
        while (head != null) {
            len++;
            head = head.next;
        }
        return len;
    }
}
```



```java
class Solution {
    public ListNode swapNodes(ListNode head, int k) {
        ListNode tmp = head, first = head, second = head;
        for (int i = 1; tmp != null; i++) {
            if (i < k) first = first.next;
            if (i > k) second = second.next;
            tmp = tmp.next;
        }

        swap(first, second);
        return head;
    }

    private void swap(ListNode node1, ListNode node2) {
        int tmp = node1.val;
        node1.val = node2.val;
        node2.val = tmp;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。



## 官方解题

&emsp;

```java

```

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。
