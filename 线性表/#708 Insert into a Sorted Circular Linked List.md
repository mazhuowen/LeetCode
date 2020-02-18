[toc]

Given a node from a **Circular Linked List** which is sorted in ascending order, write a function to insert a value insertVal into the list such that it remains a sorted circular list. The given node can be a reference to any single node in the list, and may not be necessarily the smallest value in the circular list.

If there are multiple suitable places for insertion, you may choose any place to insert the new value. After the insertion, the circular list should remain sorted.

If the list is empty (i.e., given node is null), you should create a new single circular list and return the reference to that single node. Otherwise, you should return the original given node.



Constraints:

* $0 \le \text{Number of Nodes} \le 5 * 10^4$
* $-10^6 \le \text{Node.val} \le 10^6$
* $-10^6 \le \text{insertVal} \le 10^6$



## 题目解读

&emsp;给定循环有序链表，插入值仍然保持链表有序性。需注意链表头不一定是最小值。为空，则需要新建链表

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node next;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _next) {
        val = _val;
        next = _next;
    }
};
*/
class Solution {
    public Node insert(Node head, int insertVal) {
        
    }
}
```

## 程序设计

* 主要难点在于链表头不一定是最小值或最大值，需要遍历整个链表判断。对于大于等于表头的插入值，只需遍历遇到大于插入值的值（如果存在大于插入值的结点）或遍历到升序末端（不存在大于插入值的结点）并且要判断不是头结点，避免造成死循环。
* 对于小于表头的插入值，首先遍历找到临界点，然后同插入最大值的操作。

```java
class Solution {
    public Node insert(Node head, int insertVal) {
        // 新建结点
        if(head == null) {
            head = new Node(insertVal);
            head.next = head;
            return head;
        }
        Node traverse = head;
        // 值在head后半段
        if(insertVal >= head.val) {
            // 遍历查找插入点
            while(traverse.next.val <= insertVal && traverse.next != head && traverse.next.val >= head.val) {
                traverse = traverse.next;
            }
        } 
        // 在head前半段
        else {
            // 遍历到临界点
            while(traverse.next.val >= head.val && traverse.next != head) {
                traverse = traverse.next;
            }
            // 遍历到插入点
            while(traverse.next.val <= insertVal && traverse.next != head) {
                traverse = traverse.next;
            }
        }
        // 插入值
        Node insert = new Node(insertVal);
        insert.next = traverse.next;
        traverse.next = insert;
        return head;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.3MB，在所有java提交中击败了6.17%的用户。

## 官方解题

&emsp;思路与上述方法类似，但使用了双指针来保存前后结点。