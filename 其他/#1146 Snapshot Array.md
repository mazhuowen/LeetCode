[toc]

Implement a SnapshotArray that supports the following interface:

* `SnapshotArray(int length)` initializes an array-like data structure with the given length.  **Initially, each element equals 0**.
* `void set(index, val)` sets the element at the given index to be equal to `val`.
  `int snap()` takes a snapshot of the array and returns the `snap_id`: the total number of times we called `snap()` minus $1$.
* `int get(index, snap_id)` returns the value at the given `index`, at the time we took the snapshot with the given `snap_id`



**Constraints**:

* $1 \le \text{length} \le 50000$
* At most `50000` calls will be made to `set`, `snap`, and `get`.
* $0 \le \text{index} < \text{length}$
* $0 \le \text{snap_id} < (\text{the total number of times we call snap}())$
* $0 \le \text{val} \le 10^9$



## 题目解读

&emsp;设计类似数组的数据结构，支持快照，可根据快照版本号查询历史值。

```java
class SnapshotArray {

    public SnapshotArray(int length) {

    }
    
    public void set(int index, int val) {

    }
    
    public int snap() {

    }
    
    public int get(int index, int snap_id) {

    }
}

/**
 * Your SnapshotArray object will be instantiated and called as such:
 * SnapshotArray obj = new SnapshotArray(length);
 * obj.set(index,val);
 * int param_2 = obj.snap();
 * int param_3 = obj.get(index,snap_id);
 */
```

## 程序设计

* 可用哈希表保存每个位置的版本和数值，每次快照相当于版本号增加，后续赋值都使用当前的版本号；对于在某个版本后没有操作的位置，如果使用当前版本号查询会查不到值，可迭代降低版本号查询。

```java
class SnapshotArray {
    private int snapId = 0;
    Map<Integer, Integer>[] array;

    public SnapshotArray(int length) {
        this.array = new HashMap[length];
        for (int i = 0; i < length; i++) {
            array[i] = new HashMap<>();
            // 初始化版本号和初始值零
            array[i].put(snapId, 0);
        }
    }
    
    public void set(int index, int val) {
        array[index].put(snapId, val);
    }
    
    public int snap() {
        // 版本号增加
        return snapId++;
    }
    
    public int get(int index, int snap_id) {
        int offset = 0;
        // 降低版本号查询
        while (array[index].get(snap_id - offset) == null) offset++;
        return array[index].get(snap_id - offset);
    }
}
```

## 性能分析

&emsp;时间复杂度，构造函数初始化为$O(N)$，赋值、快照为$O(1)$，获取值为$O(S)$，其中$S$为快照次数。

执行用时：46ms，在所有java提交中击败了80.18%的用户。

内存消耗：77.7MB，在所有java提交中击败了10.00%的用户。

## 官方解题

&emsp;暂无，密切关注。