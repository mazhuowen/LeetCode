[toc]

We are given a linked list with `head` as the first node.  Let's number the nodes in the list: `node_1`, `node_2`, `node_3`, `...` etc.

Each node may have a next larger **value**: for `node_i`, `next_larger(node_i)` is the `node_j.val` such that `j > i`, `node_j.val > node_i.val`, and `j` is the smallest possible choice.  If such a `j` does not exist, the next larger value is `0`.

Return an array of integers `answer`, where `answer[i] = next_larger(node_{i+1})`.

Note that in the example **inputs** (not outputs) below, arrays such as `[2,1,5]` represent the serialization of a linked list with a head node value of 2, second node value of 1, and third node value of 5.



Note:

* $1 \le \text{node.val} \le 10^9$ for each node in the linked list.
* The given list has length in the range `[0, 10000]`.



## 题目解读

&emsp;给定链表，寻找每个元素之后第一个大于该元素的值，无存在则标记0。

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
    public int[] nextLargerNodes(ListNode head) {

    }
}
```

## 程序设计

* 注意此题是寻找第一个大于当前元素的后继，而不是之后最大的值。注意到这个区别就不能使用递归简单返回链表后继最大值。
* 采用单调栈维护递减序列，遇到大于栈中的值，则次值就是栈中小于该值的第一个较大后继，出栈记录，入栈当前值，如此循环。
* 繁琐点在于按照索引顺序输出，要怎么记录这些点。栈中记录元素值和索引，维护一个链表，每遍历一次就新增0，如果这个值后续遇到大于它的后继，则出栈更新，链表中必然存在该索引。为了方便查找，采用数组表要比链表快很多。

```java
class Solution {
    public int[] nextLargerNodes(ListNode head) {
        if (head == null) return new int[]{};
        List<Integer> list = new ArrayList<>();
        // 单调递减，保存索引（结点顺序）和结点值
        Stack<int[]> stack = new Stack<>();
        int idx = 0;
        while(head != null) {
            // 加入占位
            list.add(0);
            while (!stack.isEmpty() && head.val > stack.peek()[1]) {
                int[] cur = stack.pop();
                list.set(cur[0], head.val);
            }
            // 加入栈
            stack.push(new int[]{idx, head.val});
            idx++;
            head = head.next;
        }
        int[] res = new int[idx];
        for (int i = 0; i < idx; i++) {
            res[i] = list.get(i);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：20ms，在所有java提交中击败了87.16%的用户。

内存消耗：43.7MB，在所有java提交中击败了5.55%的用户。

## 官方解题

&emsp;暂无，密切关注。