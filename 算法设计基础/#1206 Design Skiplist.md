[toc]

Design a Skiplist without using any built-in libraries.

A Skiplist is a data structure that takes $O(log(n))$ time to `add`, `erase` and `search`. Comparing with treap and red-black tree which has the same function and performance, the code length of Skiplist can be comparatively short and the idea behind Skiplists are just simple linked lists.

You can see there are many layers in the Skiplist. Each layer is a sorted linked list. With the help of the top layers, `add` , `erase` and `search` can be faster than $O(n)$. It can be proven that the average time complexity for each operation is $O(log(n))$ and space complexity is $O(n)$.

To be specific, your design should include these functions:

* `bool search(int target)` : Return whether the `target` exists in the Skiplist or not.
* `void add(int num)`: Insert a value into the SkipList. 
* `bool erase(int num)`: Remove a value in the Skiplist. If `num` does not exist in the Skiplist, do nothing and return false. If there exists multiple `num` values, removing any one of them is fine.

See more about Skiplist : https://en.wikipedia.org/wiki/Skip_list

Note that duplicates may exist in the Skiplist, your code needs to handle this situation.



Constraints:

* $0 \le \text{num, target} \le 20000$
* At most $50000$ calls will be made to `search`, `add`, and `erase`.



## 题目解读

&emsp;设计跳表。

```java
class Skiplist {

    public Skiplist() {

    }
    
    public boolean search(int target) {

    }
    
    public void add(int num) {

    }
    
    public boolean erase(int num) {

    }
}

/**
 * Your Skiplist object will be instantiated and called as such:
 * Skiplist obj = new Skiplist();
 * boolean param_1 = obj.search(target);
 * obj.add(num);
 * boolean param_3 = obj.erase(num);
 */
```

## 程序设计

* 节点数据结构设计如下：`next`指向同层下一个节点，`down`指向下一层节点。

```java
class Node {
    int val;

    // 层数
    int level;
    // 后继
    Node next;
    // 下一层
    Node down;

    Node(int val, int level, Node next, Node down) {
        this.val = val;
        this.level = level;
        this.next = next;
        this.down = down;
    }
}
```

* 主流程如下：插入时根据随机数确定插入的层数，其余过程为链表的插入删除，需注意新增层数超过当前最高层两层以上的情况。

```java
class Skiplist {
    // 标识头节点
    Node head;
    // 元素数
    int size;
    // 最大层数（最底层为0）
    int level;

    public Skiplist() {
        this.head = new Node(-1, 0, null, null);
        this.size = 0;
        this.level = 0;
    }

    public boolean search(int target) {
        Node temp = head;
        // 逐层查找
        for (int i = level; i >= 0; i--) {
            while (temp.next != null && temp.next.val < target) temp = temp.next;
            if (temp.next != null && temp.next.val == target) return true;
            // 下一层查找
            temp = temp.down;
        }
        // 不存在
        return false;
    }

    public void add(int num) {
        // if (!check()) throw new RuntimeException("add异常：" + toString());

        // 空表
        if (size == 0) {
            head.next = new Node(num, 0, null, null);
            size++;
            return;
        }

        // 插入的每层路径
        Node[] path = prev(num);
        // 已经存在则返回（本题支持重复插入，注释该行）
        // if (path[0].next != null && path[0].next.val == num) return;

        // 规模加一
        size++;
        int curLevel = level();
        // 逐层插入
        for (int i = 0; i <= curLevel && i <= level; i++) {
            if (i == 0) path[i].next = new Node(num, i, path[i].next, null);
            else path[i].next = new Node(num, i, path[i].next, path[i - 1].next);
        }

        Node down = path[level].next;
        // 如果层数比当前层数高，新增层数
        while (curLevel > level) {
            level++;
            down = new Node(num, level, null, down);
            head = new Node(-1, level, down, head);
        }
    }

    public boolean erase(int num) {
        //if (!check()) throw new RuntimeException("erase异常：" + toString());

        Node[] path = prev(num);
        // 不存在删除值
        if (path[0].next == null || path[0].next.val != num) return false;

        size--;
        // 逐层删除
        for (int i = 0; i <= level && path[i].next != null && path[i].next.val == num; i++) {
            path[i].next = path[i].next.next;
        }
        return true;
    }

    private Node[] prev(int num) {
        // 查找每层插入的前驱
        Node[] prev = new Node[level + 1];
        Node temp = head;
        for (int i = level; i >= 0; i--) {
            while (temp.next != null && temp.next.val < num) temp = temp.next;
            prev[i] = temp;
            temp = temp.down;
        }
        return prev;
    }

    // 随机算法生成当前节点层数，0～size的随机数
    private int level() {
        Random random = new Random();
        int randomNum = random.nextInt(size + 1);
        int curLevel = 0;
        while (randomNum > 1) {
            randomNum >>= 1;
            curLevel++;
        }
        return curLevel;
    }

    // 方便调试
    private boolean check() {
        Node temp = head;
        int cueLevel = level;
        while (temp != null) {
            Node temp2 = temp;
            while (temp2 != null) {
                if (temp2.level != cueLevel) return false;
                temp2 = temp2.next;
            }
            cueLevel--;
            temp = temp.down;
        }
        return true;
    }

    // 打印数据结构
    @Override
    public String toString() {
        StringBuffer res = new StringBuffer();
        Node temp = head;
        while (temp != null) {
            Node temp2 = temp;
            res.append("\n");
            while (temp2 != null) {
                res.append("(").append(temp2.val).append(",").append(temp2.level).append(") -> ");
                temp2 = temp2.next;
            }
            res.append("null");
            temp = temp.down;
        }
        return res.toString();
    }
}
```

## 性能分析

&emsp;插入、删除、遍历时间复杂度均为$O(\log_2N)$，空间复杂度为$O(N)$。

执行用时：54ms，在所有java提交中击败了40.88%的用户。

内存消耗：49.4MB，在所有java提交中击败了25.00%的用户。

## 官方解题

&emsp;暂无，密切关注。