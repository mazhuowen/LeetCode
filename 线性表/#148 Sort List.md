[toc]

Sort a linked list in $O(n\log n)$ time using constant space complexity.



## 题目解读

&emsp;题目要求对给定的链表进行排序，对时间复杂度和空间复杂度都有要求。对于对数级别的排序，想到的是快排、归并排序及堆排序；空间复杂度为常量级则不能引入优先级队列及递归等。联系到链表，算法必定要用到类似归并排序的分治法。数据结构为单链表。

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
    public ListNode sortList(ListNode head) {
        
    }
}
```

## 程序设计

* 对于归并排序，若采用自顶向下的递归算法，则空间复杂度不满足要求，必须采用自底向上的归并算法。首先一个一个结点比较，将相邻的结点归并形成长度为二的链表；然后归并长度为二的链表为长度为四的链表，以此类推直到归并完整个链表。
* 在合并相邻的两个链表时，我们需要这两个链表的前驱结点、后继结点，方便合并后重新连入整体链表。更细节的，我们需要第一个链表的前驱`pre`、尾结点`tail1`（同时也是第二个链表的前驱结点），第二个链表的尾结点`tail2`，可根据这个结点得到后继结点`next`。
* 合并链表时存在第二个链表不存在、第二个链表长度小于第一个链表等特殊情况。

```java
class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        // 哑结点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // 表长
        int len = 0;
        ListNode temp = dummy;
        // 计算表长
        while(temp.next != null) {
            temp = temp.next;
            len++;
        }
        // 每一轮归并的结点数
        int epoch = 1;
        // 待归并链表的前驱、后继及各自的尾结点
        ListNode pre, next, tail1, tail2;
        // 归并轮次
        while(epoch < len) {
            pre = tail1 = tail2 = dummy;
            // 每一轮归并，合并链表上相邻的链表
            while(pre.next != null){
                // 定位第一个链表的尾结点
                for(int i = 0; i < epoch && tail1 != null; i++) {
                    tail1 = tail1.next;
                }
                // 表明最后剩余的一段链表结点数小于等于epoch，不存在第二段链表
                // 这种情况对应整体链表最后剩余链表，无合并对象，结束本轮归并，进入下一轮
                if(tail1 == null || tail1.next == null) {
                    break;
                }
                // 定位第二个链表的尾结点，第二个链表结点数在1～epoch之间
                tail2 = tail1;
                for(int i = 0; i < epoch && tail2.next != null; i++) {
                    tail2 = tail2.next;
                }
                next = tail2.next;
                // 合并链表，temp1、temp2从两个链表头开始遍历
                ListNode temp1 = pre.next, temp2 = tail1.next;
                // 断开了个链表，方便合并
                tail1.next = null;
                tail2.next = null;
                while(temp1 != null && temp2 != null) {
                    // 链表1结点小于等于链表2结点，由于链表1与pre相连，故不做操作，继续遍历
                    if(temp1.val <= temp2.val) {
                        pre = temp1;
                        temp1 = temp1.next;
                    } 
                    // 结点2插入结点1前
                    else {
                        // 保存结点2
                        temp = temp2;
                        // 第二个链表删除结点2
                        temp2 = temp2.next;
                        // 插入结点2
                        temp.next = temp1;
                        pre.next = temp;
                        // 更新结点1的前驱
                        pre = pre.next;
                    }
                }
                // 链表2已经插入完成，链表1有剩余，则将pre定位到链表1表尾，方便连接next
                if(temp1 != null) {
                    pre = tail1;
                }
                // 链表2有剩余，则链接链表2并定位到链表2表尾，方便连接next
                if(temp2 != null) {
                    pre.next = temp2;
                    pre = tail2;
                }
                // 连接后面的链表
                pre.next = next;
                // 继续遍历
                tail1 = tail2 = pre;
            }
            // 下一轮归并结点数
            epoch <<= 1;
        }
        return dummy.next;
    }
}
```

测试样例：$4 \to 2 \to 1 \to 3$输出$1 \to 2 \to 3 \to 4$。

## 性能分析

&emsp;每轮比较$N$次，共有$\log_2N$轮，时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了37.50%的用户。

内存消耗：41.1MB，在所有java提交中击败了7.82%的用户。

## 官方解题

&emsp;暂无，密切关注。社区一些方法固然性能更高，但使用了递归，不满足空间复杂度要求。