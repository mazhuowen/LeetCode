[toc]

Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.



## 题目解读

&emsp;给定无序链表及阈值，使得小于阈值的结点都在前面，大于阈值的结点都在后面。特别注意的是阈值前后的结点顺序不能变。数据结构为单链表，入参为链表头及阈值。

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
    public ListNode partition(ListNode head, int x) {
        
    }
}
```

## 程序设计

* 将问题转换为找到第一个大于等于阈值的点，作为分界点，其前驱结点就是小于阈值的链表的表尾；从阈值结点继续遍历，如果存在小于阈值的结点，则删除并插入到前面的链表，以此类推遍历到表尾。
* 由于要保存原来的结点顺序，将链表删除的结点通过尾插法加入到阈值前的链表。
* 需要考虑到特殊情况，比如阈值大于所有的结点，此时分界点是表尾结点，或小于所有的结点，此时分界点是表首结点。

```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        // 基本情况处理
        if(head == null || head.next == null){
            return head;
        }
        // 哑结点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // pre为分界点前驱，next为分界点，tempNode为遍历结点，insertNode保存删除结点
        ListNode pre = dummy, next, tempNode, insertNode;
        // 遍历搜索分界点
        while(pre.next != null){
            if(pre.next.val >= x){
                break;
            }
            pre = pre.next;
        }
        // 阈值大于所有结点的情况，直接返回
        // 阈值小于所有结点时，pre就是dummy，同普通情况处理
        if(pre.next == null) {
            return head;
        }
        // 设置分界点和遍历点
        next = tempNode = pre.next;
        // 从分界点向后遍历
        while(tempNode.next != null){
            // 不符合要求的结点
            if(tempNode.next.val < x){
                // 保存结点
                insertNode = tempNode.next;
                // 删除结点
                tempNode.next = tempNode.next.next;
                // 尾插法插入
                pre.next = insertNode;
                pre = insertNode;
                // 链接后面部分链表
                insertNode.next = next;
            } else {
                // 满足条件则继续遍历
                tempNode = tempNode.next;
            }
        }
        return dummy.next;
    }
}
```

测试样例：$1 \to 4 \to 3 \to 2 \to 5 \to 2$，阈值为3，则输出为$1 \to 2 \to 2 \to 4 \to 3 \to 5$测试普通情况；$1 \to 1$，阈值为2，测试阈值大于所有结点的情况；同理测试阈值小于所有结点的情况。

## 性能分析

&emsp;由于主体是链表遍历，时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了72.63%的用户。

内存消耗：35.9MB，在所有java提交中击败了35.31%的用户。

## 官方解题

&emsp;官方在遍历链表的过程中，原链表保存小于阈值的结点，新链表保存大于阈值的结点，最后合并这两个链表即可。

```java
class Solution {
    public ListNode partition(ListNode head, int x) {

        // before_head、after_head为两个哑结点，保存两个链表
        // before、after用于比遍历时将结点尾插法插入，即链表的尾结点
        ListNode before_head = new ListNode(0);
        ListNode before = before_head;
        ListNode after_head = new ListNode(0);
        ListNode after = after_head;

        // 遍历原链表
        while (head != null) {
            // 小于阈值，则将结点拼接到before
            if (head.val < x) {
                before.next = head;
                before = before.next;
            }
            // 大于等于阈值，则将结点拼接到after
            else {
                after.next = head;
                after = after.next;
            }
            // 继续向后遍历
            head = head.next;
        }
		// 特别注意after后可能还存在原链表小于阈值的结点，必须将after后继置空
        after.next = null;
        // 拼接得到最后的链表
        before.next = after_head.next;

        return before_head.next;
    }
}
```

时间复杂度、空间复杂度同上。

执行用时：1ms，在所有java提交中击败了72.63%的用户。

内存消耗：36.1MB，在所有java提交中击败了5.07%的用户。