[toc]

Implementing the class `MajorityChecker`, which has the following API:

- `MajorityChecker(int[] arr)` constructs an instance of MajorityChecker with the given array `arr`;
- `int query(int left, int right, int threshold)` has arguments such that:
  - $0 \le \text{left} \le \text{right} < \text{arr.length}$ representing a subarray of `arr`;
  - $2 * \text{threshold} > \text{right} - \text{left} + 1$, ie. the threshold is always a strict majority of the length of the subarray

Each `query(...)` returns the element in `arr[left], arr[left+1], ..., arr[right]` that occurs at least `threshold` times, or `-1` if no such element exists.



**Constraints:**

- $1 \le \text{arr.length} \le 20000$
- $1 \le \text{arr[i]} \le 20000$
- For each query, $0 \le \text{left} \le \text{right} < \text{len(arr)}$
- For each query, $2 * \text{threshold} > \text{right} - \text{left} + 1$
- The number of queries is at most `10000`



## 题目解读

&emsp;给定数组，设计实现类使得根据区间可以查询到不少于阈值的数。

```java
class MajorityChecker {

    public MajorityChecker(int[] arr) {

    }
    
    public int query(int left, int right, int threshold) {

    }
}

/**
 * Your MajorityChecker object will be instantiated and called as such:
 * MajorityChecker obj = new MajorityChecker(arr);
 * int param_1 = obj.query(left,right,threshold);
 */
```

## 程序设计

* 由于是区间相关，且涉及到大量查询，首先想到的是线段树。首先线段树结构如下，包含更新和查询。更新从底向上，如果左、右区间包含超过其区间一半的值，则在右、左区间遍历查找该值并计数，最后若超过当前区间的一半就更新。如果一个值不超过子区间的一半，其在父区间必然不超过一半。

```java
class Tree {
    // 数组区间
    int start, end;
    // 超过数组一半的数和数目，如果存在必然只有一个，不存在则计数为-1
    int num;
    int count;
    // 子区间
    Tree left, right;

    // 记录数组
    int[] arr;

    Tree(int start, int end, int[] arr) {
        this.start = start;
        this.end = end;
        this.arr = arr;
        // 初始化计数为-1
        this.count = -1;
    }

    private int getMid() {
        return start + (end - start) / 2;
    }

    private Tree left() {
        if (left == null) left = new Tree(start, getMid(), arr);
        return left;
    }

    private Tree right() {
        if (right == null) right = new Tree(getMid() + 1, end, arr);
        return right;
    }

    public void update(int idx) {
        // 递归终止条件
        if (start > idx || end < idx) return;
        // 找到位置，该区间只有一个值就是本身，更新计数为1
        if (start == idx && end == idx) {
            num = arr[idx];
            count = 1;
            return;
        }
        // 递归
        left().update(idx);
        right().update(idx);
        // 更新当前结点
        // 左结点存在大于一半区间，在右区间查找该值并计数叠加
        if (left().count != -1) {
            int c = left().count;
            for (int i = right().start; i <= right().end; i++) {
                if (left().num == arr[i]) c++;
            }
            // 更新
            if (c > count && c > (end - start + 1) / 2) {
                count = c;
                num = left().num;
            }
        }

        // 同理，在左区间查找
        if (right().count != -1) {
            int c = right().count;
            for (int i = left().start; i <= left().end; i++) {
                if (right().num == arr[i]) c++;
            }
            // 更新
            if (c > count && c > (end - start + 1) / 2) {
                count = c;
                num = right().num;
            }
        }
    }

    public int[] query(int s, int e) {
        // 包含树的区间
        if (start >= s && end <= e) {
            // 返回
            return new int[]{num, count};
        }
        // 不包含区间（-2表示超出区间）
        if (start > e || end < s) {
            return new int[]{0, -2};
        }
        int[] l = left().query(s, e);
        int[] r = right().query(s, e);

        int mid = getMid();
        // 不相交
        if (r[1] == -2 && l[1] == -2) return new int[]{0, -2};
        // 相交在右边
        if (mid < s) return r;
        // 相交在左边
        if (mid >= e) return l;
        // 左右区间都没有超过半数的
        if (l[1] < 0 && r[1] < 0) {
            return new int[]{0, -1};
        }
        // 左右区间的超半数的数字相同，结果相加返回
        if (l[1] > 0 && r[1] > 0 && l[0] == r[0]) {
            return new int[]{l[0], l[1] + r[1]};
        }
        int lStart = Math.max(s, start);
        int rEnd = Math.min(e, end);
        int[] res = new int[]{0, -1};
        if (l[1] != -1) {
            int c = l[1];
            for (int i = mid + 1; i <= rEnd; i++) {
                if (l[0] == arr[i]) c++;
            }
            // 更新
            if (c > res[1] && c > (rEnd - lStart + 1) / 2) {
                res[1] = c;
                res[0] = l[0];
            }
        }
        // 同理，在左区间查找
        if (r[1] != -1) {
            int c = r[1];
            for (int i = lStart; i <= mid; i++) {
                if (r[0] == arr[i]) c++;
            }
            // 更新
            if (c > res[1] && c > (rEnd - lStart + 1) / 2) {
                res[1] = c;
                res[0] = r[0];
            }
        }
        return res;
    }
}
```

* 主体代码如下：

```java
class MajorityChecker {
    private Tree root;

    public MajorityChecker(int[] arr) {
        if (arr == null || arr.length < 1) throw new IllegalArgumentException("invalid param");
        this.root = new Tree(0, arr.length - 1, arr);
        for (int i = 0; i < arr.length; i++) {
            root.update(i);
        }
    }

    public int query(int left, int right, int threshold) {
        int[] res = root.query(left, right);
        if (res[1] >= threshold) return res[0];
        return -1;
    }
}
```

* 上述代码能满足题目条件，但是会超时。仔细分析可以发现查询方法实际上仍然每次遍历数组，

## 性能分析



## 官方解题

