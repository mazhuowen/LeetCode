[toc]

Given the `head` of a linked list, we repeatedly delete consecutive sequences of nodes that sum to `0` until there are no such sequences.

After doing so, return the head of the final linked list.  You may return any such answer.

 

Constraints:

* The given linked list will contain between `1` and `1000` nodes.
* Each node in the linked list has $-1000 \le \text{node.val} \le 1000$.



## 题目解读

&emsp;给定链表，删除和是0的子序列。

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
    public ListNode removeZeroSumSublists(ListNode head) {

    }
}
```

## 程序设计

* 最基本的思路是类似数组遍历序列和，两层循环，可使用双指针代替实现。

```java
class Solution {
    public ListNode removeZeroSumSublists(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode first = dummy, second;
        // 外层循环
        while (first != null) {
            second = first.next;
            int sum = 0;
            // 内层循环
            while (second != null) {
                sum += second.val;
                second = second.next;
                // 和为0，删除
                if(sum == 0) {
                    first.next = second;
                    sum = 0;
                }
            }
            first = first.next;
        }
        return dummy.next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了60.99%的用户。

内存消耗：39.3MB，在所有java提交中击败了5.11%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用了一种很巧妙的方法，采用字典记录前缀和和结点，如果存在序列为0，则必然存在前缀和为同一个的不同结点，这样字典保存的是最后一个前缀和为`val`的结点。继续遍历，计算前缀和`val`，若存在0序列，则字典中保存最后一个结点，进行删除操作即可，如果不存在0序列，则字典中保存的是其本身。

```java
class Solution {
    public ListNode removeZeroSumSublists(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        HashMap<Integer,ListNode> map = new HashMap<>();
        int total = 0;
        // 记录前缀和
        for(ListNode d = dummy; d != null; d = d.next){
            total += d.val;
            map.put(total, d);
        }
        total = 0;
        // 从头遍历删除
        for(ListNode d = dummy; d != null; d = d.next){
            total += d.val;
            d.next = map.get(total).next;
        }
        return dummy.next;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了96.10%的用户。

内存消耗：41.1MB，在所有java提交中击败了5.11%的用户。