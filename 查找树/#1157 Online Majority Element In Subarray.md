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

* 由于是区间相关，且涉及到大量查询，首先想到的是线段树。线段树结构如下，包含更新和查询，根据题意，给定阈值超过区间一半，使用线段树记录区间超过一半的数字，查询得到这个数再跟阈值做比较。
* 更新从底向上，每次插入叶节点并更新路径上的点。如果左、右区间包含超过其区间一半的值，则在右、左区间遍历查找该值并计数，最后若超过当前区间的一半就更新。如果一个值不超过子区间的一半，其在父区间必然不超过一半，故只需对超过子区间一半的值统计。
* 查询根据区间切分并查到区间统计值，合并阶段同更新，需要根据左右区间的超过一半的数统计右、左区间，与更新不同的是，统计遍历多了一个条件：当前子区间超过一半的数目加上另一子区间可遍历数目（即另一子区间不超过一半的数目）不大于当前整个区间的一半，则不予遍历。

```java
class Tree {
    // 数组区间
    int start, end;
    // 超过数组一半的数和数目，如果存在必然只有一个，不存在则计数为0
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
        // 更新当前结点，如果左右区间值相等，直接更新
        if (left().count != 0 && right().count != 0 && left().num == right().num) {
            num = left().num;
            count = left().count + right().count;
            return;
        }
        // 左右区间值不相等，遍历查找
        // 左结点存在大于一半区间，在右区间查找该值并计数叠加
        if (left().count != 0) {
            int c = left().count;
            for (int i = right().start; i <= right().end; i++) {
                if (left().num == arr[i]) c++;
            }
            // 更新
            if (c > count && c > (end - start + 1) / 2) {
                num = left().num;
                count = c;
            }
        }

        // 同理，在左区间查找
        if (right().count != 0) {
            int c = right().count;
            for (int i = left().start; i <= left().end; i++) {
                if (right().num == arr[i]) c++;
            }
            // 更新
            if (c > count && c > (end - start + 1) / 2) {
                num = right().num;
                count = c;
            }
        }
    }

    public int[] query(int s, int e) {
        // 不包含区间
        if (s > e) {
            return null;
        }
        // 包含树的区间
        if (start >= s && end <= e) {
            return new int[]{num, count};
        }
        int mid = getMid();
        int[] l = null, r = null;
        // 有区间查找
        if (mid < s) r = right().query(s, e);
        // 左区间查找
        else if (mid >= e) l = left().query(s, e);
        // 两个分开查找区间
        else {
            l = left().query(s, mid);
            r = right().query(mid + 1, e);
        }

        // 不相交，返回另一个
        if (l == null || r == null) return l == null ? r : l;

        // 左右区间都没有超过半数的
        if (l[1] == -1 && r[1] == -1) {
            return new int[]{0, -1};
        }
        // 左右区间的超半数的数字相同，结果相加返回
        if (l[1] != -1 && r[1] != -1 && l[0] == r[0]) {
            return new int[]{l[0], l[1] + r[1]};
        }
        int lStart = Math.max(s, start);
        int rEnd = Math.min(e, end);
        int[] res = new int[]{0, -1};
        // 左区间有值，在右区间查找，需注意查找还有一个条件就是右区间可查找的数目加上当前左区间超过一半的数目要大于当前区间一半，否则不满足要求，不查
        if (l[1] != -1 && l[1] + rEnd - mid - r[1] > (rEnd - lStart + 1) / 2) {
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
        if (r[1] != -1 && r[1] + rEnd - lStart - l[1] > (rEnd - lStart + 1) / 2) {
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

* 上述思路会超时，如果查询区间不是完整的树节点区间，每次要拆分查询，合并结果，导致需要遍历统计性能较低。如果能避免直接遍历数组，将大大增加性能。参考社区思路，采用额外空间记录每个数出现在数组中的索引，采用二分查找查找左右边界代替遍历数组。
* 其次为了方便计算，不存在超过区间一半的数的计数从`-1`改为`0`。修改后代码如下：

```java
class Tree {
    // 数组区间
    int start, end;
    // 超过数组一半的数和数目，如果存在必然只有一个，不存在则计数为0
    int num;
    int count;
    // 左右子区间
    Tree left, right;

    // 记录数组
    int[] arr;

    Tree(int start, int end, int[] arr) {
        this.start = start;
        this.end = end;
        this.arr = arr;
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

    // 返回大于（等于，若flag为true）target的第一个值
    public int binarySearch(int target, List<Integer> idxList, boolean flag) {
        int l = 0, r = idxList.size();
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (idxList.get(mid) > target || (flag && idxList.get(mid) == target)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
    // 统计区间内target出现的次数
    private int count(int target, int s, int e, ArrayList<Integer>[] idxMap) {
        // 区间不合法
        if (s > e) return 0;
        // 获取索引数组
        List<Integer> idxList = idxMap[target];
        // 左侧第一个索引（左边界）
        int l = binarySearch(s, idxList, true);
        // 区间s～e不存在target的数字
        if (l == idxList.size() || idxList.get(l) > e) return 0;

        // 右侧最后一个索引（查找到的是大于e的第一个索引，小于等于e的就是前一个）（右边界）
        int r = binarySearch(e, idxList, false) - 1;
        // 个数就是右侧索引减去左侧索引
        return r - l + 1;
    }

    public void update(int idx, ArrayList<Integer>[] idxMap) {
        // 递归终止条件
        if (start > idx || end < idx) return;
        // 找到位置，该区间只有一个值就是本身，更新计数为1
        if (start == idx && end == idx) {
            num = arr[idx];
            count = 1;
            return;
        }
        // 递归更新
        left().update(idx, idxMap);
        right().update(idx, idxMap);

        // 更新当前结点，如果左右区间值相等，直接更新
        if (left().count != 0 && right().count != 0 && left().num == right().num) {
            num = left().num;
            count = left().count + right().count;
            return;
        }
        // 左右区间值不相等，遍历查找
        // 左结点存在大于一半区间，在右区间查找该值并计数叠加
        if (left().count != 0) {
            int c = left().count;
            c += count(left().num, getMid() + 1, end, idxMap);
            // 更新
            if (c > count && c > (end - start + 1) / 2) {
                num = left().num;
                count = c;
            }
        }

        // 同理，在左区间查找
        if (right().count != 0) {
            int c = right().count;
            c += count(right().num, start, getMid(), idxMap);
            // 更新
            if (c > count && c > (end - start + 1) / 2) {
                num = right().num;
                count = c;
            }
        }
    }

    public int[] query(int s, int e, ArrayList<Integer>[] idxMap) {
        // 区间不合法，返回
        if (s > e) {
            return null;
        }
        // 包含树的区间，返回
        if (start >= s && end <= e) {
            return new int[]{num, count};
        }
        int mid = getMid();
        int[] l = null, r = null;
        // 右区间查找
        if (mid < s) r = right().query(s, e, idxMap);
        // 左区间查找
        else if (mid >= e) l = left().query(s, e, idxMap);
        // 两个区间分开查找
        else {
            l = left().query(s, mid, idxMap);
            r = right().query(mid + 1, e, idxMap);
        }

        // 不相交，返回另一个
        if (l == null || r == null) return l == null ? r : l;

        // 左右区间都没有超过半数的，当前区间不可能有半数的，直接返回
        if (l[1] == 0 && r[1] == 0) {
            return new int[]{0, 0};
        }
        // 左右区间的超半数的数字相同，结果相加，直接返回
        if (l[1] > 0 && r[1] > 0 && l[0] == r[0]) {
            return new int[]{l[0], l[1] + r[1]};
        }

        int[] res = new int[]{0, 0};
        // 左区间有值，在右区间查找
        // 需注意查找还有一个条件就是右区间可查找的数目加上当前左区间超过一半的数目要大于当前区间一半，
        // 即l[1] + e - s - r[1] > (e - s + 1) / 2，否则不满足要求，不查，简化为l[1] - r[1] > (s - e + 1) / 2
        if (l[1] > 0 && l[1] - r[1] > (s - e + 1) / 2) {
            int c = l[1];
            c += count(l[0], mid + 1, e, idxMap);
            // 更新
            if (c > res[1] && c > (e - s + 1) / 2) {
                res[0] = l[0];
                res[1] = c;
            }
        }
        // 同理，在左区间查找
        if (r[1] != 0 && r[1] - l[1] > (s - e + 1) / 2) {
            int c = r[1];
            c += count(r[0], s, mid, idxMap);
            // 更新
            if (c > res[1] && c > (e - s + 1) / 2) {
                res[0] = r[0];
                res[1] = c;
            }
        }
        return res;
    }
}
```

* 主体增加索引记录。

```java
class MajorityChecker {
    // 线段树
    private Tree root;
    // 记录索引
    private final ArrayList<Integer>[] idxMap;

    public MajorityChecker(int[] arr) {
        if (arr == null || arr.length < 1) throw new IllegalArgumentException("invalid param");

        this.root = new Tree(0, arr.length - 1, arr);
        this.idxMap = new ArrayList[20_001];

        for (int i = 0; i < arr.length; i++) {
            if (idxMap[arr[i]] == null) {
                idxMap[arr[i]] = new ArrayList<>();
            }
            // 更新索引
            idxMap[arr[i]].add(i);
            // 更新树节点
            root.update(i, idxMap);
        }
    }

    public int query(int left, int right, int threshold) {
        int[] res = root.query(left, right, idxMap);
        // 查询到超过区间一半的数（不存在为0），和阈值作对比
        if (res[1] >= threshold) return res[0];
        return -1;
    }
}
```

## 性能分析

&emsp;构造函数时间复杂度为$O(N\log_2N)$，查询最坏需要遍历到叶节点，花费$O(\log_2N)$，每次合并需要二分查找，总的时间复杂度为$O((\log_2N)^2)$；空间复杂度记录索引花费$O(2N)$，树节点内部存储数组花费$O(N^2)$，总的花费为$O(N^2)$，如果把树节点内部的数组作为公共变量，可优化为$O(N)$。



## 官方解题

&emsp;暂无，密切关注。

