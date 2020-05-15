[toc]

Given two 1d vectors, implement an iterator to return their elements alternately.



Follow up:

What if you are given `k` 1d vectors? How well can your code be extended to such cases?



## 题目解读

&emsp;锯齿迭代两个数组元素。

```java
public class ZigzagIterator {

    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        
    }

    public int next() {
        
    }

    public boolean hasNext() {
        
    }
}

/**
 * Your ZigzagIterator object will be instantiated and called as such:
 * ZigzagIterator i = new ZigzagIterator(v1, v2);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

## 程序设计

* 使用标识来判断遍历哪一条数组。

```java
public class ZigzagIterator {
    // 记录遍历的索引
    int idx1, idx2;
    List<Integer> v1, v2;
    // 记录遍历哪条数组
    boolean flag;

    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        this.v1 = v1;
        this.v2 = v2;
        this.flag = true;
    }

    public int next() {
        // 当前索引有效则遍历当前或者当前索引无效则遍历另一条
        if (flag && idx1 < v1.size() || idx2 >= v2.size()) {
            // 标识取反
            flag = !flag;
            return v1.get(idx1++);
        } else {
            flag = !flag;
            return v2.get(idx2++);
        }
    }

    public boolean hasNext() {
        return idx1 < v1.size() || idx2 < v2.size();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。题目提出锯齿遍历`k`条数组，则可用大小为`k`的数组或哈希表保存每条链表的遍历索引；使用该数组的索引作为锯齿判断的依据，每判断完则更新索引，指向下一条要判断的数组。