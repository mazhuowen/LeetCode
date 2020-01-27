[toc]

Given a singly linked list, determine if it is a palindrome.



**Follow up:**
Could you do it in $O(n)$ time and $O(1)$ space?



## 题目解读

&emsp;给定链表，判断是否是回文。规定时间复杂度和空间复杂度。数据结构为单链表。入参为链表头。

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
    public boolean isPalindrome(ListNode head) {
        
    }
}
```

## 程序设计

* 对于回文的判断，结合链表特性首先想到的是利用快慢指针找到中点，然后翻转后半部分，从中点开始遍历，若有不相等的结点，则不是回文。奇数个结点，只需分别从首尾到中心结点，偶数个结点可以新增哑结点，分别从首尾到哑结点遍历。

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null) {
            return true;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode slow = dummy, fast = dummy;
        // 查找中间结点，加了哑结点后，奇数结点fast落在null上，偶数结点fast落在尾结点上
        // 加哑结点的作用是使得slow落在中间结点或中间结点的前一个上
        while(fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        // 后半段链表翻转，和原链表遍历对比（由于奇数结点下，前半段比后半段长，以后半段为准遍历）
        ListNode reverse = reverse(slow.next);
        while(reverse != null ) {
            // 不一样，说明不是回文
            if(head.val != reverse.val) {
                return false;
            }
            reverse = reverse.next;
            head = head.next;
        }
        return true;
    }

    // 链表翻转
    private ListNode reverse(ListNode head){
        if(head == null || head.next == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode traversal = dummy.next, temp;
        while(traversal.next != null) {
            temp = traversal.next;
            traversal.next = temp.next;
            temp.next = dummy.next;
            dummy.next = temp;
        }
        return dummy.next;
    }
}
```

测试样例：$1 \to 2$返回`false`，$1 \to 2 \to 2 \to 1$返回`true`。

## 性能分析

&emsp;时间复杂度，首先快慢指针遍历花费$O(N)$，翻转花费$O(N/2)$，遍历花费$O(N/2)$，总的时间复杂度还是$O(N)$。空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.62%的用户。

内存消耗：41.1MB，在所有java提交中击败了50.65%的用户。

## 官方解题

&emsp;官方提供了使用数组或堆栈的方法、递归的方法。需与面试官沟通，若是不能改变链表结构，则引入栈或数组、递归的方法较好；若是可以改变结构，则上述思路较好。

```java
class Solution {
	// 引入额外的全局变量保存
    private ListNode frontPointer;

    // 递归方法
    private boolean recursivelyCheck(ListNode currentNode) {
        if (currentNode != null) {
            // 递归从最后的结点依次返回
            if (!recursivelyCheck(currentNode.next)) return false;
            // 经过递归后，currentNode指向的是前半段结点对应的后半段结点
            if (currentNode.val != frontPointer.val) return false;
            // currentNode向前走，frontPointer向后走
            frontPointer = frontPointer.next;
        }
        return true;
    }

    public boolean isPalindrome(ListNode head) {
        frontPointer = head;
        return recursivelyCheck(head);
    }
}
```

> 除了上述方法，社区中最快的方法不通过翻转链表，而是慢指针指向当前第n个元素，快指针遍历到倒数第n个元素，每次比较后慢指针迭代，则从慢指针位置出发循环这个过程，虽然运行时间比较短，但时间复杂度是$O(N^2)$的，不满足要求。