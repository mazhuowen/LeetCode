[toc]

Given a singly linked list, return a random node's value from the linked list. Each node must have the **same probability** of being chosen.

Follow up:
What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?



## 题目解读



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

    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    public Solution(ListNode head) {

    }
    
    /** Returns a random node's value. */
    public int getRandom() {

    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
```

## 程序设计

* 最基本的思路是随机访问结点。

```java
class Solution {
    int size;
    Random random;
    ListNode head;

    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    public Solution(ListNode head) {
        this.random = new Random();
        this.head = head;
        while (head != null) {
            this.size++;
            head = head.next;
        }
    }
    
    /** Returns a random node's value. */
    public int getRandom() {
        int k = random.nextInt(size);
        ListNode temp = head;
        while (k > 0) {
            temp = temp.next;
            k--;
        }
        return temp.val;
    }
}
```

* 如果$n$未知，则使用储水池抽样。将前$n$个放入数组，遍历剩余元素并生成不超过当前索引的随机索引，如果随机索引在数组索引范围内，则替换。

```java
class Solution {
    int k;
    ListNode[] reservoir;
    Random random;

    public Solution(ListNode head) {
        this.k = 1;
        this.reservoir = new ListNode[k];
        this.random = new Random();
        // 将前k个放入数组
        for (int i = 0; i < k && head != null; i++) {
            this.reservoir[i] = head;
            head = head.next;
        }
    }
    
    public int getRandom() {
        ListNode[] temp = Arrays.copyOf(reservoir, k);
        ListNode start = temp[k - 1].next;
        int idx = k;
        // 从第k+1个开始遍历
        while (start != null) {
            // 如果生成的替换索引在k内，替换
            int replace = random.nextInt(idx + 1);
            if (replace < k) temp[replace] = start;
            start = start.next;
            idx++;
        }
        return temp[0].val;
    }
}
```

> 使用数学归纳法证明：假设当前有$n$个样本，进入池子的概率为$\frac{k}{n}$，对于第$n + 1$个样本，要么会替换池子中的某个样本，要么不变；假设池子中的某个样本为$X_j$，首先分析第$n + 1$个样本$X_{n + 1}$不变，$X_j$仍然在池子中的情况。此时$X_{n + 1}$未选中的概率为$P_1 = \frac{n + 1 - k}{n + 1}$；其次分析选中但替换除了$X_j$之外的$k - 1$个样本中的一个的情况，概率为$P_2 = \frac{k}{n + 1}\cdot\frac{k - 1}{k} = \frac{k - 1}{n + 1}$。则$X_j$还在池子中的概率为$\frac{k}{n}(P_1 + P_2) = \frac{k}{n + 1}$。

## 性能分析

&emsp;基本思路时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：12ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.3MB，在所有java提交中击败了50.00%的用户。

&emsp;蓄水池抽样时间复杂度为$O(N)$，空间复杂度为$O(K)$。

执行用时：18ms，在所有java提交中击败了50.84%的用户。

内存消耗：40.9MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。