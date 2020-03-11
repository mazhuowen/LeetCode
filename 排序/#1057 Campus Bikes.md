[toc]

On a campus represented as a 2D grid, there are `N` workers and `M` bikes, with $N \le M$. Each worker and bike is a 2D coordinate on this grid.

Our goal is to assign a bike to each worker. Among the available bikes and workers, we choose the (worker, bike) pair with the shortest Manhattan distance between each other, and assign the bike to that worker. (If there are multiple (worker, bike) pairs with the same shortest Manhattan distance, we choose the pair with the smallest worker index; if there are multiple ways to do that, we choose the pair with the smallest bike index). We repeat this process until there are no available workers.

The Manhattan distance between two points `p1` and `p2` is $Manhattan(p_1, p_2) = \vert p_1.x - p_2.x\vert + \vert p_1.y - p_2.y\vert$.

Return a vector `ans` of length `N`, where `ans[i]` is the index (0-indexed) of the bike that the `i`-th worker is assigned to.

Note:

* $0 \le \text{workers[i][j], bikes[i][j]} < 1000$
* All worker and bike locations are distinct.
* $1 \le workers.length \le bikes.length \le 1000$



## 题目解读

&emsp;计算工人和自行车的最近距离。

```java
class Solution {
    public int[] assignBikes(int[][] workers, int[][] bikes) {

    }
}
```

## 程序设计

&emsp;最简单的思路是计算所有距离然后排序。

```java
class Solution {
    public int[] assignBikes(int[][] workers, int[][] bikes) {
        if(workers == null || workers.length == 0) {
            return new int[]{};
        }
        if(bikes == null || workers.length > bikes.length) {
            throw new IllegalArgumentException("invalid param");
        }
        // 是否已分配标识
        boolean[] workFlag = new boolean[workers.length];
        boolean[] bikeFlag = new boolean[bikes.length];
        // 距离数组
        Integer[][] distance = new Integer[workers.length * bikes.length][3];
        for (int i = 0; i < workers.length; i++) {
            for (int j = 0; j < bikes.length; j++) {
                int dis = Math.abs(workers[i][0] - bikes[j][0])
                        + Math.abs(workers[i][1] - bikes[j][1]);
                distance[i * bikes.length + j] = new Integer[]{i, j, dis};
            }
        }
        // 排序
        Arrays.sort(distance, (a, b) -> {
            // 距离相等
            if(a[2] == b[2]) {
                // 个人序号小的或自行车序号小的在前面
                return a[0] == b[0] ? a[1] - b[1] : a[0] - b[0];
            } else {
                return a[2] - b[2];
            }
        });
        // 遍历
        int[] res = new int[workers.length];
        for(int i = 0; i < distance.length; i++) {
            Integer[] cur = distance[i];
            // 已被分配
            if(workFlag[cur[0]] || bikeFlag[cur[1]]) {
                continue;
            }
            workFlag[cur[0]] = true;
            bikeFlag[cur[1]] = true;
            res[cur[0]] = cur[1];
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM\log_2NM)$，空间复杂度为$O(NM)$。

执行用时：503ms，在所有java提交中击败了65.22%的用户。

内存消耗：153.4MB，在所有java提交中击败了6.66%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较快的方法采用字典映射的方式。由于题目中限定距离小于等于2000，可以采用2000的的数组表示距离，其次由于遍历行列计算距离，如果距离已经相等，则计算的先后顺序就是按照要求：工人id小的在前，一样则自行车id小的在前。所以可以在上述数组中维护一个链表，遍历计算后插入即可。这样利用字典映射和链表解决了排序的花销。

```java
class Solution {
    public int[] assignBikes(int[][] workers, int[][] bikes) {
        if(workers == null || workers.length == 0) {
            return new int[]{};
        }
        if(bikes == null || workers.length > bikes.length) {
            throw new IllegalArgumentException("invalid param");
        }
        // 最大曼哈顿距离小于2000，可以使用2000的数组映射
        List<int[]>[] distance = new List[2000];
        // 遍历计算
        for (int i = 0; i < workers.length; i++) {
            for (int j = 0; j < bikes.length; j++) {
                int dis = Math.abs(workers[i][0] - bikes[j][0])
                        + Math.abs(workers[i][1] - bikes[j][1]);
                if(distance[dis] == null) {
                    distance[dis] = new LinkedList<>();
                }
                // 距离相同，遍历计算的顺序就是有序的，直接加入
                distance[dis].add(new int[]{i, j});
            }
        }
        // 是否已分配标识
        boolean[] workFlag = new boolean[workers.length];
        boolean[] bikeFlag = new boolean[bikes.length];

        int[] res = new int[workers.length];
        for(int i = 0; i < distance.length; i++) {
            if(distance[i] == null) continue;
            for(int[] temp : distance[i]) {
                // 未分配
                if(!workFlag[temp[0]] && !bikeFlag[temp[1]]) {
                    res[temp[0]] = temp[1];
                    // 设置分配标识
                    workFlag[temp[0]] = true;
                    bikeFlag[temp[1]] = true;
                }
            }
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行耗时：93ms，击败了92.75%的java用户。

内存消耗：101.9MB，击败了13.33%的java用户。