[toc]

Sort a linked list using insertion sort.



## 题目解读

&emsp;使用链表模拟插入排序。给定无序链表，输出有序链表。数据结构为单链表。

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
    public ListNode insertionSortList(ListNode head) {
        
    }
}
```

## 程序设计

* 根据原始插入排序，首先定义`head`为初始有序链表，尾结点为`pre`，则下一步就是删除`pre`的后继结点，并从`head`开始遍历，最远到达`pre`，找到插入位置进行插入。循环上述过程直到`pre`后继为`null`指针。
* 鉴于插入可能发生在表首，为了一般化，引入哑结点。

```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        // 考虑到插入头结点前的情况，引入哑结点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // 前面已排序链表表尾结点
        ListNode pre = dummy.next, temp, traversal;
        while(pre.next != null) {
            // 删除待排序结点
            temp = pre.next;
            pre.next = temp.next;
            // 插入前面的有序链表
            traversal = dummy;
            // 遍历到插入位置
            while(traversal != pre && traversal.next.val < temp.val){
                traversal = traversal.next;
            }
            // 待插入结点插入到链表尾，并更新pre
            if(traversal == pre && traversal.val < temp.val){
                temp.next = pre.next;
                pre.next = temp;
                pre = temp;
            } 
            // 插入到链表中间
            else {
                temp.next = traversal.next;
                traversal.next = temp;
            }
        }
        return dummy.next;
    }
}
```

测试样例：$4 \to 2 \to 1 \to 3$输出$1 \to 2 \to 3 \to 4$测试普通情况；相同元素链表测试插入头稳定性；顺序链表测试尾插入稳定性。

## 性能分析

&emsp;空间复杂度为$O(1)$；时间复杂度，最好的情况为同元素链表和倒序链表，每次插入到头结点，此时主要花销为插入花销，每次遍历花费$O(1)$，而总共遍历$N$次，故时间复杂度最好为$O(N)$；最坏的情况是顺序链表，假设有序链表长$l$，每次都要遍历完，而总共有$N$次，总的时间复杂度为$\sum_{l=1}^{N-1}l = \frac{N(N-1)}{2}$，即为$O(N^2)$。

## 官方解题

&emsp;暂无，密切关注。

## 代码改进

&emsp;上述代码严格遵守插入排序，即使待插入结点值大于已排序链表最大值，仍然需要遍历插入，该操作可以代替为`pre`后移一位。改进如下：

```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        // 考虑到插入头结点前的情况，引入哑结点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // 前面已排序链表表尾结点
        ListNode pre = dummy.next, temp, traversal;
        while(pre.next != null) {
            // 删除待排序结点
            temp = pre.next;
            // 待插入值大于所有有序链表的值，pre直接后移
            if(temp.val >= pre.val){
                pre = temp;
            } 
            // 原流程
            else {
                // 删除待排序点
                pre.next = temp.next;
                // 插入前面的有序链表
                traversal = dummy;
                // 遍历到插入位置
                while(traversal != pre && traversal.next.val < temp.val){
                    traversal = traversal.next;
                }
                temp.next = traversal.next;
                traversal.next = temp;
            }
        }
        return dummy.next;
    }
}
```

经过优化，最好情况同元素、有序链表、倒序链表时间复杂度都为$O(N)$，最坏情况为头结点为最大值，后续结点为有序值，每次都要遍历插入到倒数第二位，时间复杂度仍然为$O(N^2)$。

执行用时：4ms，在所有java提交中击败了96.71%的用户。

内存消耗：37.8MB，在所有java提交中击败了48.45%的用户。