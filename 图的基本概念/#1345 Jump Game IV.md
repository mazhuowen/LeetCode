[toc]

Given an array of integers `arr`, you are initially positioned at the first index of the array.

In one step you can jump from index `i` to index:

* `i + 1` where: $i + 1 < \text{arr.length}$.
* `i - 1` where: $i - 1 \ge 0$.
* `j` where: $\text{arr[i]} == \text{arr[j]}$ and $i \ne j$.

Return the minimum number of steps to reach the **last index** of the array.

Notice that you can not jump outside of the array at any time.



**Constraints:**

- $1 \le \text{arr.length} \le 5 * 10^4$
- $-10^8 \le \text{arr[i]} \le 10^8$



## 题目解读

&emsp;从起始索引开始跳，可以向前向后，也可以跳到相同值的索引，求到达末尾的最少步数。

```java
class Solution {
    public int minJumps(int[] arr) {

    }
}
```

## 程序设计

* 最基本的思路是广度优先搜索，但是当数组所有值相等时会超时。

```java
class Solution {
    public int minJumps(int[] arr) {
        if (arr == null || arr.length == 0) return 0;

        Map<Integer, List<Integer>> idxMap = new HashMap<>();
        for (int i = 0; i < arr.length; i++) {
            if (idxMap.get(arr[i]) == null) idxMap.put(arr[i], new ArrayList<>());
            idxMap.get(arr[i]).add(i);
        }

        int level = 0;
        Set<Integer> idxRecord = new HashSet<>();
        Queue<Integer> queue = new LinkedList<>();
        // 加入起始点
        queue.add(0);
        idxRecord.add(0);
        // 加入边界
        idxRecord.add(-1);
        idxRecord.add(arr.length);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int curIdx = queue.poll();
                // 到达终点
                if (curIdx == arr.length - 1) return level;
                // 加入i-1、i+1
                if (!idxRecord.contains(curIdx - 1)) {
                    queue.add(curIdx - 1);
                    idxRecord.add(curIdx - 1);
                }
                if (!idxRecord.contains(curIdx + 1)) {
                    queue.add(curIdx + 1);
                    idxRecord.add(curIdx + 1);
                }
                // 加入相同值索引
                if (idxMap.get(arr[curIdx]) != null) {
                    for (int nextIdx : idxMap.get(arr[curIdx])) {
                        if (nextIdx == curIdx || idxRecord.contains(nextIdx)) continue;
                        queue.add(nextIdx);
                        idxRecord.add(nextIdx);
                    }
                }
            }
            level++;
        }
        throw new RuntimeException("error logic");
    }
}
```

* 可以提前处理数组，将连续相等的数字压缩为两个。

```java
class Solution {
    public int minJumps(int[] arr) {
        if (arr == null || arr.length == 0) return 0;

        // 正对连续超长数组，将连续相等的索引压缩
        arr = dataProcess(arr);
        
        Map<Integer, List<Integer>> idxMap = new HashMap<>();
        for (int i = 0; i < arr.length; i++) {
            if (idxMap.get(arr[i]) == null) idxMap.put(arr[i], new ArrayList<>());
            idxMap.get(arr[i]).add(i);
        }

        int level = 0;
        Set<Integer> idxRecord = new HashSet<>();
        Queue<Integer> queue = new LinkedList<>();
        // 加入起始点
        queue.add(0);
        idxRecord.add(0);
        // 加入边界
        idxRecord.add(-1);
        idxRecord.add(arr.length);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int curIdx = queue.poll();
                // 到达终点
                if (curIdx == arr.length - 1) return level;
                // 加入i-1、i+1
                if (!idxRecord.contains(curIdx - 1)) {
                    queue.add(curIdx - 1);
                    idxRecord.add(curIdx - 1);
                }
                if (!idxRecord.contains(curIdx + 1)) {
                    queue.add(curIdx + 1);
                    idxRecord.add(curIdx + 1);
                }
                // 加入相同值索引
                if (idxMap.get(arr[curIdx]) != null) {
                    for (int nextIdx : idxMap.get(arr[curIdx])) {
                        if (nextIdx == curIdx || idxRecord.contains(nextIdx)) continue;
                        queue.add(nextIdx);
                        idxRecord.add(nextIdx);
                    }
                }
            }
            level++;
        }
        throw new RuntimeException("error logic");
    }

    // 连续相等的数字只保留两个（不是一个）
    private int[] dataProcess(int[] arr) {
        Integer pre = null, prePre = null;
        int idx = 0;
        for (int i = 0; i < arr.length; i++) {
            if (pre != null && prePre != null && pre == prePre && pre == arr[i]) continue;
            arr[idx++] = arr[i];
            prePre = pre;
            pre = arr[i];
        }
        return Arrays.copyOf(arr, idx);
    }
}
```

## 性能分析

&emsp;&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：76ms，在所有java提交中击败了38.82%的用户。

内存消耗：58.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区时间性能较好的方法是`idxMap`遇到连续数值，则只记录起始和结束索引，省去转换时间；其次在每次遍历入队完后删除`idxMap`中的键值对。