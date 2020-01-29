[toc]

Given a non-negative integer represented as **non-empty** a singly linked list of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.



## 题目解读

&emsp;将非负整数表示为单链表的形式，高位在表头，低位在表尾，需实现加一的操作。题目规定除了`0`，高位不会有`0`。数据结构为单链表，入参为链表头。

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
    public ListNode plusOne(ListNode head) {
        
    }
}
```

## 程序设计

* 考虑到进位，实质就是从链表尾遍历操作到链表头，自然而然想到递归算法。需注意最后若有进位产生，需要插入新的头结点`1`。

```java
class Solution {
    public ListNode plusOne(ListNode head) {
        // 为空加一则新建结点
        if(head == null) {
            return new ListNode(1);
        }
        int result = plusNode(head);
        // 表示最高位有进位，头插法并更新头结点
        if(result == 1) {
            ListNode newHead = new ListNode(1);
            newHead.next = head;
            head = newHead;
        }
        return head;
    }

    // 递归
    private int plusNode(ListNode head) {
        int sum;
        // 递归终止条件，尾结点加一
        if(head.next == null) {
            sum = head.val + 1;
        } else {
            sum = head.val + plusNode(head.next);
        }
        head.val = sum % 10;
        // 返回进位，即上一位需要加的值
        return sum / 10;
    }
}
```

测试样例：空链表、`0`链表、全部元素是`9`的链表测试进位、普通链表。

## 性能分析

&emsp;时间复杂度$O(N)$，由于递归不是尾调用会使用到额外空间，每次递归都会保存局部变量`head`，空间复杂度$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：34.8MB，在所有java提交中击败了8.80%的用户。

## 官方解题

&emsp;暂无，密切关注。社区中提到了翻转链表，再做遍历加法，时间复杂度一致，空间复杂度为$O(1)$，但这种方法需要翻转完再翻转回来；社区还有一种解法，使用双指针，我们观察题目会发现，加一操作发生进位时其操作值只能是9，如果链表尾结点值不是9，则只需要尾结点加一即可；如果链表结尾有连续的9的子链表，则只需将前驱加一，子链表全部置为0。

```java
class Solution {
    public ListNode plusOne(ListNode head) {
        ListNode first = head, second = null;
        // first遍历链表，遇到不是9的，就定位为second
        // 遍历结束，first为null，second指向最后一个非9的结点，其后都为9
        while(first != null) {
            if(first.val != 9) {
                second = first;
            }
            first = first.next;
        }
        // 如果second不是空，非9位置加一
        if(second != null) {
            second.val += 1;
        } else {
            // second为空，表示整个链表都是9，需要进位结点，插入表头
            second = new ListNode(1);
            second.next = head;
            head = second;
        }
        // second后都为置为9的结点（如果没有9，则second就是尾结点）
        first = second.next;
        // 置为0，如果尾结点没有9，则不走这个循环
        while(first != null) {
            first.val = 0;
            first = first.next;
        }
        return head;
    }
}
```

&emsp;时间复杂度$O(N)$，空间复杂度$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：34.3MB，在所有java提交中击败了88.00%的用户。