[toc]

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.



## 题目解读

&emsp;合并多个有序链表。数据结构为单链表，输入参数为包含多个链表的数组。

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
    public ListNode mergeKLists(ListNode[] lists) {
    }
}
```

## 程序设计

* 考虑到[#21 Merge Two Sorted Lists](./#21 Merge Two Sorted Lists.md)中合并两个链表，可以选择在数组列表中一一合并；进一步的，归并排序将数字拆分并两两合并排序，利用归并排序的分治策略，我们可以将数组中的链表二分，直到剩余单条链表，然后再递归合并，每次只需要合并两个链表。
* 需注意数组索引下表越界等细节；另外对start和end二分后的区间为start～mid，mid+1～end；若是start～mid-1，mid～end，由于程序除法在start、end不相等时，mid大于等于start，小于end，故mid-1可能会比start小，从而造成连锁错误，而mid+1保证了不超过end，区间划分一定要注意。

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
         // 参数校验
        if(lists == null || lists.length == 0) {
            return null;
        }
		// 采用分治
        ListNode newList = mergeKLists(lists, 0, lists.length - 1);
        return newList;
    }

    private ListNode mergeKLists(ListNode[] lists, int start, int end) {
        // 递归终止条件，即单条链表返回开始合并
        if(start == end) {
            return lists[start];
        }
        // 拆分，对子区间继续递归拆分
        int mid = (start + end) >>> 1;
        ListNode leftList = mergeKLists(lists, start, mid);
        ListNode rightList = mergeKLists(lists, mid + 1, end);

        // 哑结点
        ListNode dummy = new ListNode(0);
        ListNode tmpNode = dummy;

        // 合并（即合并两个有序链接）
        while(leftList != null && rightList != null){
            if(leftList.val < rightList.val){
                tmpNode.next = leftList;
                tmpNode = leftList;
                leftList = leftList.next;
            } else {
                tmpNode.next = rightList;
                tmpNode = rightList;
                rightList = rightList.next;
            }
        }
		// 拼接未遍历比较的部分
        if(leftList != null){
            tmpNode.next = leftList;
        }
        if(rightList != null){
            tmpNode.next = rightList;
        }
        // 返回合并后的链表，递归继续合并
        return dummy.next;
    }
}
```

测试样例：输入数组包含$1 \to 4 \to 5$、$1 \to 3 \to 4$、$2 \to 6$，输出$1 \to 1 \to 2 \to 3 \to 4 \to 4 \to 5 \to 6$测试普通功能；输入存在链表为null测试特殊情况。

## 性能分析

&emsp;假设有$k$个链表，总的结点数为$N$，拆分并合并$\log_2K$次，每次合并都要比较$N$个结点，以一层拆分合并为例，例如第一次拆分为$\frac{k}{2}$的两部分，待递归合并后会有已合并的两个链表，这一层会对这两个链表比较合并，比较次数为总结点数$N$，依次类推每一层都要比较$N$次，即时间复杂度为$O(N\log_2K)$。由于总方法创建了一个指针，递归方法创建了四个指针，均是在递归过程后创建，在每次递归结束后局部变量会从栈中弹出，总的空间复杂度是常量级。

执行用时：2ms，在所有java提交中击败了100.00%的用户。

内存消耗：41MB，在所有java提交中击败了56.62%的用户。

<img src="../images/#23.png" style="zoom:80%;" />

## 官方解题

&emsp;除了上述最优解，官方提供了以下思路：

* 暴力：遍历所有链表，将结点放在数组中，将这个数组排序，遍历并创建新的有序链表。排序可考虑动态排序，如优先级队列。时间复杂度上，遍历所有结点为$O(N)$的复杂度，优先级队列排序花费$O(N\log_2N)$，创建新链表花费$O(N)$，根据加法定理，时间复杂度为$O(N\log_2N)$。空间复杂度上，排序花费$O(N)$，创建新链表花费$O(N)$，总的花费$O(N)$。
* 逐一比较：即合并两个链表的思路拓展，比较$k$个链表的$k$个结点，将最小的结点拼接到新链表，然后继续遍历比较。时间复杂度上，每次结点比较$k - 1$次，总共比较$N$次，即$O(kN)$。空间复杂度上若创建新链表，复杂度为$O(N)$，若采用哑结点拼接，复杂度为$O(1)$。
* 采用优先级队列逐一比较：每次比较都是弹出最小结点的过程，弹出后遍历弹出结点的下一结点，并入队，具体的取出堆顶结点，将其后继结点向下冒泡，时间复杂度为$O(\log_2k)$；重复这个过程直到结束。时间复杂度上，由于比较$N$次，故总的时间复杂度为$O(N\log_2k)$。空间复杂度上，由于优先级队列花费$O(k)$，若复用结点，则总的空间复杂度为$O(k)$；若创建结点，总的空间复杂度为$O(N)$，因为$k$小于$N$。
* 逐一两两合并：将合并$k$个链表的问题转化为合并两个链表$k-1$次的问题。假设平均每条链表含有结点数$\frac{N}{k}$，第一次合并是两条$\frac{N}{k}$的链表合并，时间复杂度为$O(\frac{2N}{k})$；第二次合并为第一次合并的链表$\frac{2N}{k}$与第三条链表$\frac{N}{k}$合并，以此类推，合并$k-1$次总的时间复杂度为$O(\sum_{i=1}^{k-1}(i\cdot\frac{N}{k} + \frac{N}{k})) = O((\frac{1}{2} + \frac{N}{k})N(k-1))$，约去低级项及系数，得$O(kN)$。空间复杂度由于复用为$O(1)$。

> 逐一两两比较和分治实际上都合并了$k-1$次，但是时间复杂度有明显差异；分治每次比较$N$次，而逐一两两合并，每次合并后链表增长，上面的分析指出第$i$次合并需要$O(i\cdot\frac{N}{k} + \frac{N}{k})$，而需要合并$k-1$次，总的耗时比较大。

