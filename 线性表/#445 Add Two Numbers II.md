[toc]

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Follow up:
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.



## 题目解读

&emsp;给定两个链表代表两个整数，高位在表头，低位在表尾，计算两个整数之和并返回新的链表。

## 程序设计

* 由于高位在前，首先想到的是翻转链表使得低位在前，这样可以遍历计算，计算得到的结果也需要翻转。

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        l1 = reverseList(l1);
        l2 = reverseList(l2);
        int sum = 0;
        ListNode dummy = new ListNode(-1);
        ListNode traverse = dummy;
        // 遍历直到两个链表都为空，且最高位最位为0
        while(l1 != null || l2 != null || sum / 10 > 0) {
            sum = sum / 10;
            if(l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }
            if(l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }
            // 新的结点
            traverse.next = new ListNode(sum % 10);
            traverse = traverse.next;
        }
        return reverseList(dummy.next);
    }
	// 翻转链表
    private ListNode reverseList(ListNode l) {
        if(l == null) {
            return null;
        }
        ListNode dummy = new ListNode(-1);
        dummy.next = l;
        while(l.next != null) {
            // 删除l的后继
            ListNode temp = l.next;
            l.next = temp.next;
            // 插入到dummy后
            temp.next = dummy.next;
            dummy.next = temp;
        }
        return dummy.next;
    }
}
```

* 如果不翻转链表，则利用栈的先进后出特性从栈中遍历，后续流程一致。

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Stack<ListNode> stack1 = new Stack<>();
        Stack<ListNode> stack2 = new Stack<>();
        while(l1 != null) {
            stack1.push(l1);
            l1 = l1.next;
        }
        while(l2 != null) {
            stack2.push(l2);
            l2 = l2.next;
        }
        int sum = 0;
        ListNode dummy = new ListNode(-1);
        while(!stack1.isEmpty() || !stack2.isEmpty() || sum / 10 > 0) {
            sum = sum / 10;
            if(!stack1.isEmpty()) {
                sum += stack1.pop().val;
            }
            if(!stack2.isEmpty()) {
                sum += stack2.pop().val;
            }
            // 头插法
            ListNode temp = new ListNode(sum % 10);
            temp.next = dummy.next;
            dummy.next = temp;
        }
        return dummy.next;
    }
}
```

## 性能分析

&emsp;翻转方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了84.72%的用户。

内存消耗：50.6MB，在所有java提交中击败了5.08%的用户。

&emsp;栈方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了58.74%的用户。

内存消耗：48.8MB，在所有java提交中击败了5.08%的用户。

## 官方解题

&emsp;暂无，密切关注。