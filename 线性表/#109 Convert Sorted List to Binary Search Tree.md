[toc]

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.



## 题目解读

&emsp;给定单链表，转化为平衡二叉查找树。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        
    }
}
```

## 程序设计

* 由于给定的链表是中序序列，可以将链表的中点作为根，两部分链表分别作为左右子树递归。

```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null) {
            return null;
        }
        // 哑结点方便快慢指针计数
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode fast = dummy;
        ListNode slow = dummy;
        // 记录慢指针的前驱
        ListNode pre = dummy;
        while(fast != null && fast.next != null) {
            fast = fast.next.next;
            pre = slow;
            slow = slow.next;
        }
        // 断开连接
        pre.next = null;
        TreeNode root = new TreeNode(slow.val);
        // 递归
        root.left = sortedListToBST(dummy.next);
        root.right = sortedListToBST(slow.next);
        return root;
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N\log_2N)$，空间复杂度为$O(\log_2N)$。

执行用时：1ms，在所有java提交中击败了99.75%的用户。

内存消耗：41.1MB，在所有java提交中击败了5.19%的用户。

> 第一次寻找中间点需要遍历$N$次，第二次分为两个链表，每个遍历$\frac{N}{2}$次……，由于是二分，总共分$\log_2N$次，总的时间复杂度为$N + 2 * \frac{N}{2} + \dots = \sum_{i=1}^{\log_2N}N = N\log_2N$，即时间复杂度为$O(N\log_2N)$。

## 官方解题

&emsp;官方除了递归思路，还提供了将链表转换为数组，利用数组的性质重组为二叉查找树。

```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null) {
            return null;
        }
        List<Integer> nums = new ArrayList<>();
        while(head != null) {
            nums.add(head.val);
            head = head.next;
        }
        return sortedListToBST(nums, 0, nums.size() - 1);
    }

    private TreeNode sortedListToBST(List<Integer> nums, int start, int end) {
        if(start > end) {
            return null;
        }
        // 中间值为头结点
        int mid = start + (end - start) / 2;
        TreeNode root = new TreeNode(nums.get(mid));
        root.left = sortedListToBST(nums, start, mid - 1);
        root.right = sortedListToBST(nums, mid + 1, end);
        return root;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了32.24%的用户。

内存消耗：41.2MB，在所有java提交中击败了5.19%的用户。

&emsp;除了上述利用中序序列性质创建树，官方还提供了模拟中序遍历的过程创建树的方法：

```java
class Solution {
    private ListNode traverse;

    public TreeNode sortedListToBST(ListNode head) {
        if(head == null) {
            return null;
        }
        traverse = head;
        int len = 0;
        while(head != null) {
            head = head.next;
            len++;
        }
        return sortedListToBST(0, len - 1);
    }
    // start、end起到计数的作用，关键在于当前中序遍历为traverse结点这一性质
    private TreeNode sortedListToBST(int start, int end) {
        if(start > end) {
            return null;
        }
        int mid = start + (end - start) / 2;
        TreeNode left = sortedListToBST(start, mid - 1);
        TreeNode root = new TreeNode(traverse.val);
        root.left = left;
        traverse = traverse.next;
        root.right = sortedListToBST(mid + 1, end);
        return root;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$。

执行用时：1ms，在所有java提交中击败了99.75%的用户。

内存消耗：41.8MB，在所有java提交中击败了5.19%的用户。