[toc]

We are given `head`, the head node of a linked list containing **unique integer values**.

We are also given the list `G`, a subset of the values in the linked list.

Return the number of connected components in `G`, where two values are connected if they appear consecutively in the linked list.



**Note:**

- If `N` is the length of the linked list given by `head`, $1 \le N \le 10000$.
- The value of each node in the linked list will be in the range` [0, N - 1]`.
- $1 \le \text{G.length} \le 10000$.
- `G` is a subset of all values in the linked list.



## 题目解读

&emsp;给定包含不重复值的链表，和其子集构成的数组，判断数组中包含的组件数目，组件定义为在链表中连续的一段。

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
    public int numComponents(ListNode head, int[] G) {

    }
}
```

## 程序设计

* 仔细观察示例，如果数组中的数对应链表中的结点是表尾或结点后继不包含在数组中，则是一个组件。由于数组中组件不一定是按照链表中出现顺序排序的，需要集合记录，方便判断。

```java
class Solution {
    public int numComponents(ListNode head, int[] G) {
        if (head == null || G == null || G.length == 0) return 0;

        // 将G中数字加入集合
        Set<Integer> set = new HashSet<>();
        for (int num : G) set.add(num);

        int count = 0;
        while (head != null) {
            if (set.contains(head.val)) {
                // 组件计数
                if (head.next == null || !set.contains(head.next.val)) count++;
            }
            head = head.next;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：8ms，在所有java提交中击败了85.46%的用户。

内存消耗：41.5MB，在所有java提交中击败了8.55%的用户。

## 官方解题

&emsp;同上。