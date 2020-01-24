[toc]

Given a linked list, rotate the list to the right by k places, where k is non-negative.



## 题目解读

&emsp;将链表整体右移$k$位，$k$是一个非负数。如果已知链表长度$L$，$k$比表长小则只需截取右边的$k$长链表，然后拼接到左边；大于表长则取余再做截取拼接操作。数据结构为单链表，入参为链表头和偏移数值。

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
    public ListNode rotateRight(ListNode head, int k) {
        
    }
}
```

## 程序设计

* 考虑到要取链表左边的$k$长子链表，而链表是从左到右遍历的，可以采用双指针，第二个指针先于第一个指针$k$个结点；如果$k$小于表长，则当第二个指针遍历到表尾结点时，第一个指针结点就是待拆分结点的前驱。这样就可以将链表从拆分结点拆开，然后尾首合并。
* 对于遍历完后发现$k$比表长大，则取余继续上面操作。

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        // 参数校验
        if(head == null || head.next == null || k == 0){
            return head;
        }
        // 表长
        int len;
        ListNode firstNode = head, secondNode = head;
        // secondNode结点先走k步，并记录遍历后的结点数
        // 如果链表遍历结束，len <= k，则此时len就是表长
       for(len = 1; len <= k && secondNode.next != null; len++){
           secondNode = secondNode.next;
       }
       // 即k = len，链表不动
       if(len == k){
           return head;
       }
        // 即k远大于表长，取余后继续计算
        if(len < k){
            return rotateRight(head, k % len);
        }
        // secondNode遍历到表尾，此时firstNode就是切分结点的前驱结点
        while(secondNode.next != null){
            firstNode = firstNode.next;
            secondNode = secondNode.next;
        }
        // 从切分结点断开链表，并通过尾首连接起来
        ListNode newHead = firstNode.next;
        firstNode.next = null;
        secondNode.next = head;

        return newHead;
    }
}
```

测试样例：链表$1 \to 2$，$k = 0$、$k = 2$的情况，即偏移为0或表长时的特殊情况，链表不变；偏移大于表长时的情况；偏移小于表长的情况。

## 性能分析

&emsp;时间复杂度由于要遍历完链表得到切分结点及尾结点，若遇到取余的情况还要遍历一遍，这两种情况时间复杂度为$O(N)$。空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.94%的用户。

内存消耗：35.6MB，在所有java提交中击败了97.73%的用户。

## 官方解题

&emsp;官方解题对于$k > L$的情况处理不同，当遍历完链表后会将链表首尾连起来形成循环链表，由于第一次遍历已经知道了表长，第二次遍历就是找到$k \% L$的切分结点。该算法巧妙地通过循环链表解决了链表拼接、偏移大于表长等情况。

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        // 参数校验
   		if(head == null || head.next == null || k == 0){
            return head;
    	}

    	// 遍历到链表尾的尾节点
    	ListNode old_tail = head;
        // 表长
        int n;
        // 遍历记录表长
        for(n = 1; old_tail.next != null; n++)
            old_tail = old_tail.next;
        // 首尾连接
        old_tail.next = head;

        // 再次从head出发，遍历链表，此时拆分结点为第(n - k % n + 1)个结点，前驱为第(n - k % n)
        ListNode new_tail = head;
        // 遍历找到前驱结点
        for (int i = 1; i < n - k % n; i++)
            new_tail = new_tail.next;
        ListNode new_head = new_tail.next;

        // 拆分
        new_tail.next = null;

        return new_head;
  }
}
```

&emsp;时间复杂度为$O(N + k \% N)$即$O(N)$，空间复杂度为$O(1)$。与官方方法相比，当$k \le L$时，只需要遍历一次就能找到切分点，而官方方法无差别遍历两次。

执行用时：1ms，在所有java提交中击败了99.94%的用户。

内存消耗：37.3MB，在所有java提交中击败了12.70%的用户。