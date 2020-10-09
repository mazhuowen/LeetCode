[toc]

You have a queue of integers, you need to retrieve the first unique integer in the queue.

Implement the `FirstUnique` class:

* `FirstUnique(int[] nums)` Initializes the object with the numbers in the queue.
* `int showFirstUnique()` returns the value of the first unique integer of the queue, and returns $-1$ if there is no such integer.
* `void add(int value)` insert value to the queue.



**Constraints**:

* $1 \le \text{nums.length} \le 10^5$
* $1 \le \text{nums[i]} \le 10^8$
* $1 \le \text{value} \le 10^8$
* At most $50000$ calls will be made to `showFirstUnique` and `add`.



## 题目解读

&emsp;设计数据结构，返回队列中第一个唯一的数字。

```java
class FirstUnique {

    public FirstUnique(int[] nums) {

    }
    
    public int showFirstUnique() {

    }
    
    public void add(int value) {

    }
}

/**
 * Your FirstUnique object will be instantiated and called as such:
 * FirstUnique obj = new FirstUnique(nums);
 * int param_1 = obj.showFirstUnique();
 * obj.add(value);
 */
```

## 程序设计

* 使用链表维护唯一的数字，使用字典维护唯一的数字及其前驱节点；加入一个数时先判断是否在链表中，在则需要删除；不在则加入/需要一个数据结构记录非唯一数字，方便判断。

```java
class FirstUnique {
    int idx;
    Node head, tail;
    Map<Integer, Node> record;
    Set<Integer> remove;

    public FirstUnique(int[] nums) {
        this.head = new Node(-1, -1);
        this.record = new HashMap<>();
        this.remove = new HashSet<>();
        this.tail = head;
        while (idx < nums.length) {
            this.add(nums[idx]);
        }
    }
    
    public int showFirstUnique() {
        if (head.next == null) return -1;
        else return head.next.val;
    }
    
    public void add(int value) {
        idx++;
        if (remove.contains(value)) return;
        // 待删除
        if (record.containsKey(value)) {
            Node pre = record.get(value);
            pre.next = pre.next.next;
            // 更新尾节点或维护新关系
            if (pre.next == null) tail = pre;
            else record.put(pre.next.val, pre);
            // 删除重复数字
            record.remove(value);
            remove.add(value);
        } else {
            // 插入队尾
            tail.next = new Node(value, idx - 1);
            record.put(value, tail);
            tail = tail.next;
        }
    }
}

class Node {
    int val;
    int idx;

    Node next;

    Node(int val, int idx) {
        this.val = val;
        this.idx = idx;
    }
}
```

## 性能分析

&emsp;时间复杂度构建为$O(N)$，加入及查询为$O(1)$，空间复杂度为$O(N)$。

执行用时：149ms，在所有java提交中击败了77.42%的用户。

内存消耗：77.9MB，在所有java提交中击败了78.18%的用户。

## 官方解题

&emsp;暂无，密切关注。