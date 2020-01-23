[toc]

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## 题目解读

&emsp;给定数据结构如下，是单链表；方法传入两个链表头。题目需要实现数据的加法。从计算上来说需要注意进位的处理；从存储结构上来说，除了最高位发生进位需要新增结点外，可以复用两个链表中的某一条的结点。这道题目主要考察了链表的遍历和连接。仔细审题，两个链表非空且只有0最高位才会出现0，可知这两个链表都至少有一个结点；反序说明低位在链表前面，高位在链表后面，即链表遍历访问的过程就是从低到高的过程。

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        
     }
}
```

## 程序设计

* 由于大部分结点可复用，这里选择$l1$所在链表作为结果；存在三种情况：链表$l1$和$l2$等长，则$l1$的结点数足够了，除了高位可能发生的进位，不需要新增结点；$l1$比$l2$长，则在$l1$遍历并运算，同样除了高位可能发生的进位，不需要新增结点；$l1$比$l2$短，则遍历完$l1$及等长的$l2$部分后将$l2$剩余部分拼接到$l1$后面，继续遍历$l1$并更新，同样除了高位可能发生的进位，不需要新增结点。
* 每个结点运算可能会有进位产生，为了记录上个结点的运算结果，使用一个变量保存这个结果。每次结点的运算过程就是：上一次计算结果的除数加当前结点的值得到本次结点的计算结果；当前$l1$结点更新值为本次计算结果的余数。

> 由于两个个位数相加，考虑到进位可能出现的最大的和是9+9+1=19，进位不超过1，故此处可用取整表示进位，取余表示当前位的和。

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // 以l1为计算的模板，避免重新创建结点；同时sumResult记录链表头
        ListNode sumResult = l1;
        // 记录计算结果
        int sum = 0;
        // 遍历两个链表
        while(l1.next != null && l2.next != null){
            // 计算两个结点、进位的相加值
            sum = l1.val + l2.val + sum / 10;
            // 更新l1所在结点
            l1.val = sum % 10;
            // 遍历
            l1 = l1.next;
            l2 = l2.next;
        }
        
		// 运行到此处对应三种情况：
        // 1、l1剩余的链表只有l1一个结点，l2还有多个结点；
        // 2、l2剩余的链表只有l2一个结点，l1还有多个结点；
        // 3、l1、l2剩余的链表都只有一个结点，分别是l1、l2。
        // 这三种情况当前结点l1、l2都存在且没有计算，首先计算更新结果
        sum = l1.val + l2.val + sum / 10;
        l1.val = sum % 10;
		
        // 对应l1比l2短或相等的情况（相等的话拼接后还是null，不影响结果；l1长的话继续在下一步遍历l1），拼接链表
        if(l1.next == null)
            l1.next = l2.next;
        
        // 继续遍历l1直到表尾，每次计算更新进位，若进位是0，当前及后续结点无需更新
        while(sum / 10 != 0 && l1.next != null){
            l1 = l1.next;
            sum = l1.val + (sum / 10);
            l1.val = sum % 10;
        }
		// 遍历结束后需要考虑最高位进位的问题，若有则新增结点，进位为最高位
        if(sum / 10 != 0){
            l1.next = new ListNode(sum / 10);
        }

        return sumResult;
    }
}
```

&emsp;测试样例：$2 \to 4 \to 3$与$5 \to6 \to 4$相加得$7 \to 0 \to 8$测试普通功能；$5$与$5$相加得$0 \to 1$测试进位；不等长的链表测试链表拼接及运算功能。

## 性能分析

&emsp;时间复杂度上，整体是遍历两个链表，故时间复杂度是线性的，为两个链表中最长的值。空间复杂度上，由于复用结点，代码中新增的只是保存计算结果的变量sum、两数之和链表头sumResult及可能的最高位需要新增的结点，整体上看是常量级别。

执行时间：执行时间2ms，在所有java提交中击败了99.96%的用户。

内存消耗：44.1MB，在所有的java提交中击败了61.71%的用户。

## 官方解题

&emsp;遍历两个链表，计算结果并新建结点。思路大致类似，最大不同是官方没有复用两个链表中的结点，每次新建结点。需与面试官沟通，若可以修改传入的两个参数，则复用比较好，若不可修改，则选择官方解法。其次官方在遍历时采用一个while循环，在循环体中判断，联系到if判断底层实质是计算机中断命令，放在循环中每次计算判断对性能有少许影响。

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}
```

## 拓展

&emsp;如果链表中的数字不是按逆序存储的呢？由于链表遍历从表头开始，也就是高位，而运算要从低位开始，可使用栈或递归的方式计算。