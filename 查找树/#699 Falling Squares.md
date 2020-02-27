[toc]

n an infinite number line (x-axis), we drop given squares in the order they are given.

The `i`-th square dropped (`positions[i] = (left, side_length)`) is a square with the left-most point being `positions[i][0]` and sidelength `positions[i][1]`.

The square is dropped with the bottom edge parallel to the number line, and from a higher height than all currently landed squares. We wait for each square to stick before dropping the next.

The squares are infinitely sticky on their bottom edge, and will remain fixed to any positive length surface they touch (either the number line or another square). Squares dropped adjacent to each other will not stick together prematurely.


Return a list `ans` of heights. Each height `ans[i]` represents the current highest height of any square we have dropped, after dropping squares represented by `positions[0]`, `positions[1]`, ..., `positions[i]`.



Note:

* $1 \le \text{positions.length} \le 1000$.
* $1 \le \text{positions[i][0]} \le 10^8$.
* $1 \le \text{positions[i][1]} \le 10^6$.



## 题目解读

&emsp;给定数组为正方形序列，假设正方形按照数组中的索引顺序放置正方形，每个正方形数组中第一个值代表了左边界值，第二个值代表了边长。正方形可以叠加到其它正方形上。返回一个堆叠高度列表，每个堆叠高度表示目前最高高度。

```java
class Solution {
    public List<Integer> fallingSquares(int[][] positions) {

    }
}
```

## 程序设计

* 以每一轮正方形的视角看，我们可以记录每一轮正方形堆叠后本轮正方形的高度。这样问题就转化为求解每轮及之前轮正方形高度的最大值。

```java
class Solution {
    public List<Integer> fallingSquares(int[][] positions) {
        List<Integer> res = new LinkedList<>();
        if(positions.length == 0) {
            return res;
        }
        // 计算每一轮正方形堆叠后的最大高度
        int[] height = new int[positions.length];
        for(int i = 0; i < positions.length; i++) {
            int left = positions[i][0];
            int size = positions[i][1];
            int right = left + size;
            // 当前最大高度更新（如果不是0，则是之前轮正方形和本轮正方形有交叉，之前轮的高度，本次叠加）
            height[i] += size;
            // 还需更新之后的轮次
            for(int j = i + 1; j < positions.length; j++) {
                int left1 = positions[j][0];
                int size1 = positions[j][1];
                int right1 = left1 + size1;
                // 和后续轮次的正方形没有交叉，不做处理
                if(right1 <= left || left1 >= right) {

                } 
                // 有交叉，将当前高度和之前更新的高度作对比，选择高的那个
                // 相当于在j轮正方形下有前面几轮的不想交的正方形，此时叠加只能叠加在最高的正方形上，此处相当于更新基础高度，在j轮直接叠加j轮正方形高度即可
                else {
                    height[j] = Math.max(height[j], height[i]);
                }
            }
        }
        // height维护了每一轮正方形叠加后的高度，最后需统计每一轮的最高高度
        int max = 0;
        for(int high : height) {
            max = Math.max(max, high);
            res.add(max);
        }
        return res;
    }
}
```

* 上述思路从每一轮的正方形出发，还可以从坐标轴出发，可以使用数组模拟坐标。由于最多1000轮，而我们关心的只有正方形左右边界高度（因为其它地方高度一致），这样我们只需要关心2000个坐标。为了映射这些不连续且不有序的坐标，可以引入字典记录，先遍历数组将坐标排序后分配连续的数组索引。

```java
class Solution {
    public List<Integer> fallingSquares(int[][] positions) {
        List<Integer> res = new LinkedList<>();
        if(positions.length == 0) {
            return res;
        }
        int[] height = new int[2 * positions.length];
        // 记录正方形边界和数组索引的关系
        Map<Integer, Integer> index = new HashMap<>();
        List<Integer> boun = new LinkedList<>();
        for(int i = 0; i < positions.length; i++) {
            boun.add(positions[i][0]);
            // 重要，为了避免边界叠加，故减一
            boun.add(positions[i][0] + positions[i][1] - 1);
        }
        // 排序
        Collections.sort(boun);
        // 关联连续索引
        int t = 0;
        for(int num : boun) {
            // 不用关注重复值，只要维护数组中索引递增和实际边界递增一致即可
            index.put(num, t++);
        }
        // 暴力计算更新
        int max = 0; // 记录本轮最大值
        for(int i = 0; i < positions.length; i++) {
            int left = positions[i][0];
            int size = positions[i][1];
            // 为了避免边界叠加，故减一
            int right = left + size - 1;
            // 查询区域内的最大高度
            int cur = query(height, index.get(left), index.get(right)) + size;
            // 更新区域内高度
            update(height, index.get(left), index.get(right), cur);
            max = Math.max(max, cur);
            res.add(max);
        }
        return res;
    }

    // 返回区间最大值
    private int query(int[] height, int left, int right) {
        int max = 0;
        // 更新
        for(int i = left; i <= right; i++) {
            max = Math.max(max, height[i]);
        }
        return max;
    }

    private void update(int[] height, int left, int right, int high) {
        // 更新
        for(int i = left; i <= right; i++) {
            height[i] = high;
        }
    }
}
```

## 性能分析

&emsp;基础做法时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：27ms，在所有java提交中击败了82.35%的用户。

内存消耗：41.3MB，在所有java提交中击败了25.00%的用户。

&emsp;坐标暴力法时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：23ms，在所有java提交中击败了91.18%的用户。

内存消耗：42.5MB，在所有java提交中击败了22.50%的用户。

## 官方解题

&emsp;官方提供了线段树的思路。首先将坐标压缩为连续，这样就可以使用数组实现的线段树。注意到此题线段树是动态修改的，鉴于数组的实现方式如果每次都修改到叶节点，性能会降低；可以使用一个数组保存父节点修改的值，这样不必修改子节点，等查询的时候再做修改。其次需要注意查询和修改都要对边界路径做操作。

```java
class Tree {
    // 叶节点索引
    int idx;
    // 记录层数（用于从头至尾更新特定路径）
    int h;
    // 线段树
    int[] tree;
    // 记录非叶节点延迟更新子节点值
    int[] lazy;

    Tree(int n) {
        this.idx = n;
        this.tree = new int[2 * n];
        this.lazy = new int[n];
        // 计算高度（2^h >= n），此处高度计算方式叶节点为0
        this.h = 0;
        while(1 << h < idx) {
            h++;
        }
    }

    // 更新区间值
    public void update(int left, int right, int val) {
        if(left < 0 || right >= idx || left > right) {
            return;
        }
        left += idx;
        right += idx;
        int tempL = left, tempR = right;
        while(left <= right) {
            if(left % 2 == 1) {
                tree[left] = Math.max(tree[left], val);
                // 表示更新的是父节点，子节点未更新，则保存到lazy数组
                if(left < idx) {
                    lazy[left] = Math.max(lazy[left], val);
                }
                left++;
            }
            if(right % 2 == 0) {
                tree[right] = Math.max(tree[right], val);
                // 表示更新的是父节点，子节点未更新，则保存到lazy数组
                if(left < idx) {
                    lazy[right] = Math.max(lazy[right], val);
                }
                right--;
            }
            left /= 2;
            right /= 2;
        }
        // 上述操作极有可能更新了内部结点，但没有更新到根节点及两条边界路径，还需要更新
        // 左路径
         while (tempL > 1) {
            tempL /= 2;
            // 根据子节点更新父节点
            tree[tempL] = Math.max(tree[tempL * 2], tree[tempL * 2 + 1]);
            // 根据lazy更新结点（但不更新子节点，只更新当前结点）
            tree[tempL] = Math.max(tree[tempL], lazy[tempL]);
        }
        // 右路径
        while (tempR > 1) {
            tempR /= 2;
            // 根据子节点更新父节点
            tree[tempR] = Math.max(tree[tempR * 2], tree[tempR * 2 + 1]);
            // 根据lazy更新结点（但不更新子节点，只更新当前结点）
            tree[tempR] = Math.max(tree[tempR], lazy[tempR]);
        }
    }

    public int query(int left, int right) {
        if(left < 0 || right >= idx || left > right) {
            return 0;
        }
        left += idx;
        right += idx;
        // 从lazy更新left和right路径，update只更新父节点和路径结点，子节点未更新
        // 区间查询不必更新全部子节点，更新两条路径结点即可（由于父节点是最新的信息，故从顶到尾更新）
        for(int i = h; i > 0; i--) {
            int leftP = left >> i;
            if(lazy[leftP] > 0) {
                tree[2 * leftP] = Math.max(lazy[leftP], tree[2 * leftP]);
                tree[2 * leftP + 1] = Math.max(lazy[leftP], tree[2 * leftP + 1]);
                // 将当前lazy值更新到子结点lazy中，方便迭代更新
                if(2 * leftP < idx)
                    lazy[2 * leftP] = Math.max(lazy[2 * leftP], lazy[leftP]);
                if(2 * leftP + 1 < idx)
                    lazy[2 * leftP + 1] = Math.max(lazy[2 * leftP + 1], lazy[leftP]);
                lazy[leftP] = 0;
            }
        }
        for(int i = h; i > 0; i--) {
            int rightP = right >> i;
            if(lazy[rightP] > 0) {
                tree[2 * rightP] = Math.max(lazy[rightP], tree[2 * rightP]);
                tree[2 * rightP + 1] = Math.max(lazy[rightP], tree[2 * rightP + 1]);
                // 将当前lazy值更新到子结点lazy中，方便迭代更新
                if(2 * rightP < idx)
                    lazy[2 * rightP] = Math.max(lazy[2 * rightP], lazy[rightP]);
                if(2 * rightP + 1 < idx)
                    lazy[2 * rightP + 1] = Math.max(lazy[2 * rightP + 1], lazy[rightP]);
                lazy[rightP] = 0;
            }
        }
        int max = 0;
        while(left <= right) {
            if(left % 2 == 1) {
                max = Math.max(max, tree[left]);
                left++;
            }
            if(right % 2 == 0) {
                max = Math.max(max, tree[right]);
                right--;
            }
            left /= 2;
            right /= 2;
        }
        return max;
    }
}
```

在得到线段树后，操作原理如下：

```java
class Solution {
    public List<Integer> fallingSquares(int[][] positions) {
        // 边界坐标去重
        Set<Integer> coords = new HashSet<>();
        for (int[] pos: positions) {
            coords.add(pos[0]);
            coords.add(pos[0] + pos[1] - 1);
        }
        // 排序
        List<Integer> sortedCoords = new ArrayList<>(coords);
        Collections.sort(sortedCoords);
        // 维护离散边界和连续索引关系
        Map<Integer, Integer> index = new HashMap<>();
        int t = 0;
        for (int coord: sortedCoords) {
            index.put(coord, t++);
        }
		// 构建线段树
        Tree tree = new Tree(sortedCoords.size());
        int best = 0;
        List<Integer> ans = new ArrayList<>();
		// 动态修改查询
        for (int[] pos: positions) {
            int L = index.get(pos[0]);
            int R = index.get(pos[0] + pos[1] - 1);
            int h = tree.query(L, R) + pos[1];
            tree.update(L, R, h);
            best = Math.max(best, h);
            ans.add(best);
        }
        return ans;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：29ms，在所有java提交中击败了76.47%的用户。

内存消耗：42MB，在所有java提交中击败了22.50%的用户。