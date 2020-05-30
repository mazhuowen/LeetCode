[toc]

In an exam room, there are `N` seats in a single row, numbered `0`, `1`, `2`, ..., `N-1`.

When a student enters the room, they must sit in the seat that maximizes the distance to the closest person.  If there are multiple such seats, they sit in the seat with the lowest number.  (Also, if no one is in the room, then the student sits at seat number 0.)

Return a class `ExamRoom(int N)` that exposes two functions: `ExamRoom.seat()` returning an `int` representing what seat the student sat in, and `ExamRoom.leave(int p)` representing that the student in seat number `p` now leaves the room.  It is guaranteed that any calls to `ExamRoom.leave(p)` have a student sitting in seat `p`.



Note:

* $1 \le N \le 10^9$
* `ExamRoom.seat()` and `ExamRoom.leave()` will be called at most $10^4$ times across all test cases.
* Calls to `ExamRoom.leave(p)` are guaranteed to have a student currently sitting in seat number `p`.



## 题目解读

&emsp;设计数据结构，模拟考生选座，优先选择距离相邻考生最远的座位，如果存在多个，选择最考前的座位。

```java
class ExamRoom {

    public ExamRoom(int N) {

    }
    
    public int seat() {

    }
    
    public void leave(int p) {

    }
}

/**
 * Your ExamRoom object will be instantiated and called as such:
 * ExamRoom obj = new ExamRoom(N);
 * int param_1 = obj.seat();
 * obj.leave(p);
 */
```

## 程序设计

* 使用`TreeSet`记录当前空余的座位区间，根据长度排序，如果长度相等，则区间较前的权重大。
* 使用哈希表记录空余区间，方便切分和合并。

```java
class ExamRoom {
    int N;
    Map<Integer, Integer> start;
    Map<Integer, Integer> end;
    TreeSet<int[]> seat;

    public ExamRoom(int N) {
        this.N = N;
        this.start = new HashMap<>();
        this.end = new HashMap<>();
        // 根据长度排序，长度相等则选择区间在前面的
        this.seat = new TreeSet<>((a, b) -> distance(a) == distance(b) ? b[0] - a[0] : distance(a) - distance(b));
        // 加入整个区间(-1, N)，表示中间0~N-1的座位
        add(new int[]{-1, N});
    }
    
    public int seat() {
        // 获取最大距离
        int[] interval = seat.last();
        // 座位满了
        if (interval[0] + 1 >= interval[1]) return -1;
        int seatNo = getSeat(interval);
        // 划分区间
        remove(interval);
        add(new int[]{interval[0], seatNo});
        add(new int[]{seatNo, interval[1]});
        
        return seatNo;
    }
    
    public void leave(int p) {
        // 删除小区间，加入合并后的区间
        int s = end.get(p), e = start.get(p);
        remove(new int[]{s, p});
        remove(new int[]{p, e});
        add(new int[]{s, e});
    }

    // 求区间最大距离
    private int distance(int[] interval) {
        if (interval[0] == -1) return interval[1];
        else if (interval[1] == N) return N - 1 - interval[0];
        else return (interval[1] - interval[0]) / 2;
    }

    // 获取座位
    private int getSeat(int[] interval) {
        if (interval[0] == -1) return 0;
        else if (interval[1] == N) return N - 1;
        else return (interval[1] + interval[0]) / 2;
    }

    private void remove(int[] interval) {
        seat.remove(interval);
        start.remove(interval[0]);
        end.remove(interval[1]);
    }

    private void add(int[] interval) {
        seat.add(interval);
        start.put(interval[0], interval[1]);
        end.put(interval[1], interval[0]);
    }
}
```

## 性能分析

&emsp;&emsp;两个方法时间复杂度都为$O(\log_2N)$，空间复杂度为$O(N)$。

执行用时：19ms，在所有java提交中击败了88.51%的用户。

内存消耗：40.2MB，在所有java提交中击败了75.00%的用户。

## 官方解题

&emsp;官方思路使用`TreeSet`来保存已经选择的座位号，每次选座都需要遍历比较，时间复杂度为$O(N\log_2N)$。