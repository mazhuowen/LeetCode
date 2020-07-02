[toc]

A Range Module is a module that tracks ranges of numbers. Your task is to design and implement the following interfaces in an efficient manner.

* `addRange(int left, int right)` Adds the half-open interval `[left, right)`, tracking every real number in that interval. Adding an interval that partially overlaps with currently tracked numbers should add any numbers in the interval `[left, right)` that are not already tracked.
* `queryRange(int left, int right)` Returns true if and only if every real number in the interval `[left, right)` is currently being tracked.
* `removeRange(int left, int right)` Stops tracking every real number currently being tracked in the interval `[left, right)`.

**Note**:

* A half open interval `[left, right)` denotes all real numbers $\text{left} \le x < \text{right}$.
* $0 < \text{left} < \text{right} < 10^9$ in all calls to `addRange`, `queryRange`, `removeRange`.
* The total number of calls to `addRange` in a single test case is at most `1000`.
* The total number of calls to `queryRange` in a single test case is at most `5000`.
* The total number of calls to `removeRange` in a single test case is at most `1000`.



## 题目解读

&emsp;设计数据结构添加、删除区间，支持查询区间是否是有效的。

```java
class RangeModule {

    public RangeModule() {

    }
    
    public void addRange(int left, int right) {

    }
    
    public boolean queryRange(int left, int right) {

    }
    
    public void removeRange(int left, int right) {

    }
}

/**
 * Your RangeModule object will be instantiated and called as such:
 * RangeModule obj = new RangeModule();
 * obj.addRange(left,right);
 * boolean param_2 = obj.queryRange(left,right);
 * obj.removeRange(left,right);
 */
```

## 程序设计

* 最基本的思路是线段树维护添加、删除区间，使用标识来区分当前区间是有效还是无效，或者是包含有效和无效区间的；由于频繁添加、删除，导致区间划分很细，树的高度最后达到最大高度，性能较差。

```java
class RangeModule {
    TreeNode root;

    public RangeModule() {
        this.root = new TreeNode(0, 1_000_000_000);
    }

    public void addRange(int left, int right) {
        this.root.insert(left, right);
    }

    public boolean queryRange(int left, int right) {
        return this.root.query(left, right);
    }

    public void removeRange(int left, int right) {
        this.root.remove(left, right);
    }
}


class TreeNode {
    int start, end;
    // 0子区间存在1和2，1有效，2移除
    int stat;

    TreeNode left, right;

    TreeNode(int start, int end) {
        this.start = start;
        this.end = end;
    }

    private int getMid() {
        return start + (end - start) / 2;
    }

    private TreeNode left() {
        if (this.left == null) this.left = new TreeNode(this.start, getMid());
        return this.left;
    }

    private TreeNode right() {
        if (this.right == null) this.right = new TreeNode(getMid(), this.end);
        return this.right;
    }

    public void insert(int s, int e) {
        insert(s, e, 1);
    }

    public void remove(int s, int e) {
        insert(s, e, 2);
    }

    public boolean query(int s, int e) {
        if (s >= e) throw new IllegalArgumentException("invalid param");
        if (this.stat != 0 && this.start <= s && this.end >= e) return this.stat == 1;

        // 切分区间
        int mid = getMid();
        if (e <= mid) return this.left != null && left().query(s, e);
        else if (s >= mid) return this.right != null && right().query(s, e);
        else return this.left != null &&  left().query(s, mid) && this.right != null && right().query(mid, e);
    }

    private void insert(int s, int e, int stat) {
        if (s >= e) throw new IllegalArgumentException("invalid param");

        // 超出范围
        if (this.start >= e || this.end <= s) return;
        if (this.start >= s && this.end <= e) {
            this.stat = stat;
            return;
        }

        // 延迟更新
        if (this.stat != 0) {
            left().stat = this.stat;
            right().stat = this.stat;
        }

        left().insert(s, e, stat);
        right().insert(s, e, stat);
        // 更新当前区间状态
        if (left().stat != right().stat) this.stat = 0;
        else this.stat = left().stat;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_210^9)$，空间复杂度为$O(2 \times 10^9 - 1)$。

执行用时：99ms，在所有java提交中击败了17.78%的用户。

内存消耗：57MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方采用红黑树。由上面的分析可知，线段树只能划分区间，无法合并，导致树的高度过高，采用红黑树合并切分区间是更好的选择。

```java
class RangeModule {
    TreeSet<Interval> intervals;

    public RangeModule() {
        this.intervals = new TreeSet<>();
    }
    
    public void addRange(int left, int right) {
        Set<Interval> remove = new HashSet<>();
        for (Interval interval : intervals) {
            // 有交集
            if (interval.right >= left && interval.left <= right) {
                // 合并区间
                left = Math.min(left, interval.left);
                right = Math.max(right, interval.right);
                // 删除旧的区间
                remove.add(interval);
            } else if (interval.left > right) {
                break;
            }
        }
        intervals.removeAll(remove);
        intervals.add(new Interval(left, right));
    }
    
    public boolean queryRange(int left, int right) {
        Interval cur = intervals.higher(new Interval(0, left));
        return cur != null && cur.left <= left && cur.right >= right;
    }
    
    public void removeRange(int left, int right) {
        Set<Interval> remove = new HashSet<>();
        Set<Interval> add = new HashSet<>();
        for (Interval interval : intervals) {
            // 有交集
            if (interval.right > left && interval.left < right) {
                remove.add(interval);
                if (interval.left < left || interval.right > right) {
                    if (interval.left < left) add.add(new Interval(interval.left, left));
                    if (interval.right > right) add.add(new Interval(right, interval.right));
                }
            } else if (interval.left > right) {
                break;
            }
        }
        intervals.removeAll(remove);
        intervals.addAll(add);
    }
}


class Interval implements Comparable<Interval>{
    int left;
    int right;

    public Interval(int left, int right){
        this.left = left;
        this.right = right;
    }

    public int compareTo(Interval that){
        if (this.right == that.right) return this.left - that.left;
        return this.right - that.right;
    }
}
```

&emsp;时间复杂度加入删除为$O(N)$，查询为$O(\log_2N)$，空间复杂度为$O(N)$。

执行用时：116ms，在所有java提交中击败了16.67%的用户。

内存消耗：48.5MB，在所有java提交中击败了100.00%的用户。

> 可使用`Iterator<Interval> itr = intervals.tailSet(new Interval(0, left)).iterator();`从指定位置遍历集合，而不是从头开始遍历，大大减少时间。