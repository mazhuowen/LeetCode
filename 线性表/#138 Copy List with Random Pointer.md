[toc]

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a **deep copy** of the list.

The Linked List is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]​` where:

* `val`: an integer representing `Node.val`
* `random_index`: the index of the node (range from `0` to `n-1`) where random pointer points to, or `null` if it does not point to any node.

Constraints:

* -10000 <= `Node.val` <= 10000
* `Node.random` is null or pointing to a node in the linked list.
* Number of Nodes will not exceed 1000.



## 题目解读

&emsp;题目要求给定一个链表，结点数据结构包含一个后继和一个随机结点，需要输出该链表的深拷贝。难点在于遍历创建当前结点时，随机结点可能在后面还没有创建，也可能在前面，需要重新遍历关联。考虑到这种复杂性，题目给定限制，结点绝对值不超过10000，结点数不超过1000。

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        
    }
}
```

## 程序设计

* 参考官方最优解思路。上面分析了难点在于随机结点在当前结点创建时的关联问题，即在单次过程中，我们很难通过原结点的随机结点对应到新创建的结点，可以借助字典等维护这个关系。但是更巧妙的，可以将深度复制的结点插入到原结点后面，拼接结点后面接原结点的后继；这样随机结点的关系就得以确定且非常简洁：x新结点的随机指针对应原结点的随机指针的后继。
* 经过上述分析，可以得到流程：首先遍历复制插入新结点；然后从新遍历，复制随机结点；最后拆分两条链表。
* 需要注意随机结点是`null`的情况。

<img src="../images/#138.png" style="zoom:100%;" />

<img src="../images/#138_1.png" style="zoom:100%;" />

```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head == null) {
            return null;
        }
        Node traversal = head, temp, insert;
        // 创建新结点，插入到原结点后面
        while(traversal != null) {
            // 保存后继
            temp = traversal.next;
            // 插入
            insert = new Node(traversal.val);
            traversal.next = insert;
            insert.next = temp;
            // 继续遍历
            traversal = temp;
        }

        // 开始复制随机结点
        Node random;
        traversal = head;
        while(traversal != null){
            // 复制的结点
            insert = traversal.next;
            random = traversal.random;
            // 复制随机指针，需考虑为null的特殊情况
            insert.random = random == null ? null : random.next;
            // 继续遍历
            traversal = traversal.next.next;
        }

        // 拆分两个链表
        // 复制的链表头
        temp = head.next;
        traversal = head;
        while(traversal != null){
            insert = traversal.next;
            // 原链表连接
            traversal.next = insert.next;
            // 继续遍历
            traversal = traversal.next;
            // 新链表链接
            insert.next = traversal == null ? null : traversal.next;
        }

        return temp;
    }
}
```

测试样例：给定`head = [[7,null],[13,0],[11,4],[10,2],[1,0]]`，输出`[[7,null],[13,0],[11,4],[10,2],[1,0]]`测试普通情况；给定链表存在随机指针指向自己的结点，测试特殊情况；给定链表`val`都相同，测试特殊情况；`null`链表返回`null`。

## 性能分析

&emsp;整体为三次链表遍历，时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.6MB，在所有java提交中击败了7.46%的用户。

## 官方解题

&emsp;除了上述最优解，官方还提供了两种思路：回溯及迭代。在我看来这两种思路都一样，引入字典，遍历原链表时创建新结点及新结点的随机结点。至于这个新结点和随机结点之前是否已经创建及与原结点的对应关系都维护在字典中，之前没有创建则创建，每创建一个结点，就以原结点、新结点的键值对加入字典；之前已经创建，则从字典取出。时间复杂度$O(N)$，空间复杂度为$O(1)$。